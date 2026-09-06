+++
title = "Researchers Should Not Triage Their Own Findings"
date = "2026-09-06T00:03:00Z"
summary = "Why security findings need a fresh context with the opposite mandate."
series_key = "Industrializing Security Research"
series_part = 3
+++

_Part 3 of [Industrializing Security Research](/posts/industrializing-security-research/)._

---

## Research Builds Commitment to a Hypothesis

A research agent spends its context constructing one case. It has already:

- Selected the suspicious path.
- Interpreted ambiguous code in that path.
- Built a proof of concept.
- Argued for reachability and impact.

That work is necessary. It also gives the agent a frame to defend.

Asking the same agent to review its report does not reset that history. The review begins from the researcher's assumptions and tends to refine the case instead of rebuilding it.

---

## Triage Needs the Opposite Mandate

Research and triage should pull in opposite directions.

A triage agent should:

- Reopen the cited code from a clean context.
- Look for guards and constraints the researcher missed.
- Reconstruct the attack path instead of trusting the report.
- Challenge reachability, impact, and severity.
- Check scope, duplicates, and prior art.

The goal is not to improve the argument. The goal is to find the first fact that breaks it.

---

## Fresh Context Is an Engineering Control

A fresh agent is not magically unbiased. It simply has no research history to preserve.

That separation gives each role a clear success condition:

- The researcher succeeds by proving the candidate.
- The triage agent succeeds by disproving or correctly narrowing it.

Disagreement becomes useful evidence. If the finding survives both mandates, it deserves human attention.

---

## Post-Processing Must Be Separate by Design

This boundary cannot depend on the researcher remembering to change hats. [`claude-swarm` runs post-processing as a separate agent and context](https://github.com/protocol-security/claude-swarm/blob/e3637afb9a4deb2d6c932fc3852fa98b1f17e79f/USAGE.md#post-processing).

The researcher commits a candidate. The post-processor receives the committed work, not the conversation that produced it. Its conclusions are attributable to a different run and prompt.

**Research tries to prove the finding. Triage tries to kill it.**
