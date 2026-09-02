---
title: "Handling of redirects"
source: confluence
space: "MS — Marketing Site"
page_id: "1115979777"
url: "https://jiracation.atlassian.net/wiki/spaces/MS/pages/1115979777"
parent: "Projects"
author: "Truls Paulsen"
status: "current"
version: 1
last_modified: "2026-06-24T15:08:21.981Z"
exported: "2026-09-02"
---

# Handling of redirects

# Implementation spec — redirect rules in routing 

**Epic:** Site refactoring and design system **Goal:** Replace the structural redirect _documents_ with code _rules_, so routine deletions (courses, categories) and old URL patterns are handled automatically — without creating a redirect document each time. Documents are NOT deleted in this task; rules and documents coexist until Task 3 verifies and Task 4 deletes.

---

## Architecture: two layers, split by whether a rule needs data

Some rules are pure string transforms (no CMS lookup). Some need to know whether a document exists. Put each where it's cheapest and safest:

* **Middleware** (`middleware.ts`) — runs at the edge before rendering. Handles the **data-free string rules** (manual map lookup, `/no/` strip, blog normalization). Cheap, runs first.
* **Route components** (`page.tsx`) — run during render and already fetch the document. Handle the **existence-based fallbacks** (course missing, category missing). The "does it exist?" check is the same fetch you do to render — free.

**Why not** `next.config` **redirects?** Those run _before_ middleware and can't be overridden, which would break the "manual exceptions win first" rule. Keeping everything in middleware + routes gives one readable precedence chain.

---

## Precedence (the canonical order — do not reorder)

1. **Manual redirect document** — if one exists for this path, it wins. _(middleware)_
2. `/no/` **prefix strip** — `/no/<path>` → `/<path>`. _(middleware)_
3. **Blog normalization** — `/blogg/<kat>/<slug>` → `/blog/<slug>`; bare `/blogg` → `/blog`. _(middleware)_
4. **Course-deletion fallback** — missing course → its category (if alive) else `/kurs`. _(route)_
5. **Category-deletion fallback** — missing category → `/kurs`. _(route)_

All redirects are **permanent (308)**. 308 is the modern equivalent of 301 — Google treats it as permanent and passes equity. Use it because deletions are intended to be final.

---

## Layer 1 — `middleware.ts`

```
import {NextResponse, type NextRequest} from 'next/server'
import {getManualRedirect} from '@/lib/redirects' // cached lookup, see note below

export async function middleware(request: NextRequest) {
  const {pathname} = request.nextUrl
  const perm = (to: string) =>
    NextResponse.redirect(new URL(to, request.url), 308)

  // 1. Manual exceptions win first (Sanity-managed, cached map)
  const manual = await getManualRedirect(pathname)
  if (manual) return perm(manual)

  // 2. /no/ prefix strip
  if (pathname.startsWith('/no/')) return perm(pathname.slice(3)) // "/no/x" -> "/x"
  if (pathname === '/no') return perm('/')

  // 3. Blog normalization: /blogg/<kat>/<slug> -> /blog/<slug> ; /blogg -> /blog
  if (pathname.startsWith('/blogg/')) {
    const slug = pathname.split('/').pop()           // last segment
    return perm(slug ? `/blog/${slug}` : '/blog')
  }
  if (pathname === '/blogg') return perm('/blog')

  return NextResponse.next()
}

export const config = {
  // Skip assets/api so the middleware only sees page requests
  matcher: ['/((?!_next|api|.*\\..*).*)'],
}
```

`getManualRedirect` **contract:** it looks up the manual redirect documents by `oldUrl` and returns the `newUrl` or `null`. It **must be cached** — do not hit Sanity uncached on every request. Implement with the team's caching approach (cached fetch with short revalidation, or a periodically-refreshed in-memory map). Confirm the caching mechanism before building; this is the one piece with an implementation choice.

