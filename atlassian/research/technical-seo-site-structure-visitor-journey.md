---
title: "Technical SEO, site structure, and the visitor journey"
source: confluence
space: "MS — Marketing Site"
page_id: "1112080385"
url: "https://jiracation.atlassian.net/wiki/spaces/MS/pages/1112080385"
parent: "Research"
status: "current"
version: 1
last_modified: "2026-06-17T13:08:50.168Z"
exported: "2026-09-02"
---

# Technical SEO, site structure, and the visitor journey

‌

**For the dev/engineering manager · June 2026**

Synthesis of <custom data-type="smartlink" data-id="id-0">http://videocation.no</custom>  across Search Console, Matomo, and HubSpot, focused on the technical and structural picture: how the site performs in search, how visitors move through it, and how it's instrumented. Problems are stated as problems to solve — the implementation is yours to design. Findings are stated plainly; interpretation is flagged \[judgement\].

# **Scope note (this shaped every number)**

The Matomo property tracks three things at once: the `.no` marketing site, the logged-in app (`/no/...`, course player, admin), and the Spanish site. The marketing site is only **8%** of the property's traffic (35,619 of 473,315 unique pageviews, Jan–16 June). All analysis below is filtered to `.no` marketing pages; raw property totals (the 83k "direct" visits, the SharePoint/Microsoft-SSO referrers, the event categories) describe the app, not the site we're discussing. Worth knowing for anyone querying Matomo directly.

# **Technical SEO: ranking isn't the problem, demand is**

The counter-intuitive headline: **where our pages appear in search, they rank fine.** The enterprise pages — `/bedrift`, `/kompetanseutvikling/kompetanseprogram` — sit around **position 4**. There's no technical ranking deficit to chase on them. The problem is that almost no one searches the terms they target, so a strong position earns almost no traffic.

Three technical-SEO patterns worth your attention:

