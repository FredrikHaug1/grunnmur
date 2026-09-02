# grunnmur

Working repository for the rebuild of **videocation.no** — Videocation's Norwegian B2B
marketing site. Its two jobs are generating qualified B2B leads and building the B2B
brand.

Right now the repo holds **documentation only**. The previous implementation was
deleted deliberately in `58fdaf77`; no application code has been scaffolded yet, so
there is no `package.json` and no build.

## The three layers

| | What it is | Authority |
| --- | --- | --- |
| [`atlassian/`](atlassian/) | Read-only markdown mirror of the **MS (Marketing Site)** Confluence space, exported 2026-09-02 | **The specification.** Source of truth |
| [`.claude/rules/`](.claude/rules/) | That specification made enforceable — 18 rules, each ending in a `## Source` section citing the pages it derives from | Derived. Where a rule and `atlassian/` disagree, `atlassian/` wins |
| [`CLAUDE.md`](CLAUDE.md) | Entry point for agents: hard invariants, routing table, the traps that are easy to fall into | Operational summary of the two above |

Rules also carry **repo-owner decisions dated 2026-09-02** for areas `atlassian/` is
silent on — component structure, Sanity conventions, GROQ. Those decisions *are* the
source for those rules, and each such rule says so in a source note at the top.

## Start here

| If you want to… | Go to |
| --- | --- |
| Understand the ground rules before changing anything | [`CLAUDE.md`](CLAUDE.md) |
| Find the rule for the thing you are building | [rules index](.claude/rules/README.md) |
| Read the original specification | [Confluence export index](atlassian/README.md) |
| See which rules a spec page feeds | [Which rules each page feeds](atlassian/README.md#which-rules-each-page-feeds) |

## Reading order for a newcomer

1. [`CLAUDE.md`](CLAUDE.md) — hard invariants and routing
2. [`brand-positioning.md`](.claude/rules/brand-positioning.md) — who the site is for
   and what counts as a lead
3. [`site-structure.md`](.claude/rules/site-structure.md) — how the site is organised
4. [`tone-of-voice.md`](.claude/rules/tone-of-voice.md) — the Norwegian voice
5. [`diagnostic-tool.md`](.claude/rules/diagnostic-tool.md) — the AI analysis tool,
   the highest-risk feature on the site

## Conventions

- **The codebase is English. User-facing copy is Norwegian bokmål.** Code, comments,
  schema and field names, commit messages, and these docs — English. Only content is
  Norwegian.
- **Never fill a gap with a plausible default.** If the docs are silent or two docs
  conflict, ask. Open questions are marked `TODO(decision needed)` in the rule that
  needs them.
- **Norway only.** One market, no translation layer.

## Open decisions

Three items are unsettled and must not be guessed past:

- **Benchmark sample employee threshold** — the floor below which a company is
  excluded from the sample.
  [`diagnostic-benchmark.md`](.claude/rules/diagnostic-benchmark.md),
  [`diagnostic-tool.md`](.claude/rules/diagnostic-tool.md)
- **Vainu licensing** — unconfirmed, and it gates the private margin comparison, which
  is the whole teaser. [`diagnostic-tool.md`](.claude/rules/diagnostic-tool.md)
- **Commands** — no `package.json` exists yet, so no script names can be verified.
  [`CLAUDE.md`](CLAUDE.md)

## Also in the repo

- [`.claude/skills/`](.claude/skills/) — repo-local agent skills. New skills and
  subagents belong here, version-controlled with the repo, never in `~/.claude/`.
