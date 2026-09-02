# QA and launch

QA does not only check whether something works. It checks whether the page helps
generate qualified B2B leads or build the brand.

## Standing principles

1. Every important page has a clear job.
2. Every CTA has a clear purpose.
3. Every claim is supported by proof.
4. Every visual explains, proves, humanises or creates memory.
5. Tracking is tested before launch.
6. Mobile is checked before launch.
7. Performance and accessibility are part of quality.
8. **The site is never launched with unclear lead classification.**

## Deployment QA — run before merging to `main`

- Vercel preview deployment builds without errors or warnings
- Vercel preview URL is accessible, not returning 404 or 500
- Sanity dataset is in the expected state
- Sanity Studio is accessible and content renders correctly in preview
- No regressions before merging to `main`
- Merge only after checks pass, or known issues are documented and accepted

The visual smoke test is an **automated Playwright check in CI**, run against the
Vercel preview URL. `10 §3.9` describes a manually invoked Claude Code skill; that
skill was deleted and is not coming back — a check nobody remembers to run is not a
gate.

It must assert, at minimum:

- Every key route returns 200 — no 404, no 500
- No uncaught console errors
- Screenshots captured at **375px, 768px and 1440px**, the three viewports the
  definition of done already requires
- No regression against the previous run's screenshots

A failing smoke test blocks the merge unless the failure is documented and accepted.

## Accessibility

Colour contrast meets **WCAG AA** · keyboard navigable · visible focus states ·
alt text on meaningful images · decorative images marked appropriately · semantic
headings · descriptive links · labelled form fields · understandable error messages ·
captions on video where relevant · motion pausable or non-disruptive.

## Performance

Images optimised · modern formats where possible · media below the fold lazy-loaded ·
no unnecessary background video · hero media lightweight · Core Web Vitals checked ·
mobile performance tested (not only desktop) · third-party scripts reviewed.

## UX

Clear primary CTA · secondary CTA for lower-intent visitors · clear navigation ·
descriptive links · short forms · clear next step after submission · works on desktop,
tablet and mobile · screenshots readable · mobile CTAs easy to tap · no key content
hidden behind unnecessary interactions.

## HubSpot and sales

Forms create or update contacts correctly · company information captured · company
size classifiable · lead type classifiable · interest area captured · source and
landing page captured · sales notified where needed · lead routing works · sales
status updatable · **existing customers separated from new prospects** · **single
users not counted as qualified B2B prospects** · AI analysis output sent to HubSpot ·
sales follow-up context available.

## Launch readiness checklist

Every area needs a named owner and a ready/not-ready call before launch:

Content approved · Design approved · Mobile QA complete · SEO QA complete ·
Tracking QA complete · HubSpot QA complete · Accessibility QA complete ·
Performance QA complete · **Redirects ready** · Sales handover complete ·
**Legal/privacy reviewed, if needed** · Deployment QA complete · Go/no-go decision made

"Redirects ready" means the checklist in [`redirects.md`](redirects.md) has passed.
Legal/privacy review is marked "if needed" in general — but it is **mandatory** for
the AI analysis tool, where privacy and marketing-consent language must be reviewed
before launch.

## AI analysis tool — pre-launch gate

The highest-risk feature on the site. Before launching or changing it:

- It has a dedicated landing page
- The value proposition is clear within 5–10 seconds
- The user gets meaningful value before email capture
- The output is company-specific, not generic
- Organisation number / company lookup works
- Data sources and limitations are transparent
- The analysis avoids unsupported financial claims
- The full report justifies the email gate
- **Work email validation works without creating too much friction**
- CRM receives structured lead context, and sales receives a recommended follow-up angle
- **Privacy and marketing-consent language has been reviewed**
- **Error handling is tested**
- **Performance is acceptable**
- Success is measured by lead quality, not only form submissions

## Go / no-go

**Launch may proceed when:** no critical bugs remain · forms and tracking work ·
HubSpot receives correct data · primary CTAs work · mobile is acceptable · SEO
basics are in place · content is approved · sales knows how to handle incoming
leads · known issues are documented and accepted.

**Launch must be delayed if:** forms do not work · leads are not captured
correctly · tracking is broken · mobile layout is broken · the page creates legal or
privacy risk · the page gives misleading information · a critical navigation or
conversion path is broken.

## Bug severity

| Severity | Meaning | Example |
| --- | --- | --- |
| Critical | Blocks conversion or creates major risk | Form does not submit |
| High | Serious issue affecting experience or trust | Mobile hero broken |
| Medium | Noticeable, workaround exists | CTA spacing inconsistent |
| Low | Minor polish | Small visual inconsistency |

Critical and high are resolved before launch unless explicitly accepted.

## Launch decision record

Every launch is recorded: **Date · Launch item · Decision (Go / No-go) · Owner ·
Notes.**

## Post-launch

**24 hours:** availability · forms · chat · AI analysis tool · HubSpot submissions ·
tracking events · broken links · layout · indexing.

**7 days:** traffic · CTA clicks · form submissions · AI analysis starts · chat
enquiries · lead classification · sales feedback · page speed · errors.

**30 days:** page performance against its stated job · lead quality · source quality ·
search visibility · brand search impact · user behaviour · improvements.

## Source

- `atlassian/10-qa-and-launch.md` §2 (principles), §3.3–3.11 (QA stages), §4 (launch
  checklist), §5 (go/no-go), §6 (post-launch), §7 (bug severity)
- `atlassian/03-best-practice-guide-b2b-marketing-site.md` §15 (technical and UX
  requirements)
