# Design — components, layout, imagery

Applies to every UI surface on videocation.no. Tokens live in
[`design-tokens.md`](design-tokens.md).

## Buttons

### Primary CTA

- Background Nordic Sun `#FFE577`
- Text Black `#171717`
- DM Sans 600, 16px
- Border-radius 20px (pill)
- Padding 10px 24px
- Hover: Nordic Sun Light `#FFEA8F`

### Secondary

- Background transparent
- Border 1px solid `#E8E8E8`
- Text Dark Gray `#404040`
- DM Sans 500, 16px
- Border-radius 20px
- Hover: Light Gray `#F3F3F3` background

## Links

Dark Gray `#404040`, no underline by default. Hover adds underline plus Nordic Sun
Light. Visited is Medium Gray `#737373`.

## Navigation

DM Sans Regular, 14px, **lowercase**. Active state is a Nordic Sun underline or
background. Mobile is a hamburger menu with full overlay.

## Layout and spacing

Nordic minimalism. Space is not waste — it is the message.

- Minimum 24px between sections.
- Max content width 1200px, centred.
- Primary background White. Alternate sections between White and
  Off-white / Cool Gray Light.
- Black `#171717` sections are used sparingly — footer or a strong hero only.
- Remove the unnecessary. Soft, low-contrast compositions.

## Cards

- Background Light Gray `#F3F3F3` or Off-white `#FAFAFA`
- Border-radius 12px
- Padding 24px
- No high-contrast borders — subtle grey dividers only

## Icons

- Line iconography, never filled.
- 20px inline, 24px standalone.
- Nordic Fjord `#00A7B5` for decorative/informational use. This is deliberate — Fjord
  is the informational accent and is exempt from the interactive-only rule that binds
  Nordic Sun. See [`design-tokens.md`](design-tokens.md).
- Dark Gray `#404040` for functional use.

## Photography

Real people in real situations. No stock-photo stiffness.

**Do**

- Natural light, direct or indirect
- Warm natural tones: soft whites, beige, light wood
- Open, airy spaces
- Real people, real settings
- Soft contrast adjustment
- Simple layered clothing in muted tones, minimal accessories
- Natural, functional props with organic shapes
- Natural materials where possible

**Don't**

- Oversaturate
- Use very dark backgrounds
- Overlay text on images where it can be avoided
- Use generic stock: smiling people around laptops, handshakes, abstract business
  meetings, generic office photography, random "people learning" images, overused
  SaaS illustrations

Every image must do at least one of four jobs: explain the solution, prove we can
deliver, humanise the brand, or make us memorable. An image doing none of these is
removed.

## Logo

**The logo is never reconstructed in CSS or HTML. Always use the original image file.**

Approved backgrounds: `#FFFFFF`, `#FAFAFA`, `#F3F3F3`, `#171717` (inverted logo).

Never: change logo colours · place elements in the clear space · compress, stretch
or distort · add shadows, glows or effects · place on low-contrast backgrounds ·
rotate or crop.

## Motion

Motion must explain something. Good: showing a learning journey, revealing steps in
a process, demonstrating platform flow, drawing attention to key proof, smoothing
transitions.

Never: decorative animation with no meaning · slow scroll effects that hide content ·
heavy video backgrounds · interactions that reduce accessibility · effects that slow
the page.

Motion must be pausable or non-disruptive.

## Source

- `atlassian/04-design-system.md` §4 (interactive elements), §5 (layout and spacing),
  §6 (photography and visual elements), §7 (logo)
- `atlassian/03-best-practice-guide-b2b-marketing-site.md` §9.1–9.5 (visual design
  principles, motion)
- `atlassian/10-qa-and-launch.md` §3.3 (design QA)
