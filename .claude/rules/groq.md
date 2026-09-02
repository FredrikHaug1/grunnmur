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

## Client configuration — `useCdn` is always `true`

**Nothing in the request path may set `useCdn: false`.** This is not a preference.
A previous build set it to `false` globally, bypassed the CDN entirely and caused an
**API usage spike**. That is the reason the rule exists.

There are exactly **two** clients, and never a third:

```ts
// client.ts — all public rendering
createClient({
  useCdn: true,
  perspective: 'published',
  // no token
})
```

```ts
// live.ts — drafts and preview
export const {sanityFetch, SanityLive} = defineLive({
  client,
  serverToken: process.env.SANITY_API_READ_TOKEN,
})
```

**Draft and preview reads go through the Live Content API, not through a
`useCdn: false` client.** `defineLive()` from `next-sanity` handles the draft
perspective and live updates; that is what gives editors immediate feedback, which is
what the Deployment QA step in [`qa-and-launch.md`](qa-and-launch.md) requires.

If you find yourself wanting fresher data than the CDN gives, the answer is a cache
tag and `revalidateTag`, not a second client with the CDN switched off.

## Stega must be stripped before it reaches anything that parses

Draft reads carry stega-encoded metadata inside string values. Run `stegaClean` before
any value reaches a URL, a `srcset`, a date parser, or a `className` — invisible
characters in a class name silently break styling, and in a URL silently break the
link.

## Redirect lookups must be cached

`getManualRedirect` runs on every request through middleware. Never hit Sanity
uncached from it. See [`redirects.md`](redirects.md).

## Source

- Repo-owner decisions, 2026-09-02 (TypeGen, slug resolution, datasets, and the
  always-`true` `useCdn` rule adopted from the API-spike incident)
- [`atlassian/projects/handling-of-redirects.md`](../../atlassian/projects/handling-of-redirects.md) (cached redirect lookup requirement)
- [`atlassian/10-qa-and-launch.md`](../../atlassian/10-qa-and-launch.md) §3.9 (preview must render current content)
