---
title: "Competence Benchmark Diagnostic Architecture Specification Overview"
source: confluence
space: "MS — Marketing Site"
page_id: "1116602369"
url: "https://jiracation.atlassian.net/wiki/spaces/MS/pages/1116602369"
parent: "Diagnostic Tool"
author: "Truls Paulsen"
status: "current"
last_modified: "2026-06-29T10:16:26.157Z"
exported: "2026-09-02"
---

# Competence Benchmark Diagnostic Architecture Specification Overview

## 1. What this is (and what it is not)

The diagnostic lets a visitor enter their company, see how their operating margin compares to their industry, and read an analysis of where competence development can move that margin. It is the engine behind the **"Benchmark min bedrift"** CTA.

**This tool is an instrument, not the product.** Its job is to prove the link between collective competence and measurable business value, and to convert that proof into a qualified lead. It is not the thing Videocation sells. The design must never let the diagnostic become the hero of the offering — it is the evidence that makes the offering credible.

**Design goal:** deliver an insight a visitor cannot easily reproduce by typing a prompt into an LLM. The unique value is the _combination_ — the company's actual margin position, tied to specific competence levers for their specific industry.

---

## 2. Two-phase architecture

The system splits cleanly into a build-time batch job and a runtime lookup. Keeping the expensive work out of the request path is the central design decision.

### Phase A — Build-time (batch, scheduled, refreshed yearly)

Compute an **industry benchmark table** keyed by NACE code, from Vainu data. This runs on our schedule, not the visitor's, and is where the cost and heavy lifting live. Output is a cached table the runtime reads from.

### Phase B — Runtime (per visitor, fast and cheap)

