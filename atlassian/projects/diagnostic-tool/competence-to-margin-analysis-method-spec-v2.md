---
title: "Competence-to-Margin Analysis — Method Spec (v2)"
source: confluence
space: "MS — Marketing Site"
page_id: "1117290497"
url: "https://jiracation.atlassian.net/wiki/spaces/MS/pages/1117290497"
parent: "Diagnostic Tool"
author: "Truls Paulsen"
status: "current"
last_modified: "2026-06-29T10:15:38.567Z"
exported: "2026-09-02"
---

# Competence-to-Margin Analysis — Method Spec (v2)

## 1. Purpose

Test whether Videocation can generate a useful, credible, differentiated diagnostic report from **only a company name and/or organisation number**, using public and pre-computed data, that shows how competence development may improve operating margin.

The MVP does **not** claim to know the company's internal competence gaps. It generates **evidence-informed hypotheses** from official company data, NACE classification, industry benchmarks, and an archetype-specific value-and-competence model.

**Division of intelligence:** the LLM is the _report writer and reasoning interface_. The diagnostic intelligence sits in the structured model and benchmark database, not in the LLM's free judgment. (Core product principle, Section 21.)

**Phase 1 supports three target functions, set by organization type:**

| Target function | For | Outcome measured | Numeric benchmark? |
| --- | --- | --- | --- |
| Operating margin | Private companies | Operating profit / revenue | Yes (Vainu, NACE peers) |
| Budget-value | Public sector | Service quality / volume per budget krone | No (Phase 1) — benchmark-free |
| Mission-impact | Non-profits | Mission impact, trust, sustainability per krone | No — benchmark-free |

The **same competence engine** (archetype → value themes → factors → levers → mechanisms, with the management multiplier) serves all three. What differs is only the **benchmark layer**: private gets a quantified peer benchmark and three margin scenarios; public and non-profit run the competence analysis _without_ a numeric benchmark, grounded instead in organization type and named mechanisms.

**Why public/non-profit are in the MVP, not deferred:** Videocation has many customers in these segments. A municipality or foundation that enters its org number and gets "we can't analyze you" is not a graceful gap — it's a rejection of a whole served segment. The cost of _silence_ is higher than the cost of building it, and the build is modest because it reuses the engine (two more factor libraries plus a routing fork, not two more benchmark systems).

**Honesty is the design, not a fallback.** "We don't reduce public service to a margin, because that would be the wrong measure — here's what we look at instead" is exactly the _ærlig, selvsikker_ voice, and it signals to a municipality that we understand their world isn't a P&L. That is a better first impression than a forced fake benchmark would ever be. (KOSTRA and real public benchmarking are a Phase 3 enrichment — designed-for, not required to ship.)

---

## 2. Strategic rationale — where the moat is

A generic LLM can produce plausible business advice. It cannot reproduce:

* Norwegian company data tied to the specific visitor
* A proprietary NACE-to-archetype reclassification
* Industry operating-margin benchmarks computed from real data
* Archetype-specific **value-theme** models with weighted, evidence-graded competence levers
* Scenario logic crossed with industry competitive structure

The intended experience: _"This report knows what kind of business we are, how our industry performs, where we sit, and which competence areas most plausibly move our margin — and it's honest about what it can and can't know."_

The honesty is part of the moat, not a disclaimer bolted on. A report that distinguishes fact from hypothesis and admits its limits is more credible — and harder for a generic LLM to imitate — than one that overclaims.

---

## 3. The model in one picture

The diagnostic is a layered model. Each layer does a distinct job:

```
ORG NUMBER
   │  BRREG legal form → TARGET FUNCTION FORK
   ├─ AS / ASA / ENK …  → margin        (private: benchmark engine runs)
   ├─ KOMM / FKF / org … → budget-value  (public: benchmark-free)
   └─ FLI / STI / SA …   → mission-impact (non-profit: benchmark-free)
   │
   ▼
NACE / org type
   │  (routing key)
   ▼
ARCHETYPE  ── 1 of 7, defined by competitive structure + value-theme mix
   │
   ▼
VALUE THEMES  ── how this archetype competes: weighted mix of
   │              Desirable · Trusted · Available · Efficient
   ▼
FACTORS  ── the activity-level drivers that deliver each theme
   │          (each carries: weight, evidence grade, competence sensitivity, mechanism)
   ▼
COMPETENCE LEVERS  ── what Videocation can develop, attached to factors
   │
   ▼   × MANAGEMENT MULTIPLIER (cross-cutting, applies to every factor)
   ▼
TARGET OUTCOME  ── margin (value + cost drivers) | service-per-budget | impact-per-krone
       ▲
       │  PRIVATE ONLY: numeric benchmark + scenario (below/around/above median)
       │  PUBLIC / NON-PROFIT: no numeric benchmark — type-based hypothesis
```

