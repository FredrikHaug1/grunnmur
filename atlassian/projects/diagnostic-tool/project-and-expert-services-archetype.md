---
title: "Project & Expert Services Archetype"
source: confluence
space: "MS — Marketing Site"
page_id: "1120108545"
url: "https://jiracation.atlassian.net/wiki/spaces/MS/pages/1120108545"
parent: "Diagnostic Tool"
author: "Truls Paulsen"
status: "current"
last_modified: "2026-06-29T12:29:10.341Z"
exported: "2026-09-02"
---

# Project & Expert Services Archetype

## Why this archetype first

Project & expert services is the clearest fit for Videocation's own ICP (Norwegian B2B, 50–100 employees), the private margin path is the most-developed part of the model, and the mechanisms are concrete and easy to sanity-check. If the model can't produce a genuinely useful report here, it won't anywhere — so this is the right stress test.

Covers: consultancies, ICT/software consulting, engineering & architecture firms, law, accounting/advisory, agencies, and most NACE M (Professional, scientific, technical) plus parts of J (ICT) and F (Construction, where project delivery dominates).

---

## 1. Archetype definition

**Target function:** operating margin **Core economic logic:** profit comes from _profitable delivery per employee_. The business sells expertise-time; margin is made or lost on how well that time is priced, utilised, and delivered without waste. People are both the main cost and the main value.

**Value-theme weighting** (how this archetype competes — weights sum to 100):

| Theme | Weight | Why |
| --- | --- | --- |
| **Trusted** | 35 | Clients buy expertise they can't verify in advance; reputation, reliability and credibility drive both winning work and pricing power |
| **Desirable** | 30 | The quality and fit of the expertise itself — differentiation is in the work |
| **Efficient** | 25 | Utilisation, scope discipline, and rework determine whether good work is also profitable work |
| **Available** | 10 | Reach matters least here — growth is reputation- and relationship-led, not availability-led (this is _not_ a double-jeopardy mass-market) |

Note the low **Available** weight: unlike a consumer brand, an expert firm doesn't grow mainly through reach and availability. This shapes which competence path leads (Section 5).

---

## 2. Factor library (fully populated)

Nine factors, each tagged with value theme, outcome driver, evidence grade, scores, the management-multiplier flag, competence levers, and the named mechanism. Scores are 1–5; evidence grades A–D per the research base.

| # | Factor | Theme | Driver | Weight | Evid. | Comp.sens | Interv.fit | Mgmt× | Priority\* |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 1 | Pricing & value-based selling | Trusted | value | 5 | C | 5 | 5 | no | **High** |
| 2 | Billable utilisation | Efficient | cost | 5 | B | 4 | 4 | yes | **High** |
| 3 | Project scope control | Efficient | cost | 5 | B | 5 | 5 | yes | **High** |
| 4 | Delivery quality & rework | Desirable | both | 4 | B | 4 | 4 | yes | High |
| 5 | Proposal & tender quality | Trusted | value | 4 | C | 4 | 4 | no | Med |
| 6 | Repeat sales & account development | Trusted | value | 4 | C | 5 | 4 | no | High |
| 7 | Senior–junior leverage | Efficient | cost | 4 | C | 3 | 3 | yes | Med |
| 8 | Knowledge reuse | Desirable | both | 3 | D | 4 | 4 | yes | Med |
| 9 | PM financial literacy | Efficient | cost | 3 | C | 5 | 5 | yes | Med |

* Priority from the bounded additive formula (§12.3 of MVP spec), not shown to users.

### Full factor objects (the three top-priority ones, as the build pattern)

```
{
  "factor_id": "pricing_value_selling",
  "factor_name": "Pricing & value-based selling",
  "value_theme": "Trusted",
  "outcome_driver": "value",
  "target_function": "operating_margin",
  "impact_weight": 5,
  "impact_pathway": "direct",
  "evidence_grade": "C",
  "competence_sensitivity": 5,
  "intervention_fit": 5,
  "management_sensitive": false,
  "competence_levers": ["Value-based selling", "Pricing strategy",
                        "Commercial negotiation", "Articulating client value"],
  "mechanism": "Pricing on the value delivered rather than hours spent raises revenue per
                engagement without a matching cost increase. In expert firms this is often
                the single largest margin lever, because the cost base is relatively fixed
                and most upside flows straight to margin."
}
```

