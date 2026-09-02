# CLAUDE.md

Videocation's Norwegian B2B marketing site, videocation.no — a Next.js + Sanity CMS
monorepo. Its two jobs are generating qualified B2B leads and building the B2B brand.

**This is a greenfield rebuild.** The previous implementation was deleted
deliberately in `58fdaf77` and carries no authority. The Confluence export in
`atlassian/` is the specification; `.claude/rules/` is that specification made
enforceable.

## Hard invariants

1. **`atlassian/` is the source of truth.** Where code and a doc disagree, the code
   is wrong. Never soften a rule to match an implementation.
2. **Never fill a gap with a plausible default.** If the docs are silent or two docs
   conflict, ask. A wrong rule propagates into every future session.
3. **The codebase is English. User-facing copy is Norwegian bokmål.** Code, comments,
   commit messages, schema and field names — English. Only content is Norwegian.
4. **Every change serves qualified B2B lead generation or B2B brand building.**
   Nothing else belongs on this site.
5. **No page enters design or development without an approved page brief.**
6. **Tracking is defined before implementation and tested before launch.**
7. **The logo is never reconstructed in CSS or HTML.** Always the original image file.
8. **Redirects are 308, and never fire on a CMS error** — only on a confirmed
   not-found.
9. **Norway only.** One market, no translation layer, no `language` or `country`
   fields. Anything in the source docs about a second market is superseded.
10. **Exactly three data tools: GA4, HubSpot, Google Search Console.** Analytics is
    GA4 and nothing else. Do not introduce a fourth.

## Routing

| Doing this                                              | Read first                            |
| ------------------------------------------------------- | ------------------------------------- |
| Writing or reviewing Norwegian copy                     | `.claude/rules/tone-of-voice.md`      |
| Deciding audience, positioning, what counts as a lead   | `.claude/rules/brand-positioning.md`  |
| Colours, type scale, capitalisation                     | `.claude/rules/design-tokens.md`      |
| Buttons, cards, spacing, imagery, logo, motion          | `.claude/rules/design-components.md`  |
| Navigation, sitemap, user journeys                      | `.claude/rules/site-structure.md`     |
| Building or restructuring a page                        | `.claude/rules/page-structure.md`     |
| CTAs, forms, conversion paths                           | `.claude/rules/cta-and-conversion.md` |
| SEO, metadata, content strategy, thin-content decisions | `.claude/rules/seo-and-content.md`    |
| Anything touching redirects or middleware               | `.claude/rules/redirects.md`          |
| Analytics, HubSpot fields, lead classification          | `.claude/rules/tracking-and-leads.md` |
| Pre-merge checks, launch readiness, accessibility       | `.claude/rules/qa-and-launch.md`      |
| The AI analysis / benchmark tool                        | `.claude/rules/diagnostic-tool.md`    |
| Building the benchmark table behind that tool           | `.claude/rules/diagnostic-benchmark.md` |
| What the diagnostic report says and how                 | `.claude/rules/diagnostic-report.md`  |
| Creating or moving a component                          | `.claude/rules/atomic-structure.md`   |
| Sanity schemas, Portable Text, datasets                 | `.claude/rules/sanity-schema.md`      |
| Writing GROQ, client config, caching                    | `.claude/rules/groq.md`               |

## Commands

> TODO(decision needed): there is no `package.json` in the working tree — the rebuild
> has not been scaffolded. No script names can be verified, so none are listed here.
> Fill this section in from the real manifests once they exist, and do not copy the
> pre-rework scripts on trust.

## What to get right that is easy to get wrong here

**Sanity and GROQ**

- Datasets are `prod` and `dev` — **not** `production` / `development`.
- Document types and fields are `camelCase`. Never kebab-case a type name.
- **Norway only — there is no translation layer.** No `language` or `country` fields,
  no per-market filter in any query. Do not add one speculatively.
- `slug.current` holds the **full path** (`kurs/ledelse/lederkurs`), not a leaf segment.
- Types come from **Sanity TypeGen**, not hand-written Zod. Zod is for untrusted
  input only.
- Every query is wrapped in `defineQuery` — an inline GROQ string is silently untyped.
- Always project explicitly. Never return a bare document.

**Components**

- **Named exports only.** No default exports anywhere in the component tree.
- Imports point strictly downward: templates → organisms → molecules → atoms.
  Cross-layer imports are absolute (`@/components/atoms/Button`); siblings are
  relative. `'../../'` means the component is in the wrong layer.

**Design**

- The fonts are **Fraunces** (display) and **DM Sans** (everything else). Two fonts,
  never a third.
- Body copy is DM Sans 16px / 1.75. Primary CTA is Nordic Sun `#FFE577`, pill radius
  20px, black text.
- Navigation is lowercase. Headlines are sentence case.

**Copy**

- Never ship `Learn more`, `Submit`, or `Get started` as a CTA.
- Never ship `Empower`, `Unlock potential`, `Seamless`, `Next-generation`, or
  `Transform your workforce` as a standalone claim.
- Voice is `jordnær, ærlig, selvsikker` — korte setninger, aktiv form, ingen
  superlativer.

**Redirects**

- Never use `next.config` redirects — they run before middleware and cannot be
  overridden, which breaks manual-exception precedence.
- Never call `permanentRedirect()` inside a `try` block. It works by throwing; a
  surrounding `catch` swallows it and the redirect silently does not happen.

## Open decisions

Rules containing `TODO(decision needed)` are unresolved and must not be guessed past:
`design-tokens.md` (three colour conflicts) · `design-components.md` (accent vs icon
colour) · `cta-and-conversion.md` (CTA label) · `diagnostic-tool.md` (refresh cadence, Vainu
licensing gate, five open parameters) · `diagnostic-benchmark.md` (NACE terminal rung,
stability multiple) · `redirects.md` (cache mechanism) · `seo-and-content.md` (catalogue purpose, what the blog is for) ·
`qa-and-launch.md` (smoke-test skill) · `tone-of-voice.md` (orphaned voice rules) ·
`atomic-structure.md` (entire file unsourced) · this file (commands).

## Subagents

None configured. `structure-aware-builder` and the `refactoring` skill were removed
in the rework. Revisit `structure-aware-builder` once the module pattern exists and
is written down — restoring it now would mean it invents the pattern on first use.

## Adding to this repo

Place new skills and subagents under `.claude/skills/` or `.claude/agents/` in the
repo — team-shared and version-controlled — never in `~/.claude/`.