The **target-function fork runs first**, off BRREG legal form, because it decides whether the benchmark engine runs at all. Private companies flow through the full benchmark path; public and non-profit skip the benchmark layer and run the competence engine on their own outcome.

Two things make this model defensible and not a generic LLM wrapper:

1. **Value-first.** The spine is _value themes_, not a cost tree. Margin = value − cost; each factor can move margin by raising value (willingness to pay, retention, share) **or** lowering cost (productivity, rework). Value framing leads, because Videocation sells competence as growth, not cost-cutting. (See Method spec, "value drivers vs cost drivers".)
2. **Evidence-graded.** Every factor carries an A–D evidence grade tied to the actual research base (Section 8.2). The strongest causal evidence sits under management and operational practice; value-side edges are real but more hedged.

---

## 4. The four universal value themes

Every archetype is defined by how heavily it weights these four themes. They are the generic version of a company-specific activity-system map (cf. Porter's _What Is Strategy?_ activity systems): the things any business competes on.

| Theme | What it means | Primary margin path |
| --- | --- | --- |
| **Desirable** | The product/service itself — quality, fit, differentiation | Value (willingness to pay) |
| **Trusted** | Reliability, confidence, reputation, compliance | Value (retention, pricing power, risk avoidance) |
| **Available** | Reach, distribution, ease of buying, share | Value (revenue scale — the _How Brands Grow_ lever) |
| **Efficient** | Delivering without waste | Cost (productivity, lower unit cost) |

The archetype sets the **weight mix**. A consultancy lives on Desirable + Trusted; a logistics firm on Available + Efficient; a SaaS firm on Desirable + Available. This weighting is what differentiates archetypes and what the report foregrounds.

**Evidence note:** "Efficient" is where the strongest evidence sits (operational/cost links are empirically robust, and management→productivity has causal RCT support — Bloom et al.). The value-side themes (Desirable, Trusted, Available) are real but more weakly evidenced at the _margin_ level. Factor evidence grades reflect this.

---

## 5. Management as a cross-cutting multiplier

Management is **not** a factor alongside the others. It is the meta-activity that determines how well every other factor is executed, and it carries the **strongest causal evidence in the entire model** — randomized field experiments (Bloom et al., India textile RCT; airline-captains experiment) show management practices _cause_ productivity gains, not merely correlate.

Modeling consequence:

* Management quality is a **multiplier applied across all factors**, not a row in one archetype's factor list.
* A company can have good factor-level competence but weak management of it, and the margin leaks at the management layer.
* In report terms: management/coordination capability is framed as the lever that makes the other levers pay off.

**Honesty rail on the causal evidence:** the Bloom RCTs are in _manufacturing and operational_ settings, not Norwegian B2B services. The causal claim we can defend is narrow and directional — _management practices causally improve productivity_ — and applying it to a Norwegian service SME is a reasoned **extrapolation**, not direct evidence. The model treats the management→productivity edge as causally grounded, the productivity→margin bridge as plausibly-but-noisily supported, and the whole chain in the visitor's specific context as a hypothesis. (Full treatment: Method spec, "external validity".)

**Useful inversion for public sector:** the management-practices evidence has been extended into _healthcare and schools_ (public-adjacent settings), so the management multiplier and several efficiency levers have **better external validity for public sector** than for private B2B services. The segment without a benchmark number happens to have firmer causal grounding for its core lever — those edges can be graded higher honestly.

---

## 6. MVP scope

**In scope (Phase 1):**

* Input by org number and/or company name
* Lookup via BRREG (free identification + legal form for target-function fork) and/or Vainu (financials + benchmark source)
* **Target-function fork** off BRREG legal form → margin / budget-value / mission-impact
* NACE / org-type identification → reclassification to 1 of 7 archetypes (all 7 built)
* **Private:** industry operating-margin benchmark + cross-industry competitive-structure signal + three-scenario classification
* **Public & non-profit:** benchmark-free competence analysis on their own target function, grounded in org type and named mechanisms
* Archetype value-theme weighting → ranked competence priorities (all target functions)
* LLM-generated report in Norwegian, strict hypothesis-language rules
* Honest uncertainty/limitations statement, incl. the "why no number" framing for public/non-profit

**Out of scope (Phase 1):**

* Interviews, surveys, internal company data, manual consultant assessment
* Full causal proof; perfect margin attribution
* **Numeric** benchmarking for public/non-profit (KOSTRA, cross-municipality comparison, impact-per-krone benchmarks) — Phase 3 enrichment
* Automated course recommendation (Phase 4)
* Full coverage of every NACE subcategory

---

## 7. User flow

1. **User enters** org number (preferred) or company name (fallback search).
2. **System retrieves** company data (name, org no, **legal form**, NACE + description, municipality, registration date, employees if available).
3. **Target-function fork** — BRREG legal form sets the target function:

    * **Private** (AS, ASA, ENK, ANS, DA, …) → operating margin → benchmark path
    * **Public** (KOMM, FKF, IKS, ORGL/statlig, …) → budget-value → benchmark-free path
    * **Non-profit** (FLI, STI, SA where applicable, …) → mission-impact → benchmark-free
    * _Edge/ambiguous legal forms_ → default to private margin if Vainu has financials, else benchmark-free; flag low confidence. (Legal-form → target-function mapping is a reviewable table — Section 14.)
    
4. **System selects archetype** — NACE / org type → primary (+ optional secondary, confidence, reason). All 7 archetypes available. See Section 9.

**Private branch (margin):** 5p. Retrieve benchmark (NACE benchmark + cross-industry signal). §10. 6p. Classify scenario — below/around/above median × industry-vs-economy. §11.

**Public / non-profit branch (benchmark-free):** 5n. **No numeric benchmark.** Establish the qualitative positioning frame from org type and any available public context (mandate, service type, org form). §11.5.

**Both branches converge:** 7. Retrieve archetype value-theme model — theme weights + factors for this target function. §12. 8. Rank competence priorities — top 3–5 by priority score. §12.3. 9. LLM writes report from the structured object, under strict rules. §13.

**Teaser vs gate, by branch:**

* Private: teaser = the benchmark number; gate unlocks the full analysis.
* Public/non-profit: no number to tease, so teaser = archetype identification + the _headline competence question_ ("for a municipality delivering these services, three competence levers most affect outcome-per-budget — verify your email to see them"). The gate still works; the hook is the question, not a number. (Gate mechanics: Benchmark Diagnostic spec §8.)

---

## 8. Evidence grading (the basis for the A–D field)

Every factor carries an evidence grade. The grade is **not** a guess — it is tied to the research base assembled for this project (academic bucket via Elicit; grey-lit via deep research; both documented in the Method spec).

| Grade | Meaning | Examples in this model |
| --- | --- | --- |
| **A** | Strong empirical evidence, meta-analysis, or causal field experiment | Management practices → productivity (Bloom RCTs); HPWS → operational performance (Combs meta-analysis, 92 studies) |
| **B** | Good empirical evidence or well-established management research | Lean/operational excellence → cost/productivity; HPWS training component |
| **C** | Plausible, widely used, limited direct margin evidence | Service-profit chain links; most value-side (Desirable/Trusted) → margin edges |
| **D** | Expert hypothesis, weak evidence, needs validation | Archetype-specific judgments without a study behind them |

**Two standing caveats the grades encode:**

* **Operational link strong, margin link weaker.** The evidence that competence improves _operations_ (productivity, quality, retention) is consistently stronger than the evidence it flows through to _accounting margin_ (HPWS and Lean both show this). Cost-side edges grade higher than value-side edges as a rule.
* **External validity gap.** The strongest causal evidence is manufacturing/ operational, not Norwegian B2B services. No factor should be graded A on the basis of evidence from a setting unlike the visitor's without noting it.

---

## 9. The seven diagnostic archetypes

All NACE sections reclassify into one of seven archetypes, defined by **competitive structure and value-theme mix** (not by what they produce). NACE is the routing key; the archetype is the modeling unit.

| # | Archetype | Target function | Dominant value themes |
| --- | --- | --- | --- |
| 1 | Production & asset-intensive operations | Output quality/volume per input cost | Efficient, Desirable |
| 2 | Project & expert services | Profitable delivery per employee/project | Trusted, Desirable |
| 3 | Trade, distribution & logistics | Contribution margin, flow & working-capital efficiency | Available, Efficient |
| 4 | Recurring service & care delivery | Service quality/capacity/continuity per resource cost | Trusted, Efficient |
| 5 | Digital, IP & scalable information | Scalable revenue, retention, reuse per cost base | Desirable, Available |
| 6 | Public service & budget-value | Best service outcome per budget krone | Trusted, Efficient |
| 7 | Non-profit & mission-impact | Mission impact/trust/sustainability per krone | Trusted, Desirable |

**Phase 1 builds all 7 archetypes.** Archetypes 1–5 (private) run the full benchmark path; 6–7 (public, non-profit) run benchmark-free on their own target functions. The _numeric_ benchmarking enrichment for 6–7 (KOSTRA etc.) is deferred to Phase 3, but the competence analysis for these archetypes ships in Phase 1.

**Secondary archetypes** are supported for hybrids (e.g. ICT consulting = Project & expert services primary, Digital/IP secondary; private health = Recurring service primary, Project/expert secondary). The classification returns primary, optional secondary, confidence, and reason.

**Deliverable:** a NACE-section → archetype mapping table (all 21 sections assigned), reviewable and override-able by internal experts. This is the routing key the runtime depends on. Start with the draft mapping; split an archetype only where a mapped section genuinely doesn't fit (earn the complexity).

---

## 10. Private-sector benchmark model

### 10.1 Source and floor

* Source: **Vainu** (financials + benchmark), BRREG for free identification.
* **Sample floor: N ≥ 50** per NACE level. Below it, roll up: `5-digit → 4-digit → 3-digit → 2-digit → archetype-level`.

### 10.2 Metrics stored per NACE level

n, median, mean, p25, p75, p90 (optional), raw min/max (stored, normally not shown), benchmark confidence grade, data year.

Plus, **once for the whole sample** (the competitive-structure signal): cross-industry median, mean, p25, p75.

### 10.3 Outlier handling (before any statistic)

Exclude inactive companies, missing-revenue companies, and those below a minimum revenue threshold (threshold TBD — Section 19). Winsorize operating margin at the 1st / 99th percentile; **store raw and cleaned separately.** Never use raw min/max in user-facing output — a sample always catches a −400 % or +300 % outlier, and showing it destroys credibility.

### 10.4 Display priority

Company margin · industry median · company percentile rank · gap to median · gap to upper quartile · industry-vs-national signal. Mean is context only. Min/max hidden.

### 10.5 Multi-year (design-for, MVP-optional)

Latest margin is the MVP minimum. Design the schema to support 3-yr average, trend, and volatility — needed for the validated top-performer rule (Diagnostic spec §5) and for stability checks.

---

## 11. Scenario logic — position × industry structure

The report's framing is a **grid**, not three flat cases: within-industry position crossed with the industry's competitive structure.

### 11.1 Within-industry position (public language)

| Scenario | Condition | Narrative focus |
| --- | --- | --- |
| A. Below median | margin < industry median | Close the gap |
| B. Around median | within tolerance band of median | Move from average to stronger |
| C. Above median | margin > industry median | Protect and extend the lead |

Internal percentile bands (not shown as false precision): critical gap < p25; below median p25–49; around median \~p45–55 band; top quartile p56–75; leader > p75.

### 11.2 Crossed with industry structure (the competitive-structure signal)

From the cross-industry aggregate (§10.2):

* **Low industry margin vs the economy** → likely high competitive pressure, weak moats. Above-median here = _winning a hard game_; the same pressure erodes any edge, so continuous capability-building is survival, not luxury.
* **High industry margin vs the economy** → more structural protection, but that is where complacency creeps in; competence keeps the lead from softening.

### 11.3 Which value themes / path lead, by scenario

* **Below median** → lead with **Efficient** theme (operating productivity, the best-evidenced lever) — stop the bleeding first.
* **Around / above median, not the leader** → **Available** theme becomes relevant (share growth via sales/marketing competence) — efficiency is decent, the question is scale.
* **Market leader** → defend on all themes; the risk is sliding back to the middle (the power-curve "stuck in the flat middle" finding makes above-median already an achievement against the base rate).

### 11.4 Honesty rails (mandatory in copy)

* **Competition inference is a likely reading, not a law:** _"lave marginer i bransjen tyder ofte på hard konkurranse"_ — **tyder ofte, ikke betyr.** Low margins can be structural (regulation, commodity inputs, capital intensity).
* **Operating margin ≠ economic profit.** The signal reads operating-level competition — pricing power and operating efficiency, where competence bites. It does **not** read economic moats or capital efficiency (no capital data). Never imply a power-curve position the data can't support.
* **Part of the gap to the top is structural, not competence-closeable.** Market leadership is self-reinforcing (double jeopardy); some of the gap to the top performer is size, not capability. Competence improves margin extraction at current position and the _odds_ of executing the only strategy that grows share — necessary, not sufficient.
* **Cross-industry margin is context, never a target.** A 3 %-margin industry and a 25 %-margin industry differ structurally; never suggest the low-margin company aim for the cross-industry median.

### 11.5 Public & non-profit — benchmark-free framing

These target functions have **no numeric benchmark in Phase 1**. The report does not classify a position; it reasons from **organization type** to where competence plausibly moves the outcome. The whole competence chain survives _except_ the position number — and the **named mechanism** is what keeps it from going generic. Without a number to anchor on, the hypothesis language must be _tighter_, not looser: lean explicitly on "for organizations like yours…" and resist over-asserting to compensate for the missing benchmark.

**Public sector — target: service quality/volume per budget krone.** "Budget-to-service" decomposes into the same two levers as margin — do more with the same resource, and deliver better outcomes — just without a kroner sign on the output. Named mechanisms:

* **Case-handling competence** → fewer errors and reversals → less rework, fewer complaints → more cases closed per krone _(grade B–A; public-adjacent evidence)_
* **Workforce planning / scheduling** → less overtime and agency cost, fewer coverage gaps → more service hours per budget krone _(B)_
* **Digital / service-design competence** → more self-service, less manual handling → capacity freed for complex cases _(C)_
* **Regulatory competence** → fewer legally incorrect decisions → fewer costly appeals and the rework they trigger _(B)_
* **Leadership / coordination** → the management multiplier; **higher external validity here** than in private services (Section 5) → grade up _(A–B)_

Value themes for public: _Trusted_ (legal correctness, fairness, accessibility) and _Efficient_ (throughput per budget).

**Non-profit — target: mission impact, trust, sustainability per krone.** Thinner data, but mechanisms are still nameable, splitting into _deliver more impact per krone_ and _secure/protect the resource base_:

* **Impact-measurement competence** → ability to demonstrate results → stronger funder trust → more reliable funding → more mission delivered _(C)_
* **Fundraising / donor-communication** → lower cost per krone raised, diversified, less fragile funding → more resource reaching the mission _(C)_
* **Program-design competence** → more impact per program krone (direct efficiency) _(C)_
* **Volunteer management** → more delivered capacity per paid krone (volunteers are the non-profit productivity multiplier) _(C)_
* **Governance competence** → trust and continuity protecting the whole _(C–B)_

Value themes for non-profit: _Trusted_ (donor confidence, governance) and _Desirable_ (mission clarity, beneficiary value).

**The honest "why no number" frame (required report mode):** open by naming, in the _ærlig, selvsikker_ voice, that this kind of organization isn't measured on margin and why — then pivot to what competence _can_ do. E.g.: _"En kommune måles ikke på driftsmargin — og det bør den ikke. Det som teller er hvor mye og hvor god tjeneste dere får ut av budsjettet. Der spiller kompetanse en konkret rolle."_ This is a _better_ first impression than a forced benchmark, and it's only available to a tool honest enough to say it.

---

## 12. Archetype value-theme & factor model

### 12.1 Structure

Each archetype (all 7) has:

* a **value-theme weighting** (how much Desirable / Trusted / Available / Efficient matter), and
* a set of **factors** delivering those themes, each modeled as below.

The factor object's `target` field carries the target function (`operating_margin` | `budget_value` | `mission_impact`), and `margin_driver` generalises to `outcome_driver` — `value`/`cost` for margin, `quality`/`efficiency` for budget-value, `impact`/`sustainability` for mission-impact. Same engine, target-aware labels.

### 12.2 Factor object

```
{
  "factor_id": "project_scope_control",
  "factor_name": "Project scope control",
  "value_theme": "Efficient",
  "margin_driver": "cost",
  "target": "operating_margin",
  "impact_weight": 5,
  "impact_pathway": "direct",
  "evidence_grade": "B",
  "competence_sensitivity": 5,
  "intervention_fit": 5,
  "management_sensitive": true,
  "competence_levers": ["Project management", "Scope management",
                        "Change-order handling", "PM financial literacy"],
  "mechanism": "Better scope control reduces unpaid work, overruns and rework,
                lowering delivery cost per project and improving operating margin."
}
```

New/changed fields vs the v1 draft:

* `value_theme` — ties the factor to the spine (Section 4).
* `margin_driver` — `value` or `cost`, so the model stays value-aware and doesn't collapse into cost-cutting.
* `management_sensitive` — flags factors the management multiplier amplifies (Section 5).
* `observable_signal_strength` **removed from the _priority_ formula** — see 12.3.

### 12.3 Priority ranking (revised — fixes the v1 formula)

The v1 formula multiplied five 1–5 fields (plus evidence), giving scores spanning \~2 to \~3000 — a 1500× range hyper-sensitive to tiny judgment changes, and exactly the false precision the spec elsewhere forbids. Two fixes:

1. **Use a bounded, additive-weighted score, not a long product:**

    ```
    priority = (impact_weight × W1)
             + (competence_sensitivity × W2)
             + (evidence_grade_score × W3)
             + (intervention_fit × W4)
    
    ```

    with small integer weights (e.g. W1=3, W2=2, W3=2, W4=1). Bounded, interpretable, robust to a single field nudging.


2. **Remove** `observable_signal_strength` **from priority.** It was _down-ranking important levers just because public data can't see them_ — backwards. The whole point of hypothesis language is to surface plausible levers we _can't_ directly observe. Keep the field for the report (it tells the LLM how confidently to phrase a given lever) but do not let it suppress ranking.

The score ranks the top 3–5 priorities. **Never shown as a number.** It controls emphasis and order, nothing more. (Honesty rail: weights drive ranking, never a surfaced causal percentage.)

### 12.4 Factor libraries per archetype

Factor lists for all 7 archetypes are drafted (the v1 draft's lists for 1–5; the public/non-profit factors and named mechanisms in Section 11.5 for 6–7). Each factor is tagged with `value_theme`, `outcome_driver`, `evidence_grade`, and `management_sensitive` during the modeling phase. Building one archetype end-to-end as a worked example is the next concrete deliverable — recommended: **Project & expert services** (clearest B2B-services fit) for the private path, and **Public service & budget-value** for the benchmark-free path, so both modes are proven before the rest are filled in.

---

## 13. LLM input object & report

### 13.1 Input object

The LLM receives a structured object with a `target_function` field (`operating_margin` | `budget_value` | `mission_impact`) that selects report mode, plus `company`, `archetype` (primary/secondary/confidence/reason), and `ranked_factors` (top 3–5 with mechanism, competence levers, per-factor phrasing confidence).

* **Private:** also includes `financials` and `benchmark` (incl. cross-industry signal) and `scenario` (incl. `industry_vs_economy` and which themes/path lead).
* **Public/non-profit:** no `benchmark`/`scenario`; includes a `positioning_frame` (org type, service/mission context) and `benchmark_free: true`.

### 13.2 Report rules (LLM)

_Common to all modes:_

* Explain likely drivers via the archetype value-theme model, in **hypothesis language** — never assert verified internal weaknesses.
* Separate observed facts from inferred hypotheses, explicitly.
* Explain each competence→outcome mechanism in plain language (Section 11.5 mechanisms for public/non-profit).
* Rank 3–5 competence priorities; end with a suggested next step.
* Norwegian bokmål, Videocation tone (jordnær, ærlig, selvsikker; no jargon, no "LMS", no superlatives, no false precision).

_Private (margin) mode:_

* Start with the benchmark conclusion (the concrete number).
* Frame by scenario **and** industry structure (Section 11.1–11.4).
* Apply the margin honesty rails — _tyder ofte_; margin ≠ economic profit; structural gap; context-not-target.

_Public / non-profit (benchmark-free) mode:_

* Open with the honest **"why no number"** frame (Section 11.5) — name that this org type isn't measured on margin and why, then pivot to what competence can do.
* Reason explicitly from **organization type**, not observed performance — hypothesis language _tighter_ here, since there is no number to anchor on. Do not over-assert to compensate for the missing benchmark.
* Use the target-appropriate outcome (service-per-budget / impact-per-krone), never margin.

### 13.3 Required report structure

_Private (margin):_

1. Benchmark summary (1-page exec summary: half text, half infographics)
2. What the benchmark means
3. Industry / archetype performance logic
4. Most likely competence-sensitive levers
5. Recommended competence priorities (3–5)
6. What this analysis can and cannot conclude
7. Suggested next step

_Public / non-profit (benchmark-free):_

1. What we look at for an organization like yours (the "why no number" frame + outcome definition) — 1-page exec summary, text + simple infographics (no benchmark chart)
2. Archetype / organization-type performance logic
3. Most likely competence-sensitive levers for this outcome
4. Recommended competence priorities (3–5)
5. What this analysis can and cannot conclude (stronger emphasis — no benchmark)
6. Suggested next step

Both: body follows inverted-pyramid; **method description last** (signals confidence, keeps the top useful).

### 13.4 The report is a hypothesis generator, not an audit

Critical framing, and the resolution of every "this framework needs internal data" tension in this project: the automated report produces an **industry-level hypothesis** from thin public data. The **deep diagnostic** — the actual activity/cost/capability audit (ABM-style, the full activity-system map) — is the **human follow-up** the lead converts into. The tool creates the conversation; the consultant does the audit. The report must never pretend to be the audit.

---

## 14. Data model

Tables: **Company**, **Financial**, **Benchmark** (now incl. cross-industry aggregate fields + winsorized/raw split), **Legal-form → target-function** (new — BRREG legal form → margin/budget-value/mission-impact, reviewable, with default/confidence for ambiguous forms), **Archetype mapping** (NACE/org-type → primary/secondary/confidence/ reason/override), **Archetype value-theme weights** (theme mix per archetype, all 7), **Archetype factor** (incl. `value_theme`, `outcome_driver`, `evidence_grade`, `management_sensitive`, `target_function`), **Course/intervention mapping** (Phase 4).

Store every diagnostic request (inputs, retrieved data, archetype, benchmark used, scenario, ranked factors, **prompt version, model version**, generated report, timestamp) — essential for eval and calibration.

---

## 15. Technical architecture

```
Input org no/name
→ BRREG identify (+ legal form)
→ TARGET-FUNCTION FORK: margin | budget-value | mission-impact
→ NACE / org-type → archetype map (1 of 7)

  ┌─ PRIVATE (margin):
  │   → Vainu financials
  │   → fetch benchmark (+ cross-industry signal)
  │   → compute position → classify scenario × industry structure
  │
  └─ PUBLIC / NON-PROFIT (benchmark-free):
      → build positioning frame (org type, service/mission context)

→ fetch archetype theme weights + factors (target-aware)
→ rank competence priorities (bounded formula)
→ build LLM input object (target_function selects report mode)
→ generate report (post-gate) → store request + output
```

APIs: BRREG (identification + legal form), Vainu (company + financial + benchmark source; private only), internal model DB (legal-form→target map, archetype mapping, theme weights, factors), LLM API (report), Videocation course DB (Phase 4).

**Cost discipline:** teaser uses cheap cached data + one Vainu lookup; the LLM call runs **only post-gate**, so spend follows verified leads, not tire-kickers. Cache one Vainu lookup per org number and reuse. (Vainu licensing/cost for the benchmark pull is a Phase 0 gate — Diagnostic spec §3, §10.)

---

## 16. Validation

**Internal:** test on known organizations across all 7 archetypes — private (with benchmark) and public/non-profit (benchmark-free). For each report: is the archetype right? (private) benchmark plausible and scenario correct? are priorities relevant? does it avoid overclaiming? (public/non-profit) does it hold the "why no number" frame without over-asserting? is it usefully **better than a generic LLM answer**? does it hold the honesty rails?

**Expert review:** NACE→archetype mapping; value-theme weights; factor weights and evidence grades; competence-lever mapping; report language.

**Held-out eval against a rubric** (the rubric is the deliverable — Method spec §8): names the mechanism; ties to the real gap; hedges causation; no generic filler; flags provenance; **creates value in every scenario** (incl. above-median); Norwegian voice; passes the two-reader test (HR director finds it useful, CFO doesn't laugh).

**User feedback:** relevant? benchmark useful/surprising? priorities sensible? would they want a follow-up conversation? would they share it internally?

---

## 17. Success criteria

Report generates from name/org number alone · benchmark line is concrete and credible · feels industry-specific · cleanly separates fact from hypothesis · priorities are plausible and actionable · internal reviewers agree it beats a generic LLM · it opens a natural advisory conversation · the model improves from feedback.

---

## 18. Risks & mitigations

| Risk | Mitigation |
| --- | --- |
| NACE too broad/misleading | Secondary archetype + confidence score |
| Benchmark distorted by outliers | Winsorize; median + quartiles; raw stored separately |
| Small NACE sample | Roll up to broader level / archetype |
| LLM overclaims internal weakness | Strict hypothesis-language rules (§13.2) |
| Public data outdated | Show accounting year + data freshness |
| Report feels generic | Value-theme model + benchmark position + competitive-structure signal |
| **Margin confused with economic profit / ROE / growth** | **Hard honesty rail (§11.4); operating margin only; no power-curve positioning claim** |
| **Gap attributed wholly to competence** | **Structural-gap rail (double jeopardy); competence = necessary not sufficient** |
| Priority formula false precision | Bounded additive score; never surfaced (§12.3) |
| User questions accuracy | Explain sources, benchmark confidence, limitations |
| Competence advice feels disconnected | Always state the competence→outcome mechanism |
| **Public/non-profit report over-asserts to fill the missing number** | **Tighter hypothesis language (§11.5, §13.2); reason from org type, not performance** |
| **Public/non-profit teaser has nothing to hook on** | **Teaser = archetype + headline competence question, not a number (§7)** |
| **Wrong target function from ambiguous legal form** | **Default + confidence flag; reviewable legal-form→target table (§14)** |

---

## 19. Open questions

* Default benchmark NACE level?
* Minimum revenue threshold to exclude micro/noise companies?
* Show percentile rank, or only below/on/above median, in MVP?
* Show gap to upper quartile in MVP?
* Negative operating margin: how handled? (likely its own narrative — a loss is not a "low margin" story)
* Holding/investment companies: exclude or special-case? (their margins are meaningless for this model — likely exclude)
* How much of the report shows before the email gate?
* Worked archetype first: confirm **Project & expert services** as the end-to-end pilot.
* External-validity: is there European/services management-practice evidence to narrow the gap before grading factors A? (Method spec open item)
* Which BRREG legal forms map to which target function — and what's the default for genuinely ambiguous ones (e.g. an AS that's really a non-profit in disguise, or a municipal AS)?
* For public/non-profit, how much public context (mandate, KOSTRA, service type) is worth pulling in Phase 1 to make the positioning frame concrete vs. relying on org type alone?
* Does the public/non-profit teaser (archetype + headline question) convert without a number, or does it need a different hook?

---

## 20. Phased rollout

* **Phase 1 — Diagnostic report, all three target functions.** All 7 archetypes. Private: full benchmark path (three scenarios × industry structure, value-theme model). Public/non-profit: benchmark-free competence analysis on their own outcome, with the "why no number" frame. Excludes numeric public/non-profit benchmarking, course recs, multi-year (unless easy).
* **Phase 2 — Model refinement.** Percentile ranking, gap-to-upper-quartile, multi-year trend, better NACE fallback, improved weights, expert review, eval-driven prompt tuning.
* **Phase 3 — Public/non-profit numeric benchmarking (enrichment).** Add real benchmark data where it exists: KOSTRA and cross-municipality indicators for public sector; service declarations, annual reports, impact reporting for non-profits. This _upgrades_ the Phase 1 benchmark-free reports with numbers — it does not introduce the segments, which already shipped in Phase 1.
* **Phase 4 — Course/intervention mapping.** Competence-lever → Videocation course mapping, role-based pathways, lead capture and sales follow-up.
* **Phase 5 — Learning & calibration.** Feedback, report rating, sales-outcome tracking, weight adjustment (incl. the high-performer survey, Method spec App. B), benchmark refresh, versioned improvements.

---

## 21. Core product principle

> The LLM does not diagnose freely. **The LLM explains the output of the model.**

Product intelligence sits in: benchmark data + NACE-to-archetype mapping + value-theme weighting + evidence-graded factor model + competence-to-margin mechanisms + scenario × industry-structure logic + the honesty rails.

The report **must** be: concrete, benchmark-based, industry-specific, honest about uncertainty, clear on fact-vs-hypothesis, focused on competence as a margin lever, useful enough to open a sales conversation, and better than a generic LLM answer.

The report **must not**: claim verified internal weaknesses without evidence; pretend competence explains all margin differences; overuse jargon; show false precision; recommend generic training without a mechanism; or confuse operating margin with economic profit, ROE, growth, or valuation.
