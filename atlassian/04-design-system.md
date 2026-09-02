---
title: '04 Design System'
source: confluence
space: 'MS — Marketing Site'
page_id: '1099366401'
url: 'https://jiracation.atlassian.net/wiki/spaces/MS/pages/1099366401'
parent: 'Marketing Site'
status: 'current'
version: 3
last_modified: '2026-06-07T07:31:04.473Z'
exported: '2026-09-02'
---

# 04 Design System

# **1. Colour System**

The palette is Nordic and restrained. White and near-white dominate. The accent is used sparingly — it guides the eye, it is not decoration.

‌

## **1.1 Primary colours**

| **Name**      | **Hex**       | **Variable**    | **Usage**                                   |
| ------------- | ------------- | --------------- | ------------------------------------------- |
| White         | #FFFFFF       | --color-white   | Main background                             |
| Black         | #171717       | --color-black   | Headlines, emphasised text                  |
| Black Overlay | #171717 (65%) | --color-overlay | Image overlays for contrast and readability |

‌

## **1.2 Accent colours**

| **Name**         | **Hex** | **Variable**     | **Usage**                                                              |
| ---------------- | ------- | ---------------- | ---------------------------------------------------------------------- |
| Nordic Sun       | #FFE577 | --color-sun      | Primary CTA buttons, important interactive elements                    |
| Nordic Sun Light | #FFEA8F | --color-sun-ligh | Hover state for interactive elements on white backgrounds              |
| Nordic Fjord     | #00A7B5 | --color-fjord    | Visual weight for non-interactive elements: icons, key figures, badges |

‌

## **1.3 Neutral colours**

| **Name**          | **Hex** | **Variable**        | **Usage**                            |
| ----------------- | ------- | ------------------- | ------------------------------------ |
| Off-white         | #FAFAFA | --color-off-white   | Subtle background for depth          |
| Light Gray        | #F3F3F3 | --color-gray-light  | Card background, section alternative |
| Medium Gray Light | #E8E8E8 | --color-gray-border | Borders, divider lines               |
| Medium Gray       | #737373 | --color-gray-mid    | Secondary text, icons                |
| Dark Gray         | #404040 | --color-gray-dark   | Primary body text                    |

‌

## **1.4 Interface colours**

| **Name**        | **Hex** | **Variable**       | **Usage**                                |
| --------------- | ------- | ------------------ | ---------------------------------------- |
| Cool Gray Light | #F5F7F9 | --color-cool-light | Secondary sections, card backgrounds     |
| Cool Gray       | #EDF2F7 | --color-cool       | Hover states, active elements            |
| Warm Gray Light | #F7F6F4 | --color-warm-light | Alternative sections, soft transitions   |
| Warm Gray       | #F2F0ED | --color-warm       | Secondary hover states, subtle contrasts |

‌

## **1.5 Data visualisation colours**

| **Name**       | **Hex** | **Variable**   | **Usage**                          |
| -------------- | ------- | -------------- | ---------------------------------- |
| Nordic Terra   | #E6C4A1 | --color-data-1 | Data visualisation — primary       |
| Nordic Pine    | #C4D4B0 | --color-data-2 | Data visualisation — secondary     |
| Nordic Fjord   | #B0CCD9 | --color-data-3 | Data visualisation — tertiary      |
| Nordic Berry   | #D9B0B0 | --color-data-4 | Data visualisation — quaternary    |
| Nordic Heather | #C4B8D9 | --color-data-5 | Data visualisation — supplementary |

‌

## **1.6 Colour usage guidelines**

### **1.6.1 Interactive elements**

