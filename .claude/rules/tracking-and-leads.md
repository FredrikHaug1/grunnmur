# Tracking and lead classification

Tracking requirements are defined **before** implementation, and tested **before**
launch. A page without defined tracking is not ready for development.

## Data sources

**There are exactly three: GA4, HubSpot, and Google Search Console.** Do not add a
fourth analytics or attribution tool without an explicit decision.

| Area             | Source                   |
| ---------------- | ------------------------ |
| Organic traffic  | GA4                      |
| Page engagement  | GA4                      |
| Search data      | Google Search Console    |
| Backlinks        | Google Search Console    |
| Form data        | HubSpot                  |
| Lead status      | HubSpot                  |
| Campaign data    | UTM parameters / HubSpot |
| AI analysis tool | GA4 + HubSpot            |
| Chat             | GA4 + HubSpot            |

> **Owner decision, overrides the docs.** `07 §3` and `01` specify a 2026 analytics
> tool with a handover to GA4 in 2027, and name additional product-analytics and chat
> sources. **GA4 is the only analytics tool, from now on.** The 2026 OKR baselines
> (15,800 organic clicks, 8,100 brand clicks) were measured on the superseded tool —
> **re-baseline in GA4 before reporting against them.**

## Lead source types

| Source type      | Definition                                                      |
| ---------------- | --------------------------------------------------------------- |
| Site-sourced     | videocation.no is the primary source of conversion              |
| Campaign-sourced | From LinkedIn ads, email, webinar, event or campaign traffic    |
| Site-influenced  | Visited important pages before converting through another route |
| AI analysis      | Generated through the Company Competence Analysis Tool          |
| Chat             | First meaningful contact came through chat                      |

## Lead types

| Lead type                   | Counts towards the lead objective? |
| --------------------------- | ---------------------------------- |
| New B2B prospect            | **Yes**                            |
| Existing customer expansion | Supporting metric                  |
| Single user                 | **No**                             |
| Support enquiry             | **No**                             |
| Other / irrelevant          | **No**                             |

## Required HubSpot fields

Lead type · Source · Landing page · CTA · Interest area · Company size (50+
employees or not) · Organisation number · Domain match status · Sales status ·
Outcome · MRR impact

## AI analysis tool — extra lead fields

Beyond the standard fields, an AI-analysis lead record carries: company name ·
organisation number · industry code/category · company size indicators *where
available* · financial indicators *where available* · benchmark comparison status ·
main competence opportunity areas · email address · domain match status · source/UTM ·
analysis generated date · report sent status · **recommended sales follow-up angle**.

Sales must never open with "I saw you downloaded a report." The structured insight is
the point — without it the tool is just a gated PDF.

Tool metrics: analysis starts · successful lookups · **preview-to-email conversion
rate** · reports sent · qualified prospects created · **companies with 50+ employees
identified** · sales-accepted leads · meetings · opportunities. **Quality:** ICP fit ·
company size · industry relevance · domain match rate · sales acceptance rate ·
meeting conversion rate. Experience: drop-off at lookup and after preview · report
open rate · CTA click-through from report · return visits from analysed companies.

The quality tier is not optional — it is what "measure volume *and* quality" means.

## Review cadence

**Weekly** — form submissions · chat enquiries · AI analysis starts · tracking
errors · **broken forms or CTAs**.

**Monthly** — organic traffic · brand search · new B2B prospects · sales acceptance ·
top and underperforming pages.

**Quarterly** — progress against both objectives · lead quality · brand growth ·
major site changes required.

## Per-page KPI

Each important page has one primary job and one main KPI.

| Page type         | Primary job                      | Main KPI                                      |
| ----------------- | -------------------------------- | --------------------------------------------- |
| Homepage          | Position and route visitors      | CTA clicks, solution-page visits              |
| Solution page     | Create relevance                 | Demo clicks, AI analysis clicks, scroll depth |
| Platform page     | Explain product                  | Demo clicks, product video views              |
| Programme page    | Make offer concrete              | Programme CTA clicks, demo clicks             |
| Customer story    | Build trust                      | Assisted conversions, CTA clicks              |
| Resource page     | Build authority and early demand | Resource engagement, CTA clicks               |
| AI analysis page  | Generate qualified prospects     | Analysis starts, full reports sent            |
| Demo/contact page | Convert demand                   | Form completion or meeting booking            |

## What must be tracked on every page

Page views · primary and secondary CTA clicks · form starts · form completions · AI
analysis starts · chat interactions · correct HubSpot source and lead type · UTM
parameters preserved. Test submissions must be clearly marked or removed.

## Reporting rules

- **Do not report all form submissions as qualified leads.**
- Separate new B2B prospects from existing customers, single users and support
  enquiries.
- Measure volume _and_ quality.
- Track site influence, not only final conversion.
- Report campaign-sourced leads separately from site-sourced leads.
- Customer conversion and MRR are supporting metrics — sales owns lead-to-customer.

## Known instrumentation debt

Named problems to solve, not background:

1. **B2B conversion tracking is incomplete.** Lead actions either do not fire or are
   not wired to goals — "book a sales meeting" recorded ~2 conversions in 5.5 months
   while HubSpot received ~25–35 real sales leads.
2. **There is no closed loop.** Form → lead → deal stage → customer is not connected
   back to web source. Finding web-sourced customers meant reading a CSV by hand.
3. **The GA4 property must be marketing-only.** The historical property was
   app-dominated — the marketing site was 8% of traffic (35,619 of 473,315 unique
   pageviews), making channel data unusable for attribution. This is a property-design
   problem and it carries into GA4: either a marketing-only property, or a segment
   applied to every report. Do not repeat the mistake.
4. **LinkedIn effectiveness is unmeasured.** The payoff is indirect — awareness
   leading to later brand search and direct visits. Campaign-tag every LinkedIn link
   at minimum.

## Source

- `atlassian/07-tracking-and-performance.md` §3 (sources), §7 (lead sources), §8
  (lead types), §9 (HubSpot fields), §11 (page KPIs), §13 (reporting principles)
- `atlassian/02-target-audience-and-positioning.md` §1.4–1.6 (lead definitions)
- `atlassian/research/technical-seo-site-structure-visitor-journey.md`
  (instrumentation gaps, scope note)
- `atlassian/10-qa-and-launch.md` §3.6 (tracking QA)
