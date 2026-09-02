# Diagnostic benchmark table

How the industry benchmark behind the AI analysis tool is built and what may be shown
from it. The tool itself is in [`diagnostic-tool.md`](diagnostic-tool.md); what the
report says is in [`diagnostic-report.md`](diagnostic-report.md).

## Sampling, not census

We do not need every Norwegian company for a stable industry number — we need *enough*
per NACE code.

**Sample-size floor: 50 companies** per NACE level. Below it the numbers are not
stable enough to defend, and the code rolls up.

**Selection is random above an employee threshold**, never top-N by revenue. A median
is meant to describe the industry's actual middle; picking the largest firms biases it
upward and would tell a 38-person consultancy it trails a median built from the giants
in its code — which is not the comparison the report claims to make. The employee floor
exists to exclude shells and dormant entities, not to skew toward size.

> TODO(decision needed): the employee threshold value is unset.

Store per qualifying NACE level: sample size (n) · **median** operating margin ·
mean (context only) · **p25 and p75** · validated top-performer range · outlier
trimming applied · computation date. Plus, once for the whole sample, the
cross-industry median/mean/p25/p75 — the competitive-structure signal.

## NACE roll-up cascade

Use the most granular level meeting the 50-company floor, walking up until it does:

```
5-digit → 4-digit → 3-digit → 2-digit → SSB 9-category (last resort)
```

The cascade ends at **SSB 9-category**. The method spec §10.1 ends at
*archetype-level* instead; that is superseded. SSB is a real published Norwegian
classification, so the fallback label still names something the reader recognises —
whereas being benchmarked against "Project & expert services" would compare them to a
category of our own invention that they have never heard of.

Granularity is **data-driven per code**, never hand-collapsed upfront. Manual
collapsing bakes in judgement calls we would have to defend, and throws away precision
for codes that do have enough companies.

**Say so in the UI when levels differ.** If the visitor is 62.012 but benchmarked at
62.0, write *"sammenlignet med programmeringsvirksomhet (NACE 62)"*. Admitting the
granularity builds trust; implying false precision destroys it.

## Statistics discipline

- **Trim outliers before computing any statistic.** Winsorize at the 1st/99th
  percentile; store raw and cleaned separately.
- **Never show raw min/max.** A sample always catches a −400% or +300% margin from a
  shell company or an extraction error. Telling an HR director the best company in
  their industry runs at 300% destroys credibility.
- **Median is the headline.** Mean is context only — never benchmark against it.
  Margins are skewed, so the mean is pulled around by outliers.
- Plausibility-check the visitor's own margin. Flag implausible values rather than
  display them.

Headline framing for a non-statistical reader: *"most companies in your industry sit
between X% and Y%, and you are here."* Keep it that simple.

## Top performer — validation and anonymity

A company qualifies as industry top performer **only if all three hold**:

1. It has filed accounts for the **last 3 years**
2. Its margin is in the top of the distribution in **each** of those years, not just
   the latest
3. Its margins are **stable** — no single year exceeds **1.5×** its own
   multi-year median (this catches the firm that placed high on one inflated year)

**Output a range, never a single number:** *"Den beste i bransjen har ligget på
28–32 % driftsmargin de siste tre årene."* A multi-year range reads as more honest and
more impressive than a cherry-picked peak.

**Never name the company.** Show the margin, not the identity. Calling a specific real
company "best in industry" off our own calculation is a public claim about them, and
if our extraction is off it is a liability. This is a legal control, not a style
preference.

**The stability multiple is 1.5×.** Tight enough to exclude a firm whose strong year
came from a one-off contract or an asset sale, loose enough to tolerate normal
year-to-year variation in a services business. A firm doubling its own median in a
single year is exactly the accounting blip this criterion exists to catch.

## Data-quality caveat

Vainu extracts financials automatically from filings, and errors and missing values
occur. The tool's credibility rests on the margin number, so the validation layer
above is not optional and the missing-margin path in
[`diagnostic-tool.md`](diagnostic-tool.md) must exist by design.

## Source

- [`atlassian/projects/diagnostic-tool/competence-benchmark-architecture-spec-overview.md`](../../atlassian/projects/diagnostic-tool/competence-benchmark-architecture-spec-overview.md)
  §3 (data source caveat), §4 (benchmark table, sampling, roll-up), §5 (top
  performer), §6 (validation layer)
- [`atlassian/projects/diagnostic-tool/competence-to-margin-analysis-method-spec-v2.md`](../../atlassian/projects/diagnostic-tool/competence-to-margin-analysis-method-spec-v2.md)
  §10 (benchmark model, metrics, outlier handling)
