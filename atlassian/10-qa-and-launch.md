---
title: "10 QA and Launch"
source: confluence
space: "MS — Marketing Site"
page_id: "1093730311"
url: "https://jiracation.atlassian.net/wiki/spaces/MS/pages/1093730311"
parent: "Marketing Site"
status: "current"
version: 6
last_modified: "2026-06-10T14:12:15.039Z"
exported: "2026-09-02"
---

# 10 QA and Launch

# **1 About this document**

This document defines the QA and launch process for changes to videocatin.no.

The goal is to make sure new pages, features and changes are clear, trustworthy, technically sound, measurable and aligned with the marketing site OKRs.

QA should not only check whether something works. It should check whether the page helps generate qualified B2B leads or build the Videocation brand.

# **2 QA principles**

1. Every important page must have a clear job.
2. Every CTA must have a clear purpose.
3. Every claim should be supported by proof.
4. Every visual should explain, prove, humanise or create memory.
5. Tracking must be tested before launch.
6. Mobile experience must be checked before launch.
7. Performance and accessibility are part of quality.
8. The site should not be launched with unclear lead classification.

# **3 QA stages**

## **3.1 Content QA**

Check whether the page is clear, relevant and credible.

Checklist:

* The target audience is clear within 5–10 seconds.
* The main problem is clearly stated.
* The outcome is concrete.
* The page avoids generic SaaS language.
* The copy speaks to B2B buyers, not private individuals.
* The page explains what the customer actually gets.
* The page uses specific CTAs.
* Claims are supported by proof.
* Proof is placed close to the claims it supports.
* Spelling and grammar have been reviewed.
* Norwegian copy has been approved where relevant.

## **3.2 Positioning QA**

Check whether the page supports Videocation’s positioning.

Checklist:

* Videocation is presented as a competence development partner, not just a course platform.
* The page supports the idea of structured competence development.
* The page connects learning to business value.
* The page explains the combination of platform, content, programmes, support and methodology where relevant.
* The tone is practical, clear, professional and human.
* The page avoids vague claims such as “empower”, “unlock potential” or “next-generation” unless explained specifically.

## **3.3 Design QA**

Check whether the page follows the design system.

Checklist:

* Colours follow the approved colour system.
* Nordic Sun is used primarily for important interactive elements.
* Typography follows the Fraunces and DM Sans system.
* Headings use sentence case.
* Navigation uses the approved style.
* Buttons follow the primary and secondary CTA system.
* Layout has enough whitespace.
* Cards and sections use approved spacing and background colours.
* Images feel real, specific and relevant.
* Generic stock photography is avoided.
* Icons use the approved line style.
* The page feels recognizably Videocation.

## **3.4 UX QA**

Check whether the page is easy to use.

Checklist:

* The page has a clear primary CTA.
* The secondary CTA supports lower-intent visitors.
* Navigation is clear.
* Links are descriptive.
* Forms are short and understandable.
* The next step after form submission is clear.
* The page works on desktop, tablet and mobile.
* Important screenshots are readable.
* Mobile CTAs are easy to tap.
* No key content is hidden behind unnecessary interactions.
* Motion supports understanding and does not distract.

‌

## **3.5 SEO QA**

Check whether the page is findable and technically correct.

Checklist:

* SEO title is written.
* Meta description is written.
* Page has one clear H1.
* Heading structure is logical.
* Primary search intent is reflected in the page.
* Internal links are included.
* Images have meaningful alt text where relevant.
* URL is clear and readable.
* Canonical URL is correct, if relevant.
* Page is included in sitemap, if relevant.
* Redirects are set up for replaced pages.
* No important pages are accidentally blocked from indexing.
* Google Search Console can track the page after launch.

‌

## **3.6 Tracking QA**

Check whether the page can be measured.

Checklist:

* Page views are tracked.
* Primary CTA clicks are tracked.
* Secondary CTA clicks are tracked.
* Form starts are tracked, if relevant.
* Form completions are tracked, if relevant.
* AI analysis starts are tracked, if relevant.
* Chat interactions are tracked, if relevant.
* HubSpot receives the correct source information.
* HubSpot receives the correct lead type fields.
* UTM parameters are preserved where relevant.
* Test submissions are clearly marked or removed.
* Conversion events are visible in reporting.

## **3.7 HubSpot and sales QA**

Check whether leads can be followed up correctly.

Checklist:

* Form submissions create or update contacts correctly.
* Company information is captured.
* Company size can be classified.
* Lead type can be classified.
* Interest area is captured.
* Source and landing page are captured.
* Sales receives notification where needed.
* Lead routing works.
* Sales status can be updated.
* Existing customers are separated from new prospects.
* Single users are not counted as qualified B2B prospects.
* AI analysis output is sent to HubSpot where relevant.
* Sales follow-up context is available.

## **3.8. AI analysis tool QA**