```
{
  "factor_id": "project_scope_control",
  "factor_name": "Project scope control",
  "value_theme": "Efficient",
  "outcome_driver": "cost",
  "target_function": "operating_margin",
  "impact_weight": 5,
  "impact_pathway": "direct",
  "evidence_grade": "B",
  "competence_sensitivity": 5,
  "intervention_fit": 5,
  "management_sensitive": true,
  "competence_levers": ["Project management", "Scope & change management",
                        "Change-order handling", "Client expectation management"],
  "mechanism": "Weak scope control means unpaid work, overruns and rework — hours spent
                that can't be billed. Tightening scope and handling change orders well
                lowers delivery cost per project and lifts margin directly. Management
                quality amplifies this: it's a discipline that has to be led, not just
                trained."
}
```

```
{
  "factor_id": "billable_utilisation",
  "factor_name": "Billable utilisation",
  "value_theme": "Efficient",
  "outcome_driver": "cost",
  "target_function": "operating_margin",
  "impact_weight": 5,
  "impact_pathway": "direct",
  "evidence_grade": "B",
  "competence_sensitivity": 4,
  "intervention_fit": 4,
  "management_sensitive": true,
  "competence_levers": ["Resource & capacity planning", "Pipeline-to-staffing alignment",
                        "Project management"],
  "mechanism": "In a people-business, idle expert time is pure cost. Better matching of
                pipeline to staffing raises the share of paid hours, spreading the fixed
                people-cost over more billable work and lifting margin. Strongly
                management-driven."
}
```

### The management multiplier in practice

Six of nine factors are `management_sensitive`. Per Section 5 of the MVP spec, management quality isn't a tenth factor — it scales these six. In the report, this surfaces as: _management and project leadership are what make the other levers actually pay off_ — and it carries the model's strongest (causal) evidence, so it's framed with the most confidence, even while the firm-specific application stays hypothesis-language.

---

## 3. Evidence-grade honesty (what we can and can't stand on, for this archetype)

* **Grade B (good evidence):** utilisation, scope/rework, delivery quality — these are operational-efficiency levers, exactly where the research is strongest (HPWS/Lean operational links; management→productivity causal root).
* **Grade C (plausible, limited direct margin evidence):** pricing, account development, proposal quality — these are value-side levers. Real and often the _biggest_ margin movers, but the direct empirical margin evidence is thinner (the value side always is). The report leans on them but hedges accordingly.
* **Grade D (expert hypothesis):** knowledge reuse — intuitive, weakly evidenced; lowest weight, phrased most tentatively.

This grading is _why_ the report can be confident about scope/utilisation mechanisms and more careful about pricing — it mirrors the actual strength of evidence, not a uniform confidence the data doesn't support.

---

## 4. Worked example — a realistic company through the pipeline

**Note:** "Nordvik Rådgivning AS" is a fictional but realistic stand-in. Org number and figures are illustrative; the point is to show the full data flow, not a real company.

### 4.1 Inputs (as the system would retrieve them)

```
{
  "company": {
    "name": "Nordvik Rådgivning AS",
    "organization_number": "912345678",
    "legal_form": "AS",
    "nace_code": "70.220",
    "nace_description": "Bedriftsrådgivning og annen administrativ rådgivning",
    "municipality": "Bergen",
    "employees": 38
  },
  "target_function": "operating_margin",
  "financials": {
    "accounting_year": 2024,
    "revenue": 71000000,
    "operating_profit": 4970000,
    "operating_margin": 0.070,
    "operating_margin_3y_avg": 0.078
  }
}
```

### 4.2 Target-function fork

Legal form `AS` → **private → operating margin → benchmark path.**

### 4.3 Archetype selection

NACE 70.220 → **Archetype 2, Project & expert services** (primary). No secondary needed (clean fit). Confidence: High. Reason: management-consulting NACE is core project/expert services.

### 4.4 Benchmark (illustrative pre-computed values)

```
{
  "nace_level": "5_digit",
  "n": 612,
  "industry_median": 0.092,
  "industry_mean": 0.108,
  "industry_p25": 0.041,
  "industry_p75": 0.151,
  "industry_percentile_rank": 38,
  "cross_industry_median": 0.058,
  "benchmark_confidence": "High",
  "data_year": 2024
}
```

