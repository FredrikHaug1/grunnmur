# Redirects

Routine deletions are handled by **code rules**, not by hand-written redirect
documents. A whole class of URLs gets one rule; fifty documents are untestable.

**Existing redirect documents are not deleted.** Rules and documents coexist until
the rules are verified against the document set, and only then are superseded
documents removed. Adding the rules does not authorise a cleanup.

## Precedence — do not reorder

1. **Manual redirect document** — if one exists for this path, it wins *(middleware)*
2. **`/no/` prefix strip** — `/no/<path>` → `/<path>`, and bare `/no` → `/` *(middleware)*
3. **Blog normalisation** — `/blogg/<kat>/<slug>` → `/blog/<slug>`; `/blogg` → `/blog` *(middleware)*
4. **Course-deletion fallback** — missing course → its category if alive, else `/kurs` *(route)*
5. **Category-deletion fallback** — missing category → `/kurs` *(route)*

**All redirects are permanent 308.** Not 301, not 307. 308 is the modern permanent
redirect; Google treats it as permanent and passes equity.

## Which layer handles what

Split by whether the rule needs data:

- **Middleware** (`middleware.ts`) — data-free string rules: manual map lookup,
  `/no/` strip, blog normalisation. Runs at the edge, before rendering. Cheap, runs first.
- **Route components** (`page.tsx`) — existence-based fallbacks. The "does it exist?"
  check is the same fetch used to render, so it is free.

**Never use `next.config` redirects for this.** They run *before* middleware and
cannot be overridden, which breaks the "manual exceptions win first" rule.

The middleware matcher skips assets and API routes so it only sees page requests:

```ts
export const config = {
  matcher: ['/((?!_next|api|.*\\..*).*)'],
}
```

## The error-safety contract

This is the part that protects live pages. A redirect fires **only on a confirmed
not-found**, never on a CMS or network error.

**1. The data layer returns `null` only for a confirmed not-found, and `throws` on
any error.** `getCourse`, `getCategory` and `categoryExists` must not swallow errors
and return null. A Sanity blip treated as "missing → redirect" sends a *live*
course's visitors away.

**2. Never call `permanentRedirect()` inside a `try` block.** Next's
`redirect()`/`permanentRedirect()` work by *throwing* an internal signal — a
surrounding `catch` swallows it and the redirect silently does not happen. Do the
fetch inside `try/catch`; call the redirect *after*, outside the block.

This is the most common Next.js redirect bug. Call it out in review.

```tsx
let course
try {
  course = await getCourse(kurs)
} catch (err) {
  throw err // CMS/network error -> surface it, do NOT redirect a possibly-live page
}

if (course === null) {
  // Check the category first — prevents an A->B->C chain when it was also deleted.
  const catAlive = await categoryExists(kategori)
  permanentRedirect(catAlive ? `/kurs/${kategori}` : '/kurs') // 308
}
```

## Manual redirect lookup must be cached

`getManualRedirect(pathname)` looks up manual redirect documents by `oldUrl` and
returns `newUrl` or `null`. **It must be cached** — never hit Sanity uncached on
every request. Use a cached fetch with short revalidation, or a periodically
refreshed in-memory map.

> TODO(decision needed): the caching mechanism for `getManualRedirect` is explicitly
> left open in the spec — "confirm the caching mechanism before building."

## Known bounded edge case

A `/no/`-prefixed URL pointing at a course whose category is *also* deleted takes two
hops: `/no/kurs/dead-kat/dead-kurs` → `/kurs/dead-kat/dead-kurs` → `/kurs`. Bounded
at 2 and vanishingly rare. **Do not add complexity to chase it.** Anything beyond 2
hops is a bug.

## Blog target that no longer exists

If the blog target post is gone the visitor lands on `/blog/<slug>`, which 404s and
falls back to `/blog` via the blog route's own not-found. **Do not validate post
existence in middleware.**

## Test checklist

- [ ] Manual exception URL → its hand-picked target (manual beats every pattern)
- [ ] `/no/kurs/lederkurs` → `/kurs/lederkurs`, single 308
- [ ] `/blogg/lederkurs/<slug>` → `/blog/<slug>`, single 308
- [ ] Live course renders, no redirect
- [ ] Deleted course, category alive → `/kurs/<kategori>`, single hop
- [ ] Deleted course, category also deleted → `/kurs`, single hop, no chain
- [ ] Deleted category → `/kurs`, single 308
- [ ] **Simulated CMS error on a live course → error surfaced, NOT redirected**
- [ ] All redirects return 308, not 307

## Source

- `atlassian/projects/handling-of-redirects.md` (entire document)
- `atlassian/research/technical-seo-site-structure-visitor-journey.md` §8 (index
  hygiene: 101 404s, 161 redirects, 4 redirect errors)
