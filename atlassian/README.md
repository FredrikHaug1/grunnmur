# Confluence export — MS (Marketing Site)

Local markdown mirror of the [Marketing Site](https://jiracation.atlassian.net/wiki/spaces/MS/overview)
Confluence space (`jiracation.atlassian.net`, space key `MS`, space id `1093271554`).

Exported 2026-09-02. Read-only snapshot — edits here do **not** sync back to Confluence.
Each file carries frontmatter with its `page_id`, `url`, `parent`, and `last_modified`.

## Contents

Directory layout mirrors the Confluence tree (Confluence *folders* become directories).

| File | Confluence page |
| --- | --- |
| [`00-marketing-site.md`](00-marketing-site.md) | Marketing Site (space homepage) |
| [`01-strategy-and-okrs.md`](01-strategy-and-okrs.md) | 01 Strategy and OKRs |
| [`02-target-audience-and-positioning.md`](02-target-audience-and-positioning.md) | 02 Target Audience and Positioning |
| [`03-best-practice-guide-b2b-marketing-site.md`](03-best-practice-guide-b2b-marketing-site.md) | 03 Best Practice Guide for Designing a B2B Marketing Site |
| [`04-design-system.md`](04-design-system.md) | 04 Design System |
| [`05-site-structure-and-user-journeys.md`](05-site-structure-and-user-journeys.md) | 05 Site Structure and User Journeys |
| [`06-page-briefs.md`](06-page-briefs.md) | 06 Page Briefs |
| [`07-tracking-and-performance.md`](07-tracking-and-performance.md) | 07 Tracking and Performance |
| [`08-delivery-and-jira.md`](08-delivery-and-jira.md) | 08 Delivery and Jira |
| [`09-meetings-and-decisions.md`](09-meetings-and-decisions.md) | 09 Meetings and Decisions |
| [`10-qa-and-launch.md`](10-qa-and-launch.md) | 10 QA and Launch |
| [`templates/template-project-plan.md`](templates/template-project-plan.md) | Templates › Template - Project plan |
| [`templates/template-decision-documentation.md`](templates/template-decision-documentation.md) | Templates › Template - Decision documentation |
| [`templates/template-meeting-notes.md`](templates/template-meeting-notes.md) | Templates › Template - Meeting notes |
| [`research/marketing-site-performance-and-why-it-needs-to-change.md`](research/marketing-site-performance-and-why-it-needs-to-change.md) | Research › The marketing site: how it's performing, and why it needs to change |
| [`research/content-performance-and-what-to-change.md`](research/content-performance-and-what-to-change.md) | Research › Content performance, and what to change |
| [`research/technical-seo-site-structure-visitor-journey.md`](research/technical-seo-site-structure-visitor-journey.md) | Research › Technical SEO, site structure, and the visitor journey |
| [`projects/handling-of-redirects.md`](projects/handling-of-redirects.md) | Projects › Handling of redirects |
| [`projects/diagnostic-tool/competence-benchmark-architecture-spec-overview.md`](projects/diagnostic-tool/competence-benchmark-architecture-spec-overview.md) | Projects › Diagnostic Tool › Competence Benchmark Diagnostic Architecture Specification Overview |
| [`projects/diagnostic-tool/competence-to-margin-analysis-method-spec-v2.md`](projects/diagnostic-tool/competence-to-margin-analysis-method-spec-v2.md) | Projects › Diagnostic Tool › Competence-to-Margin Analysis — Method Spec (v2) |
| [`projects/diagnostic-tool/project-and-expert-services-archetype.md`](projects/diagnostic-tool/project-and-expert-services-archetype.md) | Projects › Diagnostic Tool › Project & Expert Services Archetype |

## Not exported

- **11 Documentation** (page id `1121353729`) — still an unpublished draft in Confluence with an
  empty body. Nothing to export; re-run once it is published.
- Empty folders `Meeting Notes` (`1095729153`) and `Decision Documents` (`1095761921`) — no pages inside.
- Attachments, images, and page comments. Text bodies only.

The space contains no blog posts.

## Refreshing

The export was pulled through the Atlassian MCP server (`getPagesInConfluenceSpace` with
`contentFormat: markdown`, falling back to per-page `getConfluencePage` for the four pages that
exceeded the bulk ADF conversion budget). Confluence macros, panels, and layout nodes are
flattened to plain markdown; a few zero-width characters from the source survive in headings.
