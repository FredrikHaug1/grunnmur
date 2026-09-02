# AI analysis tool (competence benchmark diagnostic)

A visitor enters their company, sees how their operating margin compares to their
industry, and reads where competence development can move it.

**This tool is an instrument, not the product.** It proves the link between collective
competence and business value and converts that proof into a qualified lead. Never let
it become the hero of the offering.

**Design goal:** an insight a visitor cannot reproduce by prompting an LLM. The value
is the *combination* — their actual margin position tied to competence levers for
their industry.

## Two-phase architecture

**Phase A — build time, batch.** Compute an industry benchmark table keyed by NACE
code from Vainu data, on our schedule. All cost and heavy lifting lives here.

**Phase B — runtime.** Org number → **BRREG** for name, legal form and NACE (free) →
target-function fork → **private only:** one Vainu lookup for financials → match the
pre-computed table → teaser → email gate → full report.

**BRREG first, Vainu only when financials are actually needed.** Public-sector and
non-profit visitors run benchmark-free and never need a margin, so they cost zero
Vainu enrichments. With Vainu priced per enrichment and its licensing still
unconfirmed, that materially reduces exposure.

**The expensive analysis runs only after a verified email.** Spend follows real
leads, not tire-kickers.

## Settled parameters

| Parameter | Value |
| --- | --- |
| Identification + legal form | BRREG (free) |
| Financials | Vainu (paid, private only) |
| Sample-size floor per NACE level | **50 companies** |
| Headline statistics | **Median + p25/p75** |
| Top-performer lookback | 3 years, consistent, anonymised |
| Expensive analysis | Post-gate only |
| Roll-up floor of last resort | **SSB 9-category** |
| Benchmark refresh | **Quarterly** |

Benchmark construction, roll-up, outlier handling and the top-performer rule are in
[`diagnostic-benchmark.md`](diagnostic-benchmark.md).

**Refresh is quarterly.** The architecture spec §2 says "yearly"; that is superseded.
Norwegian annual accounts land on a rolling deadline, so a quarterly rebuild keeps the
table current as filings arrive instead of letting it drift for eleven months — and
the credibility of the whole tool rests on that number being current.

## Missing margin — the flow must not break

Common for new companies, sole proprietorships and simplified accounts. When no usable
margin exists: **show the industry benchmark and name the gap plainly.** Median, spread
and top-performer range, plus a competence analysis for the *industry* without the
personal comparison.

Say why, in the honest register the rest of the report uses:

> Vi fant ikke regnskapstall for deres selskap. Her er bransjen.

Do not ask the visitor to type their own margin — that is friction at the exact moment
we are trying to prove value cheaply, and a self-reported number undoes the "we did the
work for you" effect. Do not route them to the benchmark-free public-sector report
either; a private company should not be told it is being analysed like a municipality.

## Honesty rails — mandatory in copy

- **`tyder ofte`, not `betyr`.** Low industry margins *often suggest* hard
  competition. They can also be structural — regulation, commodity inputs, capital
  intensity.
- **Operating margin ≠ economic profit.** No capital data, so never imply a moat or
  a power-curve position.
- **Part of the gap to the top is structural, not competence-closeable.** Competence
  is necessary, not sufficient.
- **Cross-industry margin is context, never a target.** Never suggest a 3%-margin
  industry aim for the cross-industry median.
- **The report is a hypothesis generator, not an audit.** The deep diagnostic is the
  human follow-up. Never let it pretend otherwise.

## Email gate

Purpose is stopping **bulk** abuse, not forensic certainty. Over-engineering costs
real leads to stop hypothetical scrapers.

1. Match email domain against the known domain for that org number
2. If absent, search by company name — a `companyname.no` match is a strong signal
3. Rare edge case: read the `.no` site and check the content matches the industry
4. **Graceful bottom rung — when confidence is low, do not hard-block.** Accept with
   a soft flag. We got the lead either way.

The visitor must see meaningful, company-specific value *before* the gate: company
name, industry, 2–3 observations, one example competence opportunity, and a clear
preview of the full report.

## Phase 0 gates — these block the build

**Vainu licensing is not confirmed.** Pricing is per-enrichment, and the licence may
restrict surfacing Vainu data in a public-facing product. Settle it with the Vainu
account contact **before** build — the financials half of the tool depends on it.
Routing identification through BRREG reduces the exposure but does not remove it: the
private-sector margin comparison, which is the whole teaser, still needs Vainu. Also
unresolved: what share of visitor-type companies have a usable margin, and how stale
the table can get before it stops being credible.

All five of architecture spec §11's open parameters were settled on 2026-09-02:
sample selection is **random above an employee threshold**; the onward CTA
**branches on the result**; the stability multiple is **1.5×**; the missing-margin UX
**shows the industry benchmark and says so**; BRREG **stays** as the free
identification step.

> TODO(decision needed): the employee threshold itself — the floor below which a
> company is excluded from the benchmark sample — is still unset.

## Required disclaimer

> The analysis is based on publicly available company data, industry benchmarks and
> predefined competence opportunity models. It is intended as a strategic starting
> point, not as financial advice. The report identifies potential areas worth
> investigating and does not guarantee margin improvement.

## Source

- `atlassian/projects/diagnostic-tool/competence-benchmark-architecture-spec-overview.md`
  (architecture, benchmark table, validation, email gate, settled parameters)
- `atlassian/projects/diagnostic-tool/competence-to-margin-analysis-method-spec-v2.md`
  §11.4 (honesty rails), §13.4 (hypothesis not audit)
- `atlassian/03-best-practice-guide-b2b-marketing-site.md` §12 (signature diagnostic
  tools, disclaimer)
- `atlassian/10-qa-and-launch.md` §3.8 (AI analysis tool QA)
- `atlassian/05-site-structure-and-user-journeys.md` §4.7 (free analysis page)
