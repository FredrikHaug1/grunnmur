# Tone of voice

User-facing copy on videocation.no is **Norwegian bokmål**. Everything else in this
repo — code, comments, schema and field names, commit messages, these rules — is
English. Norwegian examples below are quoted from the source docs and must not be
translated.

## The voice

Three words, all sourced: **jordnær, ærlig, selvsikker**.

In practice: `korte setninger, aktiv form, ingen superlativer`.

Never use: jargon · superlatives · false precision.

**On the word "LMS":** the no-jargon, no-"LMS" rule is scoped to **generated
diagnostic report copy** (method spec §13.2), not to the site as a whole. Videocation's
own positioning statement is *"instead of **LMS** tools built for individual
learning"*, `05 §5.5` is a whole journey named *"Visitor comparing Videocation with
LMS alternatives"*, and *"Our LMS is not creating enough value"* is a headline buyer
problem. Use "LMS" freely when naming the category we are positioning against; never
as filler jargon.

## What good looks like

From the sample diagnostic report — this is the target register:

> **Kort oppsummert**
>
> Nordvik Rådgivning har en driftsmargin på 7,0 %. Medianen for sammenlignbare
> norske rådgivningsselskaper er 9,2 %. Dere ligger altså litt under midten av
> bransjen — rundt 38. percentil.

Honesty about limits, stated plainly rather than buried:

> Analysen kan ikke se inn i selskapet deres. Men for et selskap som dette peker
> fire kompetanseområder seg ut som de mest sannsynlige for å løfte marginen.

Saying the uncomfortable thing first, and being right about it:

> En kommune måles ikke på driftsmargin — og det bør den ikke. Det som teller er
> hvor mye og hvor god tjeneste dere får ut av budsjettet. Der spiller kompetanse
> en konkret rolle.

## Banned phrases

Never ship these as standalone claims:

`Empower` · `Unlock potential` · `Seamless` · `Next-generation` ·
`Scalable solution` · `Transform your workforce` · `All-in-one platform` ·
`Future-ready organization` · `AI driven`

They are permitted only when immediately backed by a specific explanation. As
standalone claims they read like every other B2B site.

## Lead with the outcome, not the feature

| Weak | Better |
| --- | --- |
| "Access to 500+ courses" | "Give employees relevant training tied to business priorities" |
| "Video-based learning platform" | "Deliver structured learning programs employees can complete alongside work" |
| "Expert-led courses" | "Build shared understanding with trusted Norwegian experts" |
| "Analytics dashboard" | "See who is learning, where progress stops, and which programs need follow-up" |

## Lead with the buyer's problem, not our category

Buyers do not think "we need a learning platform." They think:

- "Our leaders need a common language."
- "We need to roll out AI competence across the company."
- "Employees are not completing training."
- "We need to show that learning supports business goals."
- "We need risk-based compliance training."
- "Our LMS is not creating enough value."

Organise messaging around these, not around our product structure.

## Hypothesis language

When making a claim the data cannot prove, hedge explicitly and separate observed
fact from inference. `tyder ofte`, not `betyr`. Never assert a verified internal
weakness in a customer's organisation without evidence.

## Before publishing

- Is the target audience clear within 5–10 seconds?
- Is the main problem clearly stated, and the outcome concrete?
- Does the page avoid generic SaaS language?
- Does the copy speak to B2B buyers, not private individuals?
- Does it explain what the customer actually gets?
- Are the CTAs specific?
- Are claims supported by proof, placed close to the claim?
- Spelling and grammar reviewed?
- Has the Norwegian copy been approved?

> TODO(decision needed): the pre-rework `TOV.md` also specified a fourth value word
> *Uformell* (muntlig, glimt i øyet, emojis med måte), a LIKS score target of 20–35,
> a ChatGPT policy, structural templates for LinkedIn posts, blog posts and customer
> stories, a 60-character title limit, a 150–160-character meta description, and the
> checks *"written as Videocation, not as an individual"*, *"can it be said more
> simply"* and *"active present tense"*. **None of it appears anywhere in
> `atlassian/`.** Re-authorise it or let it stay dropped.

## Source

- `atlassian/projects/diagnostic-tool/competence-to-margin-analysis-method-spec-v2.md`
  §13.2 (report rules, Norwegian bokmål, tone), §11.5 (public-sector framing)
- `atlassian/projects/diagnostic-tool/project-and-expert-services-archetype.md` §5
  (sample report, tone note)
- `atlassian/03-best-practice-guide-b2b-marketing-site.md` §5.2 (buyer problems),
  §5.3 (outcome messaging), §5.4 (banned language)
- `atlassian/10-qa-and-launch.md` §3.1 (content QA), §3.2 (positioning QA)
- `atlassian/06-page-briefs.md` §4.9 (buyer problem examples)
