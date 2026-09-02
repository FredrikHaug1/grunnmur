# Sanity schema conventions

> **Source note:** `atlassian/` states no Sanity conventions. Everything here comes
> from explicit decisions taken by the repo owner on 2026-09-02. Those decisions are
> the source. Change them here when they change.

## Naming

**camelCase for document types and for fields.** One convention everywhere.

```ts
defineType({
  name: 'landingPage',
  fields: [
    {name: 'heroImage', type: 'image'},
    {name: 'publishedAt', type: 'datetime'},
  ],
})
```

Never kebab-case a type name, never PascalCase one. Type and field names are
**English**, like everything else in the codebase — only the content inside them is
Norwegian.

## Norway only

**This is a Norwegian site. There is no second market and no translation layer.**

- No `language` or `country` fields on documents.
- No per-market filtering in GROQ.
- One `settings` document, not one per market.

Every Confluence doc describes videocation.no and nothing else. Do not add a
translation layer speculatively — carrying an unused one through every query costs
something on every read, forever.

## Slugs carry the full path

`slug.current` holds the **complete path**, not a leaf segment:

```ts
slug: {current: 'kurs/ledelse/lederkurs'}
```

Any URL resolves in one query — `*[slug.current == $path][0]` — which is what the
redirect fallback logic in [`redirects.md`](redirects.md) needs to answer "does this
exist?" cheaply. Editors are responsible for the whole path being correct.

## Site settings is a singleton

One `settings` document with a **fixed `_id` of `settings`**. Locked to a single
instance in the Studio structure; editors cannot create a second one and cannot
delete it. Navigation and footer live here, not in code — `05 §3` treats top-level
navigation as a strategic decision, so marketing must be able to change it without a
deploy.

## Portable Text

Rich, with custom marks and embedded blocks:

| Kind | Allowed |
| --- | --- |
| Block styles | `normal`, `h2`, `h3`, `h4`, `blockquote` |
| Lists | bullet, numbered |
| Decorators | `strong`, `em` |
| Annotations | `link` (external), `internalLink` (reference to a document) |
| Embedded blocks | `image`, `muxVideo`, `ctaBlock` |

`internalLink` is a **reference**, never a typed-in URL string. It is what makes the
internal-linking requirement in [`seo-and-content.md`](seo-and-content.md)
enforceable — a referenced target cannot silently 404 when a page is renamed.

**No `h1` in Portable Text.** Every page has exactly one H1 and it comes from the
page title, not the body.

## Datasets

**`prod` and `dev`.** Not `production` / `development`.

This diverges from Sanity's defaults, so error messages and tutorials will say
otherwise. It is deliberate.

## Validation

The `redirect` document type requires a `type` field. Note the limit: **Sanity
validation runs in the Studio only.** It blocks publish in the UI; it does **not**
block a write through `@sanity/client` or the HTTP API. A script or migration can
still create an invalid document. If automation starts writing documents, that gap
needs closing separately.

## Source

- Repo-owner decisions, 2026-09-02 (naming, Norway-only scope, slugs, settings,
  Portable Text, datasets)
- [`atlassian/05-site-structure-and-user-journeys.md`](../../atlassian/05-site-structure-and-user-journeys.md) §3 (navigation is
  marketing-owned, hence CMS-managed)
- [`atlassian/projects/handling-of-redirects.md`](../../atlassian/projects/handling-of-redirects.md) (path resolution needs, redirect
  document shape)
- [`atlassian/10-qa-and-launch.md`](../../atlassian/10-qa-and-launch.md) §3.5 (one H1 per page)
