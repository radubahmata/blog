+++
title = "One Context Is Not a Security Team"
date = "2026-09-06T00:01:00Z"
summary = "Why a broad suite of security skills makes focused research hard to repeat."
series_key = "Industrializing Security Research"
series_part = 1
+++

_Part 1 of [Industrializing Security Research](/posts/industrializing-security-research/)._

---

In [What's Left When the World Becomes Agentic?](/posts/whats-left-when-world-becomes-agentic/), I left one sentence hanging:

> The agent (well, it's no longer _one_ agent, but that's its own series of posts coming up) reads the archive, learns the shape of it, and reuses it.

This is the beginning of that series.

---

## My First System Was a Skill Suite

My first attempt was not a multi-agent system. It was a suite of skills that one agent could invoke during an interactive security session.

The suite covered the full research lifecycle:

- `context-building` mapped the attack surface.
- `crash-surface` generated node-crash and denial-of-service hypotheses.
- `consensus-invariants` looked for ways honest nodes could disagree.
- `exploitation` turned a candidate into a proof.
- `adversarial-review`, deduplication, and scope triage challenged the completed finding.

Every technique was useful. Making all of them available to every researcher was not.

---

## A Focused Prompt Did Not Fence In the Agent

A focused prompt did not restrict which skills the agent could use. A node-crash hunter could:

1. Map more of the attack surface.
2. Follow a consensus lead.
3. Build the proof.
4. Review and triage its own result.

Steps 1 and 3 support the crash hunt. Step 2 changes the research technique. Step 4 crosses into post-processing. Each transition looked reasonable on its own, but the sequence turned a focused crash hunter into a general researcher reviewing its own work.

---

## Security Research Runs Backward

The difference from coding is structural.

- Coding starts with an intended result and divides the work needed to build it.
- Security research starts with an implementation and works backward through incomplete signals. There is no known path to divide in advance.

A hunter must build the proof. Without it, the agent has found a suspicion, not a vulnerability.

---

## Repeatability Means Repeating the Technique

I was not trying to make an LLM deterministic. Different runs should explore different paths and may find different bugs.

I wanted a higher probability that the work I requested was the work performed:

- A crash hunter traces attacker-controlled input toward panics, resource exhaustion, and liveness failures.
- A consensus hunter tests whether nodes can reach different validity decisions.
- A variant hunter follows the root causes of earlier findings into adjacent code.
- A triager checks every submitted hypothesis after the research agents finish.

The hunter roles can run in parallel. Triage is a conditional next step: it starts only if those researchers submit hypotheses to check.

Each role needs a bounded context, the techniques required for its job, and a clear evidence standard. The result can vary. The method should remain recognizable.

Unexpected work is not free exploration when it leaves the intended attack surface uncovered.