### 4.5 Scenario classification

* Company margin 7.0 % < industry median 9.2 % → **Scenario A: below median.**
* Percentile rank 38 → below median band (p25–49), not critical (above p25).
* Industry median 9.2 % vs cross-industry median 5.8 % → **industry runs _above_ the whole-economy median** → this is a _relatively healthy-margin_ industry, not a brutal low-margin one. Competitive-structure read: moderate pressure, not a price war.
* 3-yr avg (7.8 %) slightly above latest (7.0 %) → margin has dipped recently; worth a gentle note, not alarm.
* **Path lead:** below median → lead with **Efficient** theme (utilisation, scope) — stop the leakage first — with pricing (value) as the high-upside second lever.

### 4.6 Ranked competence priorities (top 4, post-formula)

1. Project scope control (Efficient, B, mgmt×) — _stop billable leakage_
2. Pricing & value-based selling (Trusted, C) — _highest upside lever_
3. Billable utilisation (Efficient, B, mgmt×) — _spread fixed people-cost_
4. Repeat sales & account development (Trusted, C) — _cheaper growth than new logos_

…with the **management multiplier** framed across scope and utilisation.

---

## 5. Sample report (Norwegian, user-facing)

> This is what the LLM produces from the structured object above. Tone follows the Videocation guidelines (jordnær, ærlig, selvsikker — korte setninger, aktiv form, ingen superlativer). Exec summary first; inverted pyramid; method last.

---

### Kompetanseanalyse: Nordvik Rådgivning AS

**Kort oppsummert**

Nordvik Rådgivning har en driftsmargin på 7,0 %. Medianen for sammenlignbare norske rådgivningsselskaper er 9,2 %. Dere ligger altså litt under midten av bransjen — rundt 38. percentil.

Det er verdt å merke seg at rådgivningsbransjen i seg selv tjener bedre enn norsk næringsliv sett under ett. Bransjen er ikke presset ned i en knallhard priskonkurranse. Det betyr at gapet opp til medianen sannsynligvis handler mer om hvordan arbeidet prises og leveres enn om at bransjen er vanskelig å tjene penger i.

Analysen kan ikke se inn i selskapet deres. Men for et selskap som dette peker fire kompetanseområder seg ut som de mest sannsynlige for å løfte marginen: kontroll på prosjektomfang, verdibasert prising, fakturerbar tid, og videresalg til eksisterende kunder.

**Hva tallet betyr**

En driftsmargin på 7,0 % mot en median på 9,2 % er et håndterbart gap, ikke et faresignal. Marginen har falt litt fra et treårssnitt på 7,8 %, så det er verdt å følge med på utviklingen.

I et rådgivningsselskap er folk både den største kostnaden og den største verdien. Marginen avgjøres derfor av tre ting: om timene prises riktig, om de faktisk blir fakturert, og om arbeidet leveres uten unødvendig dobbeltarbeid. Det er her kompetanse spiller en konkret rolle.

**Hvorfor bransjelogikken peker hit**

Selskaper som dette konkurrerer først og fremst på tillit og på kvaliteten i selve arbeidet. Kunder kjøper ekspertise de ikke kan kontrollere på forhånd. Da blir rykte, presisjon og leveranseevne avgjørende — både for å vinne oppdrag og for å kunne ta godt betalt.

Samtidig er det effektiviteten som avgjør om godt arbeid også blir lønnsomt arbeid. To selskaper kan levere like god rådgivning, men ende på helt ulik margin — fordi det ene har bedre kontroll på omfang, bemanning og prising.

**De mest sannsynlige kompetanseløftene**

_Kontroll på prosjektomfang._ Når omfanget glipper, gjøres det arbeid som ikke kan faktureres — overskridelser, endringer som ikke prises, og dobbeltarbeid. Bedre styring av omfang og endringer senker leveransekostnaden per prosjekt og løfter marginen direkte. Dette er like mye et lederansvar som en ferdighet: det må ledes, ikke bare læres.

_Verdibasert prising._ Å prise etter verdien dere skaper, ikke bare timene dere bruker, er ofte den enkeltfaktoren som betyr mest for marginen i et rådgivningsselskap. Når kostnadsbasen er ganske fast, går det meste av en prisøkning rett til bunnlinjen.

