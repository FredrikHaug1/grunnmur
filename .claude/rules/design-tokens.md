# Design tokens

Colour and type for videocation.no. These values are the spec — where any existing
code disagrees, the code is wrong.

## Colour

Nordic and restrained. White and near-white dominate. Accent guides the eye; it is
not decoration.

### Primary

| Name | Hex | Variable | Use |
| --- | --- | --- | --- |
| White | `#FFFFFF` | `--color-white` | Main background |
| Black | `#171717` | `--color-black` | Headlines, emphasised text |
| Black Overlay | `#171717` @ 65% | `--color-overlay` | Image overlays for contrast |

### Accent

| Name | Hex | Variable | Use |
| --- | --- | --- | --- |
| Nordic Sun | `#FFE577` | `--color-sun` | Primary CTA buttons, important interactive elements |
| Nordic Sun Light | `#FFEA8F` | `--color-sun-light` | Hover state on white backgrounds |
| Nordic Fjord | `#00A7B5` | `--color-fjord` | Non-interactive weight: icons, key figures, badges |

**Nordic Sun Light is `#FFEA8F`, not `#FFD100`.** `04 §1.6.1`, `§4.1.1` and `§4.2` all
say `#FFD100`; they are wrong. `#FFEA8F` is a genuine light tint of the base, so hover
lifts rather than deepens and the token name stays honest. The variable is
`--color-sun-light` — `04 §1.2` truncates it to `--color-sun-ligh`, which is a typo.

### Neutral

| Name | Hex | Variable | Use |
| --- | --- | --- | --- |
| Off-white | `#FAFAFA` | `--color-off-white` | Subtle background for depth |
| Light Gray | `#F3F3F3` | `--color-gray-light` | Card background, section alternative |
| Medium Gray Light | `#E8E8E8` | `--color-gray-border` | Borders, dividers |
| Medium Gray | `#737373` | `--color-gray-mid` | Secondary text, icons |
| Dark Gray | `#404040` | `--color-gray-dark` | Primary body text |

### Interface

`#F5F7F9` `--color-cool-light` (secondary sections, card backgrounds) ·
`#EDF2F7` `--color-cool` (hover, active) ·
`#F7F6F4` `--color-warm-light` (alternative sections) ·
`#F2F0ED` `--color-warm` (secondary hover, subtle contrast)

### Data visualisation

`#E6C4A1` `--color-data-1` Nordic Terra · `#C4D4B0` `--color-data-2` Nordic Pine ·
`#B0CCD9` `--color-data-3` Nordic Ice · `#D9B0B0` `--color-data-4` Nordic Berry ·
`#C4B8D9` `--color-data-5` Nordic Heather

`04 §1.5` calls `--color-data-3` "Nordic Fjord", colliding with the accent colour of
the same name. **The data swatch is renamed Nordic Ice**; the accent keeps the name.

### Applying colour

- Primary CTA background is Nordic Sun. Never another colour.
- **Nordic Sun is interactive-only.** Never decoration, never a background wash.
- **Nordic Fjord is the informational accent** — icons, key figures, badges. It is
  exempt from the interactive-only rule. `04 §1.6.1`'s blanket "reserve accent colours
  for interactive elements" was written with Nordic Sun in mind; `04 §1.2` defines
  Fjord as "visual weight for non-interactive elements", which is the narrower and
  correct reading.
- Body text is Dark Gray `#404040`; secondary text Medium Gray `#737373`;
  headlines Black `#171717`.
- Links are Dark Gray, with Nordic Sun Light on hover.
- Backgrounds: White for main content, Cool/Warm Gray variants for secondary
  sections, Black Overlay over images.

## Typography

Two fonts. No more. Fraunces carries identity and authority. DM Sans carries
everything else.

```css
@import url('https://fonts.googleapis.com/css2?family=Fraunces:opsz,wght,WONK@9..144,100..900,0..1&family=DM+Sans:opsz,wght@9..40,100..900&display=swap');
```

| Element | Font | Size | Weight | Line height | Fraunces settings |
| --- | --- | --- | --- | --- | --- |
| H1 — Hero | Fraunces | 48px / 3rem | 600 | 1.1 | `opsz 48, wght 600, WONK 0.75` |
| H2 — Section | Fraunces | 32px / 2rem | 500 | 1.2 | `opsz 32, wght 500, WONK 0.5` |
| H3 — Subsection | Fraunces | 24px / 1.5rem | 500 | 1.25 | `opsz 24, wght 500, WONK 0.25` |
| H4 — Small heading | DM Sans | 20px / 1.25rem | 600 | 1.4 | — |
| Body primary | DM Sans | 16px / 1rem | 400 | 1.75 | — |
| Body secondary | DM Sans | 14px / 0.875rem | 400 | 1.7 | — |
| CTA buttons | DM Sans | 16px / 1rem | 600 | 1.5 | — |
| Navigation | DM Sans | 14px / 0.875rem | 400 | 1.5 | — |
| Meta text | DM Sans | 12px / 0.75rem | 400 | 1.5 | — |

Fraunces WONK axis: 0.7–1.0 on large display headings, reduced toward 0 on H3/H4.
Reduce further on mobile if Fraunces feels restless at small sizes.

## Capitalisation

Headlines sentence case · Navigation lowercase · Buttons sentence case ·
Meta labels lowercase · Course titles sentence case · Expert names natural case ·
Acronyms UPPERCASE.

## Typographic constraints

- Minimum contrast ratio 4.5:1 (WCAG AA).
- Add emphasis with weight, not size. Create hierarchy with size and weight, not colour.
- Maximum two fonts per page. Never a third.
- Minimum body size on mobile is 14px.
- Avoid text on images. Use Black Overlay where unavoidable.

## Source

- `atlassian/04-design-system.md` §1 (colour), §2 (typography), §3 (typography guidelines)
