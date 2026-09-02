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
4. **Catalogue removal** — `/kurs` and everything under it → `/` *(middleware)*
5. **Blog post not-found** — a missing post falls back to `/blog` *(route)*

**All redirects are permanent 308.** Not 301, not 307. 308 is the modern permanent
redirect; Google treats it as permanent and passes equity.

## Which layer handles what

Split by whether the rule needs data:

- **Middleware** (`middleware.ts`) — data-free string rules: manual map lookup,
  `/no/` strip, blog normalisation, catalogue removal. Runs at the edge, before
  rendering. Cheap, runs first.
- **Route components** (`page.tsx`) — existence-based fallbacks. The "does it exist?"
  check is the same fetch used to render, so it is free.

## Catalogue removal

The `/kurs` branch does not exist on the new site — see
[`seo-and-content.md`](seo-and-content.md). Every URL under it redirects to the
homepage:

```ts
// 4. Catalogue removal — the whole branch is gone
if (pathname === '/kurs' || pathname.startsWith('/kurs/')) return perm('/')
```

This is a **pure string rule in middleware**, not a route fallback. There is nothing
to look up: no course exists, no category exists, so there is no "does it exist?"
question to ask. That removes the two most error-prone rules the old site had.

Because it sits *after* the manual-document check, a specific old course URL worth
sending somewhere better than the homepage can still be given its own redirect
document — the blanket rule is the floor, not a ceiling.

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
any error.** A fetch helper must never swallow an error and return null. A Sanity blip
treated as "missing → redirect" sends a *live* page's visitors away.

**2. Never call `permanentRedirect()` inside a `try` block.** Next's
`redirect()`/`permanentRedirect()` work by *throwing* an internal signal — a
surrounding `catch` swallows it and the redirect silently does not happen. Do the
fetch inside `try/catch`; call the redirect *after*, outside the block.

This is the most common Next.js redirect bug. Call it out in review.

```tsx
let post
try {
  post = await getPost(slug)
} catch (err) {
  throw err // CMS/network error -> surface it, do NOT redirect a possibly-live page
}

if (post === null) {
  permanentRedirect('/blog') // 308 — confirmed not-found only
}
```

The catalogue rule needs none of this: it is a string match in middleware with nothing
to fetch, so it cannot misfire on a CMS error. Every *remaining* existence-based
fallback must follow the contract above.

## Manual redirect lookup must be cached

`getManualRedirect(pathname)` looks up manual redirect documents by `oldUrl` and
returns `newUrl` or `null`. **It must be cached** — never hit Sanity uncached on
every request.

**Use a cached fetch, tagged, invalidated by webhook.** The same revalidation path the
rest of the content uses:

```ts
// read
cacheTag('redirects')

// on a redirect document change, via the Sanity webhook
revalidateTag('redirects')
```

One caching mechanism for the whole app, consistent across every serverless instance,
and an editor's new redirect goes live in seconds. Do **not** use a module-scope
in-memory map — each instance would hold its own copy and disagree during refresh — and
do **not** bake the map in at build, because deploys are manual and the editor would
see nothing happen.

## Known bounded edge case

A `/no/`-prefixed catalogue URL takes two hops: `/no/kurs/x/y` → `/kurs/x/y` → `/`.
Bounded at 2. **Do not add complexity to chase it.** Anything beyond 2 hops is a bug.

## Blog target that no longer exists

If the blog target post is gone the visitor lands on `/blog/<slug>`, which 404s and
falls back to `/blog` via the blog route's own not-found. **Do not validate post
existence in middleware.**

## Test checklist

- [ ] Manual exception URL → its hand-picked target (manual beats every pattern)
- [ ] `/no/blog/<slug>` → `/blog/<slug>`, single 308
- [ ] `/blogg/lederkurs/<slug>` → `/blog/<slug>`, single 308
- [ ] `/kurs` → `/`, single 308
- [ ] `/kurs/ledelse/lederkurs` → `/`, single 308
- [ ] `/no/kurs/ledelse/lederkurs` → `/`, bounded at 2 hops
- [ ] A catalogue URL with its own redirect document → that document's target, not `/`
- [ ] Missing blog post → `/blog`, single 308
- [ ] **Simulated CMS error on a live page → error surfaced, NOT redirected**
- [ ] All redirects return 308, not 307

## Source

- [`atlassian/projects/handling-of-redirects.md`](../../atlassian/projects/handling-of-redirects.md) (entire document)
- [`atlassian/research/technical-seo-site-structure-visitor-journey.md`](../../atlassian/research/technical-seo-site-structure-visitor-journey.md) §8 (index
  hygiene: 101 404s, 161 redirects, 4 redirect errors)