Use this checklist before launching or changing the AI analysis tool.

Checklist:

* The tool has a dedicated landing page.
* The value proposition is clear within 5–10 seconds.
* The user gets meaningful value before email capture.
* The output is company-specific, not generic.
* Organisation number or company lookup works.
* The preview is useful before email capture.
* Data sources and limitations are transparent.
* The analysis avoids unsupported financial claims.
* The full report is useful enough to justify the email gate.
* Work email validation works without creating too much friction.
* CRM receives structured lead context.
* Sales receives recommended follow-up angle.
* Privacy and marketing-consent language has been reviewed.
* Error handling is tested.
* Performance is acceptable.
* Success is measured by lead quality, not only form submissions.

Recommended disclaimer:

“The analysis is based on publicly available company data, industry benchmarks and predefined competence opportunity models. It is intended as a strategic starting point, not as financial advice. The report identifies potential areas worth investigating and does not guarantee margin improvement.”

## **3.9 Deployment QA**

Check that the build and content pipeline are stable before merging to main.

Checklist:

* Vercel preview deployment builds without errors or warnings
* Vercel preview URL is accessible and not returning 404 or 500 errors
* Sanity dataset is in the expected state
* Sanity Studio is accessible and content renders correctly in preview
* Run the visual smoke test via Claude Code (`visual-smoke-test` skill) against the preview URL and review output
* No regressions flagged in the smoke test before merging to `main`
* Merge to `main` only after smoke test passes or known issues are documented and accepted

## **3.10 Performance QA**

Checklist:

* Images are optimized.
* Modern image formats are used where possible.
* Media below the fold is lazy-loaded.
* No unnecessary background video is used.
* Hero media is lightweight.
* Core Web Vitals are checked.
* Mobile performance is tested.
* Third-party scripts are reviewed.
* Page does not feel slow on mobile.

‌

## **3.11 Accessibility QA**

Checklist:

* Colour contrast meets WCAG AA.
* Page can be navigated with keyboard.
* Focus states are visible.
* Meaningful images have alt text.
* Decorative images are marked appropriately.
* Headings are semantic.
* Links are descriptive.
* Forms have labels.
* Error messages are understandable.
* Videos have captions where relevant.
* Motion can be paused or is non-disruptive.

‌

# **4 Launch readiness checklist**

Before launch, confirm:

| **Area** | **Ready?** | **Owner** | **Notes** |
| --- | --- | --- | --- |
| Content approved |  |  |  |
| Design approved |  |  |  |
| Mobile QA complete |  |  |  |
| SEO QA complete |  |  |  |
| Tracking QA complete |  |  |  |
| HubSpot QA complete |  |  |  |
| Accessibility QA complete |  |  |  |
| Performance QA complete |  |  |  |
| Redirects ready |  |  |  |
| Sales handover complete |  |  |  |
| Legal/privacy reviewed, if needed |  |  |  |
| Go/no-go decision made |  |  |  |
| Deployment QA complete |  |  |  |

# **5 Go/no-go criteria**

A launch can go ahead when:

* No critical bugs remain
* Forms and tracking work
* HubSpot receives correct data
* Primary CTAs work
* Mobile experience is acceptable
* SEO basics are in place
* Content is approved
* Sales knows how to handle incoming leads
* Any known issues are documented and accepted

A launch should be delayed if:

* Forms do not work
* Leads are not captured correctly
* Tracking is broken
* Mobile layout is broken
* The page creates legal or privacy risk
* The page gives misleading information
* A critical navigation or conversion path is broken

# **6 Post-launch checks**

## **6.1 First 24 hours**

Check:

* Site availability
* Forms
* Chat
* AI analysis tool
* HubSpot submissions
* Tracking events
* Broken links
* Major layout issues
* Search indexing issues, if relevant

## **6.2 First 7 days**

Check:

* Traffic
* CTA clicks
* Form submissions
* AI analysis starts
* Chat enquiries
* HubSpot lead classification
* Sales feedback
* Page speed
* Error reports

## **6.3 First 30 days**

Review:

* Page performance against expected job
* Lead quality
* Source quality
* Search visibility
* Brand search impact, if relevant
* User behaviour
* Improvement opportunities

# **7 Bug severity levels**

| **Severity** | **Meaning** | **Example** |
| --- | --- | --- |
| Critical | Blocks conversion or creates major risk | Form does not submit |
| High | Serious issue affecting user experience or trust | Mobile hero broken |
| Medium | Noticeable issue but workaround exists | CTA spacing inconsistent |
| Low | Minor polish issue | Small visual inconsistency |

Critical and high issues should be resolved before launch unless explicitly accepted.

# **8 Launch decision record**

Use this table to document launch approval.

| **Date** | **Launch item** | **Decision** | **Owner** | **Notes** |
| --- | --- | --- | --- | --- |
|  |  | Go / No-go |  |  |

‌
