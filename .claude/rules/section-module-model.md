# Section and module model

How a page is assembled. Folder layout and import direction are in
[`component-structure.md`](component-structure.md).

A `page` is nothing but an ordered list of **sections**. There are no hard-coded hero
fields, no fixed intro, no per-page theme. Everything visible comes from sections.

## The split

**A section is a layout box.** It owns spacing, background, width, and the direction
its modules flow in. **It does not own content.**

**A module is content.** It sits inside a section and owns what is on the page —
a heading, an image, a CTA, a testimonial.

That split is the whole point: "a title, an image and a CTA side by side" and "the same
three stacked" are the same section with one field flipped.

## Section shape

```ts
section {
  label: string        // internal name, shown in the Studio array preview
  anchorId?: string    // slugified, for in-page links
  hidden: boolean      // default false, filtered out in the GROQ query
  layout: sectionLayout
  appearance: sectionAppearance
  modules: array of [ ...module types ]
}
```

**`sectionLayout`** — `width` (`full` | `wide` | `default` | `narrow`) ·
`paddingTop` / `paddingBottom` (`none` | `xs` | `sm` | `md` | `lg` | `xl`) ·
`direction` (`vertical` | `horizontal`) · `columns` (`1`–`4`, only when horizontal) ·
`gap` (`none` | `sm` | `md` | `lg` | `xl`) · `align` · `justify` · `reverseOnMobile`.

`default` width is the 1200px max content width. Section padding never resolves below
the 24px minimum gap between sections.

**`sectionAppearance`** — `background` (`none` | `white` | `offWhite` | `coolLight` |
`dark`) · `backgroundImage` + `overlay` · `theme` (`auto` | `light` | `dark`) ·
`divider` · `radius`.

Backgrounds resolve to the palette in [`design-tokens.md`](design-tokens.md):
`white` `#FFFFFF` · `offWhite` `#FAFAFA` · `coolLight` `#F5F7F9` · `dark` `#171717`.
Dark sections are used **sparingly** — footer or a strong hero.

`theme` passes down through React context so modules read text and button colours
without each one re-deriving them from the background.

## Module shape

Every module carries its own content fields plus:

```ts
moduleLayout {
  span: 'auto' | '1' | '2' | '3' | '4' | 'full'
  alignSelf: 'inherit' | 'start' | 'center' | 'end'
  hidden: boolean
}
```

## Four things a module must never do

1. **Contain a section.** Nesting is one way only.
2. **Define its own outer vertical padding.** Spacing between modules is the section's
   `gap`. A module that adds its own margin breaks the rhythm the section controls.
3. **Declare its own background colour.** Backgrounds belong to sections. Card
   surfaces are the exception and they come from the theme.
4. **Assume it is alone.** The same module renders in a 1-column vertical section and
   a 4-column horizontal one.

## Adding to the library

Add a **token variant** first. Only create a new `_type` when the *content shape*
genuinely differs — a new visual treatment of the same fields is a variant, not a type.

## Hidden is filtered in GROQ

`hidden != true` is filtered in the query, on both sections and modules — never in
React. Shipping hidden content to the client and hiding it with CSS leaks unpublished
copy into the HTML.

## The launch library is five modules

`titleModule` · `richTextModule` · `imageModule` · `ctaModule` · `spacerModule`

That is enough to rebuild one real page end to end, which is the actual gate. **Do not
expand the library until one page has proved the section/module model.** If the model
is wrong, everything built on it inherits the mistake, and migrating a large library
twice is the expensive outcome.

Expected second wave, once the model holds: `cardsModule`, `testimonialModule`,
`logoWallModule`, `statsModule` — these cover the proof requirements in `03 §11` and
the homepage order in `06 §5.1`.

## Source

- Repo-owner decision, 2026-09-02: adopt the section/module structure, layered on
  `atlassian/` for all values.
- `atlassian/04-design-system.md` §5 (1200px max width, 24px minimum section gap,
  dark used sparingly, background alternation), §1 (palette)
- `atlassian/06-page-briefs.md` §5 (per-page-type section order the model must express)