- Use Nordic Sun (#FFE577) for primary CTA buttons
- Use Nordic Sun Light (#FFD100) for hover states on white backgrounds
- Reserve accent colours for interactive elements — not decoration

‌

### **1.6.2 Text**

- Primary text: Dark Gray (#404040) on light backgrounds
- Secondary text: Medium Gray (#737373)
- Headlines: Black (#171717)
- Links: Dark Gray with Nordic Sun Light on hover

‌

### **1.6.3 Backgrounds**

- Main content: White (#FFFFFF)
- Secondary sections: Cool/Warm Gray variants
- Image overlays: Black Overlay

‌

# **2. Typography**

Two fonts. No more. Fraunces builds identity and authority — it is the face of the brand. DM Sans ensures readability and clarity in everything else.

‌

## **2.1 Font pair**

| **Fraunces** Display / headings Variable font with three axes: opsz — optical size (9–144) wght — weight (100–900) WONK — character (0–1) Google Fonts — free | **DM Sans** Body text / UI / navigation Designed specifically for on-screen readability. Variable font with optical size adjustment from 9–40pt. Google Fonts — free |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

‌

## **2.2 Google Fonts import**

`@import url('https://fonts.googleapis.com/css2?family=Fraunces:opsz,wght,WONK@9..144,100..900,0..1&family=DM+Sans:opsz,wght@9..40,100..900&display=swap');`

‌

## **2.3 Typographic scale**

_ℹ️  Fraunces WONK axis: use higher values (0.7–1.0) on large display headings. Reduce toward 0 on H3/H4 for better readability at smaller sizes._

‌

| **Element**             | **Font**   | **Size**          | **Weight** | **Line Height** | **Fraunces Settings**          |
| ----------------------- | ---------- | ----------------- | ---------- | --------------- | ------------------------------ |
| **H1 — Hero**           | `Fraunces` | `48px / 3rem`     | `600`      | `1.1`           | `opsz 48, wght 600, WONK 0.75` |
| **H2 — Section**        | `Fraunces` | `32px / 2rem`     | `500`      | `1.2`           | `opsz 32, wght 500, WONK 0.5`  |
| **H3 — Subsection**     | `Fraunces` | `24px / 1.5rem`   | `500`      | `1.25`          | `opsz 24, wght 500, WONK 0.25` |
| **H4 — Small heading**  | `DM Sans`  | `20px / 1.25rem`  | `600`      | `1.4`           | `—`                            |
| **Body text primary**   | `DM Sans`  | `16px / 1rem`     | `400`      | `1.75`          | `—`                            |
| **Body text secondary** | `DM Sans`  | `14px / 0.875rem` | `400`      | `1.7`           | `—`                            |
| **CTA buttons**         | `DM Sans`  | `16px / 1rem`     | `600`      | `1.5`           | `—`                            |
| **Navigation**          | `DM Sans`  | `14px / 0.875rem` | `400`      | `1.5`           | `—`                            |
| **Meta text**           | `DM Sans`  | `12px / 0.75rem`  | `400`      | `1.5`           | `—`                            |

‌

## **2.4 CSS implementation**

### **2.4.1 Fraunces — H1 example**

`h1 {`

`  font-family: "Fraunces", serif;`

`  font-size: 48px;`

`  font-weight: 600;`

`  line-height: 1.1;`

`  font-variation-settings: 'opsz' 48, 'wght' 600, 'WONK' 0.75;`

`}`

‌

### **2.4.2 DM Sans — body text**

`p {`

`  font-family: "DM Sans", sans-serif;`

`  font-size: 16px;`

`  font-weight: 400;`

`  line-height: 1.75;`

`  font-optical-sizing: auto;`

`}`

‌

## **2.5 Capitalisation rules**

- Headlines: Sentence case
- Navigation: Lowercase
- Buttons: Sentence case
- Meta labels: Lowercase
- Course titles: Sentence case
- Expert names: Natural case
- Acronyms: UPPERCASE

‌

# **3. Typography Guidelines**

## **3.1 Contrast and readability**

- Minimum contrast ratio: 4.5:1 (WCAG AA)
- Increase weight rather than size to add emphasis
- Use appropriate line height — DM Sans needs air to read well
- Avoid text on images; use Black Overlay if necessary

‌

## **3.2 Hierarchy**

- Maximum two fonts per page — Fraunces and DM Sans
- Use size and weight — not colour — to create text hierarchy
- Maintain consistent spacing between text elements
- Limit the number of text styles per page

‌

## **3.3 Responsive typography**

- Scale text sizes appropriately across devices — especially H1 on mobile
- Minimum readable size: 14px body text on mobile
- WONK value can be reduced on mobile if Fraunces feels restless at small sizes
- Adjust line height for different screen sizes

‌

# **4. Interactive Elements**

Buttons, links, and navigation follow a simple system: Nordic Sun for primary actions, grey for secondary ones.

‌

## **4.1 Buttons**

### **4.1.1 Primary CTA**

- Background: Nordic Sun (#FFE577)
- Text: Black (#171717)
- Font: DM Sans 600, 16px
- Border-radius: 20px (pill shape)
- Hover: Nordic Sun Light (#FFD100)
- Padding: 10px 24px

‌

### **4.1.2 Secondary**

- Background: transparent
- Border: 1px solid #E8E8E8
- Text: Dark Gray (#404040)
- Font: DM Sans 500, 16px
- Hover: Light Gray (#F3F3F3) background
- Border-radius: 20px

‌

## **4.2 Links**

- Colour: Dark Gray (#404040)
- Underline: none by default
- Hover: underline + Nordic Sun Light (#FFD100)
- Visited: Medium Gray (#737373)

‌

## **4.3 Navigation**

- Font: DM Sans Regular, 14px, lowercase
- Active state: Nordic Sun underline or background
- Mobile: hamburger menu with full overlay

‌

# **5. Layout and Spacing**

Nordic minimalism. Space is not waste — it is the message. Every element needs room to exist.

‌

## **5.1 Core principles**

- Minimalist — remove the unnecessary
- Generous whitespace — minimum 24px between sections
- Clean, uncluttered layouts
- Soft, low-contrast compositions

‌

## **5.2 Card elements**

- Background: Light Gray (#F3F3F3) or Off-white (#FAFAFA)
- Border-radius: 12px
- Padding: 24px
- No high-contrast borders — use subtle grey dividers

‌

## **5.3 Section structure**

- Primary background: White
- Alternate sections between White and Off-white/Cool Gray Light
- Dark sections: use Black (#171717) sparingly — for example footer or strong hero section
- Max content width: 1200px, centred

‌

# **6. Photography and Visual Elements**

Real people in real situations. No stock-photo stiffness. Videocation is about learning that happens in the real world.

‌

## **6.1 Photography style**

- Natural light — direct or indirect sunlight
- Warm, natural tones: soft whites, beige, light wood
- Open, airy spaces
- Real people, real settings
- Soft contrast adjustments — never oversaturated
- Avoid very dark backgrounds
- Avoid text overlaid on images where possible

‌

## **6.2 Course images and expert visuals**

- Simple, layered clothing in muted tones
- Minimal accessories
- Natural, functional props with organic shapes
- Natural materials where possible

‌

## **6.3 Icons**

- Style: line iconography — not filled
- Size: 20px (inline), 24px (standalone)
- Colour: Nordic Fjord (#00A7B5) for decorative/informational use
- Colour: Dark Gray (#404040) for functional use

‌

# **7. Logo**

The logo is never reconstructed in CSS or HTML. Always use the original image file.

‌

## **7.1 Approved backgrounds**

- #FFFFFF — white
- #FAFAFA — off-white
- #F3F3F3 — light gray
- #171717 — black (inverted logo)

‌

## **7.2 Never**

- Change logo colours
- Place elements in the clear space
- Compress, stretch, or distort
- Add shadows, glows, or effects
- Place on low-contrast backgrounds
- Rotate or crop

‌
