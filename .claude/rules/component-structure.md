# Component structure

> **Source note:** `atlassian/` states nothing about component structure. This layout
> was chosen by the repo owner on 2026-09-02. That decision is the source. It replaces
> the earlier atomic-design proposal, which was never sourced and is now deleted.

## Three layers, by what they know about

| Layer | Knows about | Lives at |
| --- | --- | --- |
| **Component** | Nothing about Sanity. Plain props, plain types. Design-system primitives: `Button`, `Text`, `Container`, `Icon`. | `frontend/components/` |
| **Module** | One module's content shape. Renders content. | `frontend/modules/` |
| **Section** | Layout and appearance. Owns spacing, background, width, flow direction. Renders modules. | `frontend/sections/` |

**A component that imports a Sanity type is in the wrong layer.** The boundary is not
"is it small" — it is whether the thing knows where its data came from. Keeping
`components/` Sanity-free is what lets it be tested, reused and restyled without the
CMS in the loop.

## Import direction

```
sections → modules → components
```

Sections may import modules and components. Modules may import components. Components
import neither. Nothing imports upward.

## Dispatchers

Sections and modules are each dispatched by `_type` through a single list component —
`SectionList` and `ModuleList`. Registering a new type means adding it to its
dispatcher; a type that renders nowhere is not done.

**Every dispatcher needs an unknown-type fallback:** render nothing in production, and
a visible warning in development. A missing case must never crash a live page, and
must never be silent locally.

## Studio side

Schema mirrors the same split:

```
studio/src/schemaTypes/
├── documents/   # page, settings, redirect, post, ...
├── sections/    # layout shells
├── modules/     # content
└── objects/     # shared: link, seo, sectionLayout, sectionAppearance
```

Shared field groups live in `objects/` and are composed into section and module
schemas. Redefining the same field inline in two schemas is a defect.

## Exports

**Named exports only. No default exports.** A default export is renameable at every
import site, which defeats grep and makes a layer violation invisible in review.

## Client boundaries

`'use client'` is the first line when required, and is pushed as far down as possible.
A section needing interactivity in one corner extracts that corner rather than marking
the whole section client-side.

## Not UI

Script injection, JSON-LD emitters, consent wrappers and analytics bootstrapping are
not components, modules or sections. They live outside these three folders.

## Tokens map to classes through static lookup objects

**Never construct a Tailwind class dynamically.** This is forbidden:

```tsx
className={`pt-${padding}`}   // the compiler cannot see it — the class is never generated
```

Every token maps through an explicit object:

```ts
const PADDING_TOP: Record<Padding, string> = {
  none: 'pt-0',  xs: 'pt-4',  sm: 'pt-8',
  md: 'pt-16',   lg: 'pt-24', xl: 'pt-32',
}
```

Two consequences, both deliberate: Tailwind can see every class it will ever need to
generate, and **a token that exists in the Studio with no entry in the frontend map
fails type-check**. That failure is the feature — it is what stops an editor picking a
value that renders as nothing.

Token vocabularies are shared between the Studio and the frontend from one place, so
adding a token is one edit. The Studio builds its `options.list` from the same arrays
the frontend maps.

**Every editor-facing control is a token** — never a free-text or free-number field.
An editor typing `37px` into a padding field is a bug in the schema, not in the CSS.

## Source

- Repo-owner decision, 2026-09-02 (three-layer split, folder layout, token discipline)
- `atlassian/` defines no component structure. Token *values* come from
  [`design-tokens.md`](design-tokens.md), which is sourced from `04`.
