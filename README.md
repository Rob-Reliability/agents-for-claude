# Agents for Claude — Your AI Reliability Team

**7 field-grade Claude Code subagents for industrial reliability and
maintenance engineers.** Built by [Rob Reliability](https://robreliability.com).

Unlike [skills](https://github.com/Rob-Reliability/reliability-skills-for-claude)
(which run *inside* your main session and share its context), each of these
agents runs as an **independent specialist session** with its own fresh
context, its own model, and its own permissions. They read the pile — the
300-page OEM manual, the 10,000-row CMMS export — in isolation, and send back
only the synthesis. Your main session stays clean.

📖 **Full guide** — what subagents are, skills vs. subagents, anatomy of an
agent file, when (not) to use them:
[robreliability.com/tools/agents-for-claude.html](https://robreliability.com/tools/agents-for-claude.html)

---

## Install (30 seconds)

1. Copy the `.md` files from [`agents/`](agents/) into `.claude/agents/`
   inside your Claude Code project (create the folder if it doesn't exist).
2. That's it. Invoke an agent by name (*"use the rca-challenger agent on this
   RCA"*) or just describe the task — the description triggers it
   automatically.

> **Tip:** run `/agents` in Claude Code to see them, check their
> configuration, and adjust tools / model / memory per agent.

**Recommended memory settings** (via `/agents`):

- All agents → **project memory**, so they inherit your `CLAUDE.md`: failure
  code taxonomy, tag conventions, criticality matrix, site terminology.
  That's what makes the output field-grade instead of generic.
- `rca-challenger` → **memory: `none`**, deliberately. A challenger with no
  memory of your investigation history cannot inherit your confirmation
  bias — the blank context is what makes the critique honest.

## The team

| Agent | What it does | Model | Access |
|---|---|---|---|
| [`oem-manual-extractor`](agents/oem-manual-extractor.md) | Reads an OEM manual or vendor doc in isolation and returns only the reliability-relevant content — failure modes, maintenance tasks with frequencies, MTBF figures, operating limits, critical spares — every item page-cited. | Haiku | Read-only |
| [`cmms-digester`](agents/cmms-digester.md) | Digests raw CMMS work order exports into structured failure histories per equipment tag, mapped to your site failure code taxonomy. Flags what it can't classify — never guesses. | Haiku | Read-only |
| [`weibull-analyst`](agents/weibull-analyst.md) | Fits life data, interprets beta/eta in engineering language (infant mortality / random / wear-out), and translates into maintenance implications. States plainly when the data is insufficient — no false precision. | Sonnet | Read-only |
| [`fmeca-drafter`](agents/fmeca-drafter.md) | Pre-populates a FMECA worksheet — functions, functional failures, modes, effects — from structured history and manual extracts, every row provenance-tagged. **Never scores criticality**: rating belongs to the facilitation session. | Sonnet | Read-only |
| [`bad-actor-hunter`](agents/bad-actor-hunter.md) | Ranks your worst offenders by impact (cost, downtime, frequency, HSE) with the ranking logic explicit and counterintuitive signals surfaced. Top-N with justification, ready for a review. | Sonnet | Read-only |
| [`pmi-builder`](agents/pmi-builder.md) | Turns a strategy line, manual extract, or task description into a field-ready PM procedure — steps, tools, safety/LOTO, acceptance criteria, duration. Written for the shift technician, not the auditor. | Sonnet | Read-only |
| [`rca-challenger`](agents/rca-challenger.md) | Adversarial review of your RCA: attacks the causal chain, lists alternatives not ruled out, flags missing evidence, challenges the stop point. Returns a structured defensibility verdict. | Sonnet | Read-only |

## How they work as a team

One agent is useful. Seven are a workflow:

```
oem-manual-extractor ─┐
                      ├─→  fmeca-drafter ──┐
cmms-digester ────────┤    weibull-analyst ├─→  pmi-builder ──→  rca-challenger
                      └─→  bad-actor-hunter┘    (to the field)    (quality control)
   (feed the data)         (run the analysis)
```

The extractor and digester turn raw piles into structured input. The
analysts turn structured input into findings. The pmi-builder turns
decisions into executable work. The rca-challenger attacks your conclusions
before management does.

Because each runs in its own session, the heavy reading happens in parallel,
on cheap models (Haiku for volume work, Sonnet for judgment work), and none
of it pollutes your main context.

## Why read-only?

If your AI *can* touch data, assume it will. These agents are constrained by
configuration (`tools: read-only` in the front matter), not by polite
prompting. Permissions are configured, not requested — and that's also your
answer when IT security asks how the AI is constrained.

Same logic applies in reverse: a markdown agent file from the internet can
contain prompt injections. Read these files before installing them — they're
short, and that's the point.

## Part of an ecosystem

- 🧠 **The methods:** [18 reliability skills for Claude](https://github.com/Rob-Reliability/reliability-skills-for-claude) · [guide](https://robreliability.com/tools/skills-for-claude.html)
- 🤖 **The team (this repo):** 7 subagents · [guide](https://robreliability.com/tools/agents-for-claude.html)

Skills and subagents compose: a skill running in your main session can fire
off these agents in parallel and assemble their reports.

## Want this wired into your plant?

Each agent is the try-it-yourself version of a Rob Reliability service —
FMECA facilitation, RCA/ECA, PM optimization, bad actor programs, Weibull
studies. If you'd rather have the whole workflow running on your CMMS, your
standards, and your templates:
[book a 30-minute call](https://calendar.app.google/UqjFoSLmXmhJqMQDA) or
email [hello@robreliability.com](mailto:hello@robreliability.com).

---

*Built by Rob Reliability — AI automation for industrial reliability and
maintenance. Obscure AI is the enemy.*