> Note: if the blog target post no longer exists, the visitor lands on `/blog/<slug>` which 404s — that's handled by the blog route's own not-found (fall back to `/blog`). Don't try to validate post existence in middleware.

---

## Layer 2 — route fallbacks

### Course route — `app/kurs/[kategori]/[kurs]/page.tsx`

```
import {permanentRedirect} from 'next/navigation'
import {getCourse, categoryExists} from '@/lib/queries'

export default async function CoursePage({params}) {
  const {kategori, kurs} = await params

  // Fetch INSIDE try/catch — but never redirect from the catch (see error-safety)
  let course
  try {
    course = await getCourse(kurs)
  } catch (err) {
    throw err // CMS/network error -> surface it, do NOT redirect a possibly-live page
  }

  if (course === null) {
    // Confirmed deleted/never-existed. Bounce to category IF it's alive, else course index.
    // Checking the category first prevents a real-time A->B->C chain when the
    // category was also deleted.
    const catAlive = await categoryExists(kategori)
    permanentRedirect(catAlive ? `/kurs/${kategori}` : '/kurs') // 308
  }

  return <Course data={course} />
}
```

### Category route — `app/kurs/[kategori]/page.tsx`

```
import {permanentRedirect} from 'next/navigation'
import {getCategory} from '@/lib/queries'

export default async function CategoryPage({params}) {
  const {kategori} = await params

  let category
  try {
    category = await getCategory(kategori)
  } catch (err) {
    throw err // error -> surface, do NOT redirect
  }

  if (category === null) {
    permanentRedirect('/kurs') // 308
  }

  return <Category data={category} />
}
```

---

## Error-safety contract (the part that protects live pages)

A redirect must fire **only on a confirmed not-found**, never on a CMS/network error. If a fetch fails during a Sanity blip and the code treats that as "missing → redirect," you'd send a _live_ course's visitors away. So:

1. **The data layer returns** `null` **only for a confirmed not-found, and** `throws` **on any error.** `getCourse` / `getCategory` / `categoryExists` must NOT swallow errors and return null. This is a hard requirement on `@/lib/queries`.
2. **Never call** `permanentRedirect()` **inside a** `try` **block.** Next's `redirect()`/`permanentRedirect()` work by _throwing_ a special internal signal — a surrounding `catch` would swallow it and the redirect silently wouldn't happen. Do the fetch in `try/catch`; call the redirect _after_, outside the block. (This is the most common Next.js redirect bug — call it out in code review.)

---

## Known bounded edge case

A `/no/`-prefixed URL pointing at a course whose category is _also_ deleted can take two hops: `/no/kurs/dead-kat/dead-kurs` → (strip) → `/kurs/dead-kat/dead-kurs` → (fallback) → `/kurs`. That's A→B→C, but **bounded at 2 and vanishingly rare** (a deleted course, under a deleted category, behind an old locale prefix). Acceptable — do not add complexity to chase it. Anything beyond 2 hops is a bug.

---

## Test checklist

- [ ] Manual exception URL → its hand-picked target (manual wins over every pattern)
- [ ] `/no/kurs/lederkurs` → `/kurs/lederkurs`, single 308
- [ ] `/blogg/lederkurs/<slug>` → `/blog/<slug>`, single 308
- [ ] Live course renders (no redirect)
- [ ] Deleted course, category alive → `/kurs/<kategori>`, single hop
- [ ] Deleted course, category also deleted → `/kurs`, single hop (no chain)
- [ ] Deleted category → `/kurs`, single 308
- [ ]  **Simulated CMS error on a live course → error surfaced, NOT redirected**
- [ ] All redirects return 308 (permanent), not 307

---

## Out of scope / confirm before building

* Caching mechanism for `getManualRedirect` — confirm with tech lead.
* No redirect documents are deleted in this task.
* The `@/lib/queries` error/null contract may need adjusting in existing fetch helpers — check they throw on error rather than returning null.
