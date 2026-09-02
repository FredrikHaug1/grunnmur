# GROQ conventions

> **Source note:** `atlassian/` states no GROQ conventions. Everything here comes from
> explicit decisions taken by the repo owner on 2026-09-02.

## Types come from Sanity TypeGen

Write plain GROQ inside `defineQuery`, then generate types from the deployed schema.

```ts
import {defineQuery} from 'next-sanity'

export const PAGE_QUERY = defineQuery(`
  *[_type == "page" && slug.current == $path][0]{
    title,
    body,
  }
`)
```

**Do not hand-write a parallel Zod schema for query responses.** TypeGen derives
types from the schema itself, so they cannot drift from it — a hand-maintained
second definition can and will. Zod stays available for genuinely untrusted input
(form payloads, webhook bodies), not for typing our own CMS reads.

Regenerate types whenever the schema changes. A query whose types are stale is a
build problem, not a runtime one — keep it that way.

## Every query is a `defineQuery` constant

Never inline a GROQ string at the call site. TypeGen only sees queries wrapped in
`defineQuery`, so an inline string is silently untyped.

## Path resolution

Slugs hold the full path, so resolution is one query with no joins and no per-market
filter:

```groq
*[_type == "page" && slug.current == $path][0]
```

Never filter on `i18n.country` or `language` — [`sanity-schema.md`](sanity-schema.md)
defines this as a Norway-only project and those fields do not exist.

## Projections are explicit

Always project the fields you need. Never return a bare document:

```groq
// Do
*[_type == "course" && slug.current == $path][0]{
  title,
  "slug": slug.current,
  expert->{name, image},
}

// Don't
*[_type == "course" && slug.current == $path][0]
```

An unbounded projection ships every draft-era field and every future field to the
client, and makes it impossible to see what a component actually depends on.

## Fragments

Repeated projections live as named fragments and are composed into queries, so a
field added to a course appears everywhere a course is rendered:

```ts
const courseFields = `
  title,
  "slug": slug.current,
  expert->{name, image},
`
```

## Client configuration

**`useCdn: true` in production, `false` in preview.**

```ts
createClient({
  useCdn: !isPreview,
  perspective: isPreview ? 'drafts' : 'published',
})
```

Published traffic goes through the CDN. Preview and draft reads bypass it so editors
see changes immediately — the Deployment QA step in
[`qa-and-launch.md`](qa-and-launch.md) requires content to render correctly in the
Vercel preview before merge, which a stale CDN read would defeat.

Consequence to remember when debugging: there are **two** cache layers in production
— Sanity's CDN and the app's own cache. Content that looks stale in production but
fresh in preview is the CDN, not the app.

## Redirect lookups must be cached

`getManualRedirect` runs on every request through middleware. Never hit Sanity
uncached from it. See [`redirects.md`](redirects.md).

## Source

- Repo-owner decisions, 2026-09-02 (TypeGen, `useCdn`, slug resolution, datasets)
- `atlassian/projects/handling-of-redirects.md` (cached redirect lookup requirement)
- `atlassian/10-qa-and-launch.md` §3.9 (preview must render current content)