1. Visitor enters [org.no](http://org.no) (or company name / domain, resolved to [org.no](http://org.no)).
2. One Vainu lookup returns: NACE code, the company's own operating margin, employee count, registered website/domain.
3. NACE code is matched against the pre-computed benchmark table.
4. Visitor sees the **teaser**: their margin vs. the industry median and spread.
5. To unlock the **full report** (the Claude competence analysis), the visitor submits a work email that passes the verification gate.

The split matters for cost: the teaser uses only cheap, cached data plus a single Vainu lookup. The expensive Claude analysis only runs after a verified email — so spend follows real leads, not tire-kickers.

---

## 3. Data source: Vainu

Vainu is the single source for v1 discovery. It consolidates registry data, financial statements (sourced from official filings), and a website/domain field in one API — replacing what would otherwise be three separate integrations (BRREG + proff + PDF parsing) and removing all live scraping from the request path.

**Known caveat:** Vainu extracts financials automatically from filings, and occasional errors or missing values occur. Because the tool's credibility rests on the margin number, the spec requires a validation layer (Section 6) and a defined fallback when a margin is missing or implausible.

**Open licensing/cost question — resolve in Phase 0:** Vainu pricing is usage-based per enrichment/record. Two risks: (a) a bulk pull to build the benchmark table may be a significant quota hit, and (b) the license may restrict surfacing Vainu data in a public-facing product. Both must be confirmed with our Vainu account contact before build, not discovered after.

---

## 4. The benchmark table (Phase A detail)

### 4.1 Sampling, not census

We do not need every Norwegian company to get a stable industry number — we need _enough_ per NACE code. Sampling keeps enrichment cost controlled and is statistically sufficient.

* **Sample-size floor: 50 companies** per NACE level. Below this, the numbers are not stable enough to defend, and we roll up (Section 4.3).
* Sample selection method to be set in Phase 0 (random above an employee threshold, or top-N by revenue — cost vs. representativeness trade-off).

### 4.2 What we store per NACE level

For each NACE level that meets the sample floor:

* Sample size (n)
* **Median** operating margin — the headline "typical company" number
* Mean operating margin (context only; do not benchmark against it)
* **p25 and p75** — the interquartile range ("most companies sit between X and Y")
* Validated top-performer margin range (Section 5)
* Outlier-trimming applied
* Computation date

**Why median + p25/p75 as the headline:** margins are skewed, so the mean is pulled around by outliers. The median is the honest typical value. "Most companies in your industry sit between X% and Y%, and you are here" is the most intuitive thing an HR reader can take in — and most of them are not trained in statistics. Keep it simple.

### 4.3 NACE roll-up cascade

Norway's NACE has 800+ codes at the 5-digit level; many hold only a handful of companies. We cannot compute a stable median for a thin code, so granularity is **data-driven, decided per code** — not hand-collapsed upfront.

For a given code, use the most granular level that meets the 50-company floor, walking up until it does:

```
5-digit → 4-digit → 3-digit → 2-digit → SSB 9-category (last resort)
```

The SSB 9-category rollup (already proven with existing customers via Claude) is the floor of last resort, used only when even the 2-digit level is thin.

**Why data-driven, not manual collapsing:** manual collapsing bakes in judgment calls we would have to defend, and throws away precision for codes that _do_ have enough companies. Let the data decide the granularity per code.

**Honesty in the UI:** a company's own NACE code and the benchmark level it is compared against can differ (visitor is 62.012, but benchmarked at 62.0 because 62.012 was thin). The UI must say so plainly — "sammenlignet med programmeringsvirksomhet (NACE 62)" — rather than imply false precision. Admitting the granularity builds trust, not the opposite.

---

## 5. Top performer (validated, anonymized for v1)

Showing the best in the industry is aspirational and powerful — but only if it is real, not an accounting blip. Validation rule:

A candidate qualifies as industry top performer only if:

* It has filed accounts for the **last 3 years**, and
* Its margin is in the top of the distribution in **each** of those years (not just the latest), and
* Its margins are **stable** — no single year exceeds a set multiple of its own multi-year median (catches the firm that placed high on one inflated year).

**Output as a range, not a single number:** "Den beste i bransjen har ligget på 28–32 % driftsmargin de siste tre årene." A multi-year range reads as more honest and more impressive than a cherry-picked peak.

**Anonymized for v1.** We show the top performer's _margin_, not its name. Naming a specific company "best in industry" off our own calculation is a public claim about a real company — and if our extraction is off, that is a liability. The number alone is what makes the competence argument land. Named top performers can come later, once we trust the data.

---

## 6. Data validation layer

Required before any number reaches a visitor:

* **Outlier trimming** before computing any statistic. Use percentiles (p25/p75 for the headline range), never raw min/max — a sample will always catch a −400 % or +300 % margin from a shell, a one-off contract, or an extraction error. Telling an HR director the best company runs at 300 % destroys credibility.
* **Plausibility check** on the visitor's own margin; flag implausible values rather than display them.
* **Missing-margin fallback** (see 7.2).

---

## 7. Runtime flow (Phase B detail)

### 7.1 Happy path

[org.no](http://org.no) → Vainu lookup → NACE + own margin → match benchmark table → teaser → email gate → Claude competence analysis → full report.

### 7.2 Fallback: company identified, no usable margin

Common for newly registered companies, sole proprietorships, and firms filing simplified accounts. The flow must not crash. When no usable margin exists:

* Show the **industry benchmark** (median, p25/p75, top performer) anyway, and
* Run a competence analysis for the **industry** without the personal gap.

Still useful, still captures the lead. The exact UX to be defined, but the path must exist by design.

### 7.3 The Claude competence analysis (the payoff, post-gate)

Runs only after a verified email. References the company's **actual position**:

* **Below industry:** identify where the gap likely comes from and how collective competence closes it.
* **Above industry:** recommend how competence keeps them ahead.

The personal margin gap is what separates this from a generic LLM answer. The analysis must lean on the company's real numbers, not produce an industry essay.

---

## 8. Email verification gate

Purpose: stop **bulk** abuse, not achieve forensic certainty on every lookup. Over-engineering the gate costs real leads to stop hypothetical scrapers.

Verification cascade:

1. Match the email domain against Vainu's known website/domain for that [org.no](http://org.no).
2. If Vainu has no domain, search by company name; a `companyname.no` match is a strong (\~99 %) signal.
3. Edge case — a company on a non-.no domain (e.g. .io) whose .no is held by an unrelated firm: read the .no site and check the content matches the industry (a second, rare Claude call).
4. **Graceful bottom rung:** when confidence is low, do **not** hard-block. Accept with a soft flag, or fall back to a lighter check. We got the lead either way; the gate's job is volume control, not a fortress.

---

## 9. Conversion path

`Benchmark min bedrift` → diagnostic → teaser → email gate → full report → \[onward CTA: TBD — book en prat vs. enter the free KI-program\].

The onward CTA after the full report is an open product decision, flagged for the positioning/CTA conversation, not settled here.

---

## 10. Phase 0 — validate before build

Three things to test with Vainu in hand before committing to the architecture:

1. **Cost & licensing of the benchmark pull.** What does a sample (or census) pull cost in enrichments, and does our license permit surfacing Vainu data in a public product? Direct conversation with the Vainu account contact — this gates the whole build.
2. **Margin coverage.** What share of real visitor-type companies actually have a usable operating margin in Vainu? This tells us how often the 7.2 fallback fires and whether the personal-gap story is the common case or the exception.
3. **Benchmark staleness tolerance.** How old can the quarterly table get before the numbers stop being credible? Sets the real refresh cadence.

---

## 11. Open decisions (not yet settled)

* Sample selection method (random above employee threshold vs. top-N by revenue)
* Onward CTA after the full report
* Stability multiple for the top-performer validation rule
* Exact missing-margin UX
* Whether to keep BRREG as a free identification fallback to reduce Vainu spend on the teaser (deferred — v1 uses Vainu only, per discovery decision)

---

## 12. Settled parameters (for reference)

| Parameter | Value |
| --- | --- |
| Data source (v1) | Vainu (sole source) |
| Sample-size floor per NACE level | 50 companies |
| Headline statistics | Median + p25/p75 |
| Top-performer lookback | 3 years, consistent, anonymized |
| Benchmark refresh | Quarterly (cadence confirmed in Phase 0) |
| Expensive analysis | Post-gate only (Claude) |
| Roll-up floor of last resort | SSB 9-category |
