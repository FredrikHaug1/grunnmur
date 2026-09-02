# Component structure — atomic design

> **This file has no source in `atlassian/`.** No Confluence doc mentions atomic
> design, layer names, or import direction. It is drafted from the instruction that
> "the component structure follows atomic design" and is awaiting approval. Every
> section below is a proposal, not a derived rule. Correct it, then delete this
> banner.

## The four layers

| Layer | What qualifies | Lives at |
| --- | --- | --- |
| **Atom** | A single-purpose UI primitive. No business logic, no data fetching, no knowledge of Sanity. `Text`, `Button`, `Icon`, `Link`, `Divider` | `apps/web/src/components/atoms/<Name>/` |
| **Molecule** | A small composition of atoms with one job. Takes **plain props** — never a Sanity document shape. `Card`, `SearchInput`, `ExpertChip` | `apps/web/src/components/molecules/<Name>/` |
| **Organism** | A page section, self-contained and meaningful on its own. May be a Server Component. May receive Sanity-shaped data. `Hero`, `CourseGrid`, `Menu`, `Footer` | `apps/web/src/components/organisms/<Name>/` |
| **Template** | A page-type layout composing organisms. Holds no content of its own. `LandingPage`, `BlogPost`, `CoursePage` | `apps/web/app/_templates/` |

## Import direction — strictly downward

An import may only point at a **lower** layer, never sideways across layers and never
upward.

```
templates → organisms → molecules → atoms
```

| From | May import |
| --- | --- |
| Atom | Nothing from any other layer |
| Molecule | Atoms |
| Organism | Molecules, atoms |
| Template | Organisms, molecules, atoms |

An atom that needs a molecule is a sign the atom is really a molecule. Fix the
classification, do not add the import.

## Paths

- **Cross-layer** — always absolute: `@/components/atoms/Button`
- **Same layer, sibling** — relative: `'../CourseCard'`
- **Never** `'../../'` to escape a folder. If you need it, the component is in the
  wrong layer.

## File layout

Every component is a folder with a barrel:

```
src/components/
├── atoms/
│   └── Button/
│       ├── Button.tsx      ← named export, no default
│       └── index.ts        ← export {Button} from './Button'
├── molecules/
│   └── Card/               ← same shape
└── organisms/
    └── Hero/               ← same shape
```

Each layer has its own `index.ts` re-exporting its components.

## Exports

**Named exports only. No default exports anywhere in the component tree.** A default
export makes the symbol renameable at every import site, which defeats grep and makes
layer violations invisible in review.

## Client boundaries

`'use client'` is the first line of the file when required, and is pushed **as far
down the tree as possible**. An organism that needs interactivity in one corner
extracts that corner rather than marking the whole section client-side.

## Where non-UI things go

Script injection, SEO metadata emitters, and provider wrappers (`JsonLd`,
`PreviewProvider`, analytics bootstrapping) are **not** atomic layers. They live in
`apps/web/src/components/` at the top level, outside `atoms/`, `molecules/` and
`organisms/`.

> TODO(decision needed): confirm templates belong in `app/_templates/` rather than a
> fifth `src/components/templates/` layer.

> TODO(decision needed): do organisms fetch their own data (each a Server Component
> with its own query), or receive all data as props from the template? This decides
> whether the data layer is centralised per page or distributed per section, and it
> is the single biggest architectural choice in this file.

> TODO(decision needed): is there a `pages` layer above templates, or does the App
> Router route file fill that role?

## Source

None. Drafted from the user's instruction that the component structure follows
atomic design. **Not derived from `atlassian/`.**
