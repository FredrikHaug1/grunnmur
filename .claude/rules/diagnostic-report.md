# Diagnostic report — modes, structure, and what the LLM may do

The report the AI analysis tool emits. Data, benchmarks and the email gate are in
[`diagnostic-tool.md`](diagnostic-tool.md).

## The core architectural constraint

> **The LLM does not diagnose freely. The LLM explains the output of the model.**

Diagnostic intelligence lives in the structured model and benchmark database — NACE
mapping, archetype value-theme weights, evidence-graded factors, scenario logic and
the honesty rails. The LLM is the **report writer and reasoning interface**, not the
diagnostician. Build the LLM step as a renderer over a scored model, never as a
prompt that reasons from scratch.

## Target-function fork — runs first

BRREG legal form decides which outcome the report is even about, and whether the
benchmark engine runs at all:

| Legal form | Target function | Benchmark? |
| --- | --- | --- |
| AS, ASA, ENK, ANS, DA … | Operating margin | **Yes** — full benchmark path |
| KOMM, FKF, IKS, ORGL … | Budget-value (service per budget krone) | **No** — benchmark-free |
| FLI, STI, SA … | Mission-impact (impact per krone) | **No** — benchmark-free |

Ambiguous forms default to private margin **if** financials exist, otherwise
benchmark-free — and are flagged low-confidence.

**Public and non-profit are in scope from Phase 1, not deferred.** Videocation has
many customers in these segments. A municipality that enters its org number and gets
"we can't analyze you" is a rejection of a served segment, not a graceful gap.

## Benchmark-free mode — the "why no number" frame

For public and non-profit there is **no numeric benchmark in Phase 1**. The report
does not classify a position; it reasons from organisation type to where competence
plausibly moves the outcome.

This absence is **scoped, not permanent**. KOSTRA and real public benchmarking are a
Phase 3 enrichment that *upgrades* these same reports with numbers — designed-for, not
required to ship. Build the schema to accept them later.

**Open by naming why there is no number**, in the honest, confident voice:

> En kommune måles ikke på driftsmargin — og det bør den ikke. Det som teller er hvor
> mye og hvor god tjeneste dere får ut av budsjettet. Der spiller kompetanse en
> konkret rolle.

**Never use margin as the outcome for these.** Use service-per-budget or
impact-per-krone.

**Hedge harder here, not less.** With no number to anchor on, hypothesis language
must be *tighter*. Do not over-assert to compensate for the missing benchmark.

Teaser for these segments is the archetype plus the headline competence question —
there is no number to tease, so the hook is the question.

## Private mode — scenario framing

The private report is framed by a **grid**, not three flat cases: within-industry
position crossed with the industry's competitive structure.

| Scenario | Condition | Narrative focus |
| --- | --- | --- |
| A. Below median | margin < industry median | Close the gap |
| B. Around median | within tolerance of median | Move from average to stronger |
| C. Above median | margin > industry median | Protect and extend the lead |

Internal bands — **never surfaced, they would be false precision**: critical gap
< p25 · below median p25–49 · around median ~p45–55 · top quartile p56–75 ·
leader > p75.

**Cross with industry structure.** Industry margin *below* the whole-economy median
suggests high competitive pressure and weak moats — above-median there means winning a
hard game, and continuous capability-building is survival. Industry margin *above* the
economy means more structural protection, and that is where complacency creeps in.

**Which theme leads, by scenario:**

- **Below median** → lead with **Efficient** (operating productivity, the
  best-evidenced lever). Stop the leakage first.
- **Around or above median, not the leader** → **Available** becomes relevant; growth
  via sales and marketing competence.
- **Market leader** → defend on all themes; the risk is sliding back to the middle.

## Required report structure

**Private (margin):**

1. Benchmark summary — one-page exec summary, half text, half infographics
2. What the benchmark means
3. Industry / archetype performance logic
4. Most likely competence-sensitive levers
5. Recommended competence priorities (3–5)
6. What this analysis can and cannot conclude
7. Suggested next step

**Public / non-profit (benchmark-free):**

1. What we look at for an organisation like yours — the "why no number" frame plus
   the outcome definition. No benchmark chart.
2. Archetype / organisation-type performance logic
3. Most likely competence-sensitive levers for this outcome
4. Recommended competence priorities (3–5)
5. What this analysis can and cannot conclude — **stronger emphasis, no benchmark**
6. Suggested next step

**Both:** inverted pyramid, and the **method description goes last**. Putting method
last signals confidence and keeps the top of the report useful.

Always rank 3–5 competence priorities and end with a suggested next step.

## The next step branches on the result

The onward CTA is **not fixed**. It is chosen from what the analysis found:

| Result | Next step |
| --- | --- |
| Below median — a concrete, named gap | **Book a demo / ta en prat.** There is something specific to discuss, and this is the warmest moment on the site. |
| Around or above median | The free KI-programme, or another lower-friction step. There is no gap to fix, so a sales CTA has nothing to point at. |
| Benchmark-free (public / non-profit) | Lower-friction step — the report has raised a question, not measured a shortfall. |

This makes the CTA data-dependent, which is more build work than a single fixed
button. It is worth it: offering "book a demo to close your gap" to a company that is
already outperforming its industry reads as though nobody looked at the numbers.

## Language and hedging

Norwegian bokmål. Voice per [`tone-of-voice.md`](tone-of-voice.md). Separate observed
fact from inferred hypothesis explicitly. Never assert a verified internal weakness
in the reader's organisation. Always state the competence → outcome mechanism in
plain language — a recommendation without a named mechanism is generic filler.

The priority score that ranks the levers is **never shown as a number**. It controls
emphasis and order, nothing else.

## Source

- `atlassian/projects/diagnostic-tool/competence-to-margin-analysis-method-spec-v2.md`
  §1 (target functions), §7 (user flow, fork), §11.5 (benchmark-free framing),
  §13.2–13.3 (report rules and structure), §21 (core product principle)
- `atlassian/projects/diagnostic-tool/project-and-expert-services-archetype.md` §5
  (worked report)