_Fakturerbar tid._ Ledig ekspertkapasitet er ren kostnad. Bedre kobling mellom salg og bemanning øker andelen betalte timer og fordeler de faste personalkostnadene over mer arbeid som faktisk gir inntekt.

_Videresalg til eksisterende kunder._ Å utvikle kunder dere allerede har, er billigere enn å vinne nye. God kunde- og relasjonskompetanse gir mer forutsigbar inntekt og lavere salgskostnad.

På tvers av disse: god prosjektledelse er det som gjør at de andre løftene faktisk gir effekt. Forskningen er tydeligst nettopp her — bedre ledelse henger årsaksmessig sammen med høyere produktivitet.

**Anbefalte kompetanseprioriteringer**

1. Prosjektledelse og styring av omfang
2. Verdibasert salg og prising
3. Kapasitets- og bemanningsplanlegging
4. Kunde- og relasjonsutvikling

**Hva analysen kan og ikke kan si**

Dette er en hypotese bygget på offentlige tall og på hva som vanligvis driver marginen i denne typen selskap. Den bygger ikke på innsyn i Nordvik Rådgivning. Vi vet at dere ligger litt under bransjemedianen — vi vet ikke sikkert hvorfor. Det vi peker på, er de mest sannsynlige stedene å lete.

Tallet sier heller ikke noe om økonomisk lønnsomhet utover drift, og bransjegapet bør ikke leses som en fasit på hvor hard konkurransen er. Lave eller høye marginer i en bransje kan ha flere årsaker enn konkurranse alene.

**Neste steg**

Den naturlige fortsettelsen er en samtale der vi går fra hypotese til det konkrete: hvor i deres leveranser oppstår lekkasjen, og hvilke kompetansetiltak som monner mest. Det krever innsikt i egne tall som denne analysen ikke har.

_Metode: Analysen kobler selskapets driftsmargin mot et bransjebenchmark beregnet fra norske selskaper i samme NACE-kategori (median og kvartiler, n = 612). Bransjen plasseres mot norsk næringsliv samlet som et signal om konkurransebilde. Kompetanseområdene er hentet fra en arketypemodell for prosjekt- og ekspertvirksomhet, gradert etter forskningsbelegg. Modellen foreslår hvor kompetanse mest sannsynlig påvirker marginen — den måler ikke selskapets faktiske praksis._

---

## 6. The honest test: is this better than a generic LLM?

Hold this report against what a raw "how can a Norwegian consultancy improve its margin" prompt would produce. The differences that matter:

* **It uses the company's real number and real position** (7.0 % vs 9.2 % median, 38th percentile) — a generic answer can't.
* **It reads the competitive structure** (industry runs above the whole-economy median → gap is about pricing/delivery, not a brutal market) — a non-obvious, data-derived inference.
* **It's honest about what it can't know**, and that honesty is specific, not boilerplate.
* **The levers are ranked and mechanism-linked**, not a generic list of best practices.
* **It mirrors evidence strength** — confident on scope/utilisation, careful on pricing.

Where it's _still vulnerable_ to the "generic" critique, and what to watch in eval:

* The four levers (scope, pricing, utilisation, account development) are the _obvious_ ones for a consultancy. A sharp reader might think "I knew that." The differentiation has to come from the _specificity of the mechanism and the tie to their actual gap_, not from surprising levers. If eval readers find it obvious, the fix is sharper mechanisms and tighter coupling to the number — possibly archetype sub-types.
* This is the central risk for the whole product, exposed concretely. Better to see it here, on one archetype, than after building all seven.

---

## 7. What this worked example tells the build

* The pipeline holds: fork → archetype → benchmark → scenario × structure → ranked factors → report. Nothing in the flow broke when run end-to-end.
* The factor object schema is sufficient — every field got used.
* The management multiplier needs the report to _name_ it without making it a factor; the sample does this in one paragraph. Works.
* **The open question the model can't answer for you:** is "scope/pricing/utilisation for a consultancy" insightful enough? That's an eval question for real readers (HR director + CFO), and it's the thing to test before committing to all seven archetypes. Build this one, run five real consultancies through it, show the reports to people who run such firms, and listen for "I knew that" vs "that's useful."
