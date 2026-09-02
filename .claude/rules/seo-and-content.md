# SEO and content strategy

## The core finding

Ranking is not the problem — demand is. The enterprise pages sit around **position
4** for the terms they target. Almost nobody searches those terms. This is a
**build-from-scratch content roadmap aimed at creating demand**, not a rewrite to
capture existing demand. It pays back over quarters.

Do not treat buyer-vocabulary underperformance as a technical SEO defect.

## Buyer vocabulary

The language of the new positioning, which barely registers in search today:

`kompetanseutvikling for ansatte` · `kompetanseplattform` · `opplæring av ansatte` ·
`internopplæring` · `læringskultur` · `kollektiv kompetanse`

## Depth beats volume

Publishing more no longer builds authority — search engines demote thin content and
AI answer-engines absorb generic how-to traffic before it reaches us. The two
strongest pages in search are deep blog posts, not catalogue pages; between them
they carry ~17% of all search traffic. They earned rankings by being the best
answer, not by existing.

What survives is content a machine cannot generate from thin air: behaviour-change
and learning-transfer evidence · real Norwegian enterprise case studies · the
Kompetansekompass data · a genuine point of view on why collective competence beats
individual courses.

**The SEO fix and the B2B pivot are the same content brief.**

## Every page must offer a next step

**The blog exists to move readers toward the product or a conversation.** Not brand
awareness, not reach. That is settled, so the 90–98% exit rate is a **defect**, and
the fix is structural rather than editorial.

Every post ships with related content, a contextual next step, and a CTA matched to
the reader's likely intent. A post that earns a visit and offers nowhere to go is not
finished — this applies to the on-strategy content most of all, since customer stories
and the kollektiv-kompetanse pieces pull strong readership and currently lose every
reader.

## The course catalogue is removed

**The `/kurs` branch does not exist on the new site.** Not consolidated — removed. All
336 course pages, the category pages, and the `/kurs` hub itself are gone.

Courses return later as an **API-driven listing** serving brand awareness and the sales
team, not as indexable content pages. Until that exists, there is no catalogue.

This resolves the thin-content problem by deletion: the 821 "Crawled – currently not
indexed" pages — 68% of the not-indexed pile, the textbook fingerprint of thin content
— stop existing rather than needing a consolidation model.

Redirect handling for the removed branch is in [`redirects.md`](redirects.md).

**Consequence to plan for:** `/kurs` was the strongest non-homepage entry point on the
old site (2,103 unique pageviews, 25% bounce, a genuine springboard). Removing it
forfeits that traffic deliberately. Expect an organic dip and do not read it as a
regression.

## Index hygiene backlog

Known state to clear and then keep clear: 101 genuine 404s · 161 pages behind
redirects plus 4 redirect errors · 45 pages blocked by robots.txt with a further
**40 indexed despite being blocked** · 2 duplicates without a canonical · 1 where
Google overrode the chosen canonical.

The 40 indexed-but-blocked URLs are a misconfiguration, not a choice.

## Per-page SEO requirements

Every page ships with:

- SEO title written
- Meta description written
- Exactly **one** H1
- Logical heading structure
- Primary search intent reflected in the page
- Internal links included
- Meaningful alt text on meaningful images
- Clear, readable URL
- Correct canonical where relevant
- Included in the sitemap where relevant
- Redirects set up for any replaced page
- Not accidentally blocked from indexing
- Trackable in Google Search Console after launch

## Gated vs ungated

| Content | Gate? |
| --- | --- |
| Blog articles | Usually no |
| Short guides | Usually no |
| Templates / checklists | Sometimes |
| Webinars | Often yes |
| Benchmark reports | Often yes |
| High-value tools | Often yes |
| Product demo | No, unless booking is required |

Over-gating reduces trust and reach.

## Source

- [`atlassian/research/technical-seo-site-structure-visitor-journey.md`](../../atlassian/research/technical-seo-site-structure-visitor-journey.md) (ranking vs
  demand, thin content, index hygiene, blog dead-end, problems 5, 7, 8)
- [`atlassian/research/content-performance-and-what-to-change.md`](../../atlassian/research/content-performance-and-what-to-change.md) (depth over volume,
  buyer vocabulary, blog retention, catalogue trade-off)
- [`atlassian/10-qa-and-launch.md`](../../atlassian/10-qa-and-launch.md) §3.5 (SEO QA checklist)
- [`atlassian/03-best-practice-guide-b2b-marketing-site.md`](../../atlassian/03-best-practice-guide-b2b-marketing-site.md) §14.3 (gating)
- [`atlassian/06-page-briefs.md`](../../atlassian/06-page-briefs.md) §7 (SEO requirements)
