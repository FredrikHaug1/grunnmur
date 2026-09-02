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
| Creating or moving a component, module or section       | `.claude/rules/component-structure.md` |
| What a section owns vs a module                         | `.claude/rules/section-module-model.md` |
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
- **`useCdn` is always `true`.** Never `false` in the request path — that caused an
  API usage spike once already. Drafts go through the Live Content API, not a second
  client.
- Every query is wrapped in `defineQuery` — an inline GROQ string is silently untyped.
- Always project explicitly. Never return a bare document.

**Components**

- **Named exports only.** No default exports anywhere in the component tree.
- Imports point strictly downward: sections → modules → components.
- **`components/` never imports a Sanity type.** If it does, it belongs in `modules/`.
- **Never build a Tailwind class dynamically** (`` `pt-${x}` ``). Tokens map to classes
  through static lookup objects, or the compiler never generates the class.
- Sections own spacing, background and width. Modules own content and must not set
  their own vertical padding or background.

**Design**

- The fonts are **Fraunces** (display) and **DM Sans** (everything else). Two fonts,
  never a third.
- Body copy is DM Sans 16px / 1.75. Primary CTA is Nordic Sun `#FFE577`, pill radius
  20px, black text.
- Navigation is lowercase. Headlines are sentence case.

**Copy**

- Never ship `Learn more`, `Submit`, `Download` or `Get started` as a CTA.
- The free-analysis CTA is **Få en gratis kompetanseanalyse**; its nav label is
  **Gratis analyse**. The other three labels in the docs are superseded.
- Voice is four words, not three: `jordnær, uformell, ærlig, selvsikker`.
- Never ship `Empower`, `Unlock potential`, `Seamless`, `Next-generation`, or
  `Transform your workforce` as a standalone claim.
- Korte setninger, aktiv form, ingen superlativer.
- **There is no course catalogue.** `/kurs` and everything under it is gone and 308s
  to `/`. Courses return later as an API-driven listing, not as indexable pages.

**Redirects**

- Never use `next.config` redirects — they run before middleware and cannot be
  overridden, which breaks manual-exception precedence.
- Never call `permanentRedirect()` inside a `try` block. It works by throwing; a
  surrounding `catch` swallows it and the redirect silently does not happen.

## Open decisions

Nearly everything was settled on 2026-09-02. Three items remain, and they must not be
guessed past:

- **The benchmark sample's employee threshold** — `diagnostic-benchmark.md`,
  `diagnostic-tool.md`. The floor below which a company is excluded from the sample.
- **Vainu licensing** — `diagnostic-tool.md`. Unconfirmed, and it gates the private
  margin comparison, which is the whole teaser.
- **Commands** — this file. No `package.json` exists yet.

## A deleted spec you may hear about

`marketing-site-specs.md` was removed on 2026-09-02 — it called itself a system prompt
and was riddled with superseded decisions (Proxima Nova, a `#333333`/`#FFE880` palette,
a `production` dataset). **`atlassian/` wins every time.** Three things were salvaged
before deletion and now live in the rules: the section/module model, the token→class
discipline, and the always-`true` `useCdn` rule. If a branch or comment cites that
file, it is citing something that no longer exists — check the rules instead.

## Subagents

None configured. `structure-aware-builder` and the `refactoring` skill were removed
in the rework. Revisit `structure-aware-builder` once the module pattern exists and
is written down — restoring it now would mean it invents the pattern on first use.

## Adding to this repo

Place new skills and subagents under `.claude/skills/` or `.claude/agents/` in the
repo — team-shared and version-controlled — never in `~/.claude/`.
