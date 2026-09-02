# Rules

The specification in [`atlassian/`](../../atlassian/) made enforceable. Each file
states what to do, and ends with a `## Source` section linking the Confluence pages
and dated repo-owner decisions it derives from.

[`../../CLAUDE.md`](../../CLAUDE.md) carries the canonical routing table and the hard
invariants. This page is the same set as a directory index, grouped by subject.

## Brand and copy

| Rule | Covers |
| --- | --- |
| [`brand-positioning.md`](brand-positioning.md) | Audience, positioning, lead definitions, where proof goes |
| [`tone-of-voice.md`](tone-of-voice.md) | Norwegian bokmål voice, banned phrases, hypothesis language |
| [`seo-and-content.md`](seo-and-content.md) | Content strategy, buyer vocabulary, per-page SEO, gating |

## Design

| Rule | Covers |
| --- | --- |
| [`design-tokens.md`](design-tokens.md) | Colour, typography, capitalisation |
| [`design-components.md`](design-components.md) | Buttons, links, cards, icons, photography, logo, motion |

## Structure and pages

| Rule | Covers |
| --- | --- |
| [`site-structure.md`](site-structure.md) | Navigation, sitemap, priority user journeys |
| [`page-structure.md`](page-structure.md) | The page-brief gate, section order per page type |
| [`section-module-model.md`](section-module-model.md) | What a section owns vs a module |
| [`component-structure.md`](component-structure.md) | Three-layer split, import direction, token→class discipline |

## Conversion and measurement

| Rule | Covers |
| --- | --- |
| [`cta-and-conversion.md`](cta-and-conversion.md) | CTA hierarchy, canonical labels, forms, one job per page |
| [`tracking-and-leads.md`](tracking-and-leads.md) | GA4/HubSpot/GSC, lead classification, per-page KPIs |
| [`qa-and-launch.md`](qa-and-launch.md) | Pre-merge checks, accessibility, go/no-go, post-launch |

## Engineering

| Rule | Covers |
| --- | --- |
| [`sanity-schema.md`](sanity-schema.md) | Schema naming, slugs, Portable Text, datasets |
| [`groq.md`](groq.md) | `defineQuery`, TypeGen, projections, client config |
| [`redirects.md`](redirects.md) | Precedence order, 308s, the error-safety contract |

## AI analysis tool

| Rule | Covers |
| --- | --- |
| [`diagnostic-tool.md`](diagnostic-tool.md) | Two-phase architecture, settled parameters, email gate |
| [`diagnostic-benchmark.md`](diagnostic-benchmark.md) | Sampling, NACE roll-up, statistics, top-performer rule |
| [`diagnostic-report.md`](diagnostic-report.md) | Report modes, structure, what the LLM may do |

## Adding a rule

A rule needs a `## Source` section. Where [`atlassian/`](../../atlassian/) is silent,
say so in a source note and name the dated decision it rests on instead — see
[`component-structure.md`](component-structure.md) for the pattern. Then add it to the
routing table in [`../../CLAUDE.md`](../../CLAUDE.md) and to the right group above.