* **A large "surfaced but ignored" bucket — now confirmed in the index.** Hundreds of course pages draw thousands of impressions while sitting at positions 25–45, and the indexing report shows the bigger version of the same thing: **821 pages are "Crawled – currently not indexed"** — Google has seen them and judged them not worth indexing at all. That's 68% of the not-indexed pile and the textbook fingerprint of thin content. Candidates for consolidation, not isolated fixes.
* **The catalogue is thin on every channel.** 336 `/kurs/*` pages averaging \~17 unique pageviews each over 5.5 months. **\[judgement\]** The 821 crawled-not-indexed pages turn this from "low traffic" into "Google actively declines to index most of them," which strengthens the consolidation case considerably — but still mind the internal-linking/topical-authority role thin pages can play, and the product-visibility argument (a thin catalogue weakens the enterprise pitch) that sits outside the data. Don't mass-prune without modelling the link-graph impact.
* **The trend is confirmed.** Over 16 months, average search position improved while impressions fell and clicks held steady. The indexing report explains it: from mid-March to mid-June 2026, "Not indexed" fell from \~2,244 to \~1,206 (nearly halved, \~1,000 pages) while "Indexed" held flat at \~480–496. Google has been steadily shedding pages it won't rank, and the indexed core barely moved. The junk leaving the index _is_ the mechanism behind the improving position — a healthy direction, though the volume of churn (redirects/404s, below) is worth understanding.
* **A concrete hygiene list from the index** (snapshot, all known pages): 101 genuine **404s**; 161 pages behind **redirects** plus 4 redirect errors; 45 pages **blocked by robots.txt** with a further 40 _indexed despite_ being blocked (Google indexed URLs it can't read — a small misconfiguration); and a few canonical issues (2 duplicates without a canonical, 1 where Google overrode the chosen canonical). None catastrophic; all worth a cleanup pass.

# **Site structure and the visitor journey**

The journey data tells a clear structural story.

**The homepage is the hub and carries the site.** 17,238 unique pageviews, \~60% of all marketing entries, with low bounce (20%) and low exit (24%). People enter here and move deeper. It works.

**A few pages are springboards; most are dead-ends.**

| **Page** | **uPV** | **Bounce** | **Exit** | **Role** |
| --- | --- | --- | --- | --- |
| `/` | 17,238 | 20% | 24% | Hub — feeds deeper |
| `/kurs` | 2,103 | 25% | 32% | Springboard — engaged |
| `/priser` | 1,315 | 3% | 49% | High-intent endpoint — read, then leave |
| `/kompetanseutvikling/kompetanseprogram` | 873 | 61% | 30% | Visited, doesn't hold |
| `/bedrift` | 552 | 63% | 34% | Visited, doesn't hold |
| Blog posts (each) | 200–370 | 88–94% | 90–98% | Dead-ends |

**The enterprise pages get traffic the structure can't capitalise on.** Here's the key cross-channel fact: `/kompetanseprogram` got **17 organic search clicks** but has **873 unique pageviews** — so \~98% of its traffic arrives from internal navigation, direct, and LinkedIn, not search. The B2B pages aren't undiscovered. They're reached, and then **over 60% of visitors bounce**, because once a visitor is on them there's no compelling next step and no conversion action of their own.

**The blog is a structural cul-de-sac.** Every post loses 90%+ of readers with no second click — including the on-strategy customer-story and kollektiv-kompetanse posts. Structurally there's no related-content, no contextual next step, no path onward. High-traffic entry points that lead nowhere.

**The only real conversion surface is a generic contact form.** Genuine enterprise leads mostly arrive via `/kontakt`, not the enterprise pages — because the enterprise pages have no conversion path, so the buyer falls back to the generic form. That form is also doing support and cancellations, all through one undifferentiated box.

# **Instrumentation: we can't see our own funnel**

This is the part with the most leverage, and it's largely a tracking-architecture problem.

‌

* **B2B conversion tracking is incomplete.** Matomo was set up B2C-first, B2B-second. The B2B goals reflect it: the "book a sales meeting" link recorded \~2 conversions in 5.5 months, and the "Contact us" and "Lead Gen" goals recorded effectively zero — yet HubSpot received \~25–35 real sales leads in the same window. So the on-site lead actions either aren't firing or aren't wired to fire. The demand is real; the tracking misses it.
* **There's no closed loop.** Form submission → HubSpot lead → deal stage → customer is not connected back to web source. Finding our two web-sourced customers required reading a HubSpot CSV by hand. We cannot currently see the marketing funnel end to end.
* **No marketing-only channel segment exists.** Because the property is app-dominated, we can't get a clean acquisition picture for the marketing site without a segment. The current channel data is unusable for marketing attribution.
* **A measurement gap on LinkedIn (named).** We run a LinkedIn brand-awareness strategy whose likely payoff is indirect — awareness leading to later brand search / direct visits — which is currently unattributable. As a direct click source LinkedIn is tiny (118 visits). Closing this needs campaign tagging and timeline correlation, not just a new report.
* **Export hygiene (minor).** The entry-page-_titles_ export came back as a single day despite a multi-month selection — worth a look at how that report exports, since it silently produced misleading data.

# **The problems to solve (framed for you to design against)**

1. **Give the enterprise pages a conversion path of their own.** `/bedrift` and `/kompetanseprogram` get real cross-channel traffic and bounce 60%+ with no action to take. Problem to solve: what does a visitor _do_ on these pages, and how do we capture it?
2. **Fix the closed loop.** Make web source traceable through to customer: form submission tagged by source → HubSpot → deal → outcome. Problem: end-to-end funnel visibility without manual reconciliation.
3. **Instrument the real lead actions.** The lead/contact/booking events need to fire reliably and map to goals, so pipeline isn't a CSV-sorting exercise. Problem: trustworthy on-site conversion tracking for B2B.
4. **Separate the contact form's three jobs.** Sales, support, and cancellation through one box buries the sales signal. Problem: route intent to the right destination at the point of contact.
5. **Resolve the blog dead-end structurally.** 90%+ exit with no onward path. Problem: how does a blog reader discover a relevant next step?
6. **Create a marketing-only analytics segment** (or separate the properties) so acquisition for the `.no` site is measurable apart from the app.
7. **Catalogue consolidation, modelled not mass-applied.** Reduce the thin long tail without damaging internal-linking/topical authority. Problem: which pages to consolidate, and what's the link-graph impact?
8. **Clear the index hygiene backlog.** The indexing report surfaced 101 404s, 161 redirects (+4 errors), 85 robots.txt-related issues (45 blocked, 40 indexed-but-blocked), and a few canonical problems. Problem: clean these up and decide what's intentional versus accidental — especially the 40 indexed-but-blocked URLs. (Confirm whether the property is domain-level and folds in app/help subdomains; the counts may need the same marketing-only carving as everything else.)

# **Known gaps (named, not smoothed)**

* **LinkedIn effectiveness is unmeasured** — the awareness strategy's indirect payoff is currently invisible; closing it needs tagging + timeline analysis (problem #3-adjacent).
* **Marketing-only acquisition is unmeasurable** without a segment (problem #6).

These don't change the conclusions — they're the difference between "strongly indicated" and "confirmed," and each has a clear way to close it.

‌
