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

**Phase B — runtime.** Org number → one Vainu lookup (NACE, own margin, employees,
domain) → match the pre-computed table → teaser → email gate → full report.

**The expensive analysis runs only after a verified email.** Spend follows real
leads, not tire-kickers.

## Settled parameters

| Parameter | Value |
| --- | --- |
| Data source (v1) | Vainu, sole source |
| Sample-size floor per NACE level | **50 companies** |
| Headline statistics | **Median + p25/p75** |
| Top-performer lookback | 3 years, consistent, anonymised |
| Expensive analysis | Post-gate only |
| Roll-up floor of last resort | **Disputed** — see `diagnostic-benchmark.md` |

Benchmark construction, roll-up, outlier handling and the top-performer rule are in
[`diagnostic-benchmark.md`](diagnostic-benchmark.md).

> TODO(decision needed): refresh cadence is stated twice and differently — "refreshed
> yearly" (§2) versus "Quarterly" (§12). Pick one.

## Missing margin — the flow must not break

Common for new companies, sole proprietorships and simplified accounts. When no
usable margin exists: show the industry benchmark anyway, and run the competence
analysis for the *industry* without the personal gap. Still useful, still captures
the lead.

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
account contact **before** build. "Vainu, sole source" above is the intended design,
not a cleared one. Also unresolved: what share of visitor-type companies have a usable
margin, and how stale the table can get before it stops being credible.

> TODO(decision needed): open per architecture spec §11 — sample selection method ·
> onward CTA after the full report · stability multiple · missing-margin UX · whether
> BRREG stays as a free identification fallback.

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
