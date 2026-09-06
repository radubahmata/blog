+++
title = "Task Swarms Blur Roles"
date = "2026-09-06T00:02:00Z"
summary = "Why leader-centric agent swarms do not preserve distinct security-research techniques."
series_key = "Industrializing Security Research"
series_part = 2
+++

_Part 2 of [Industrializing Security Research](/posts/industrializing-security-research/)._

---

## Flagship Swarms Keep a Main Thread

The flagship implementations I checked use different mechanics but share one topology:

- [Claude Code agent teams](https://code.claude.com/docs/en/agent-teams) have a team lead that creates tasks, spawns teammates, and synthesizes their work. Teammates also load the project's skills.
- [Kimi Agent Swarm](https://www.kimi.com/en/help/agent/agent-swarm) uses a commander to direct specialists.
- [Gemini CLI subagents](https://github.com/google-gemini/gemini-cli/blob/main/docs/core/subagents.md) are specialists invoked as tools by the main agent.

The leader provides the main thread. It decides what happens next and assembles the result.

[Part 1 explains why that topology fits coding better than security research](/posts/one-context-is-not-a-security-team/#security-research-runs-backward). Research starts without a known destination, so dividing the work also shapes what each researcher looks for.

---

## Every Redirect Rewrites the Specialist's Context

In my early leader-and-researcher experiments, the leader did more than schedule work. It would:

- Pass hypotheses from one researcher to another.
- Redirect a researcher as results arrived.
- Add its own conclusions to the next prompt.
- Ask a specialist to cover a gap outside its original role.

Each instruction could help the immediate investigation. Across a long session, they made the assigned role negotiable.

A consensus hunter gradually became a general reviewer. A crash hunter inherited someone else's framing. When the full skill suite was also available, either agent could switch techniques without a hard boundary.

---

## Task Coordination Is Not Technique Isolation

These systems coordinate multiple agents. An industrial security-research process needs another property: the ability to repeat a specific technique without a coordinator quietly changing it.

I needed:

- A fixed research role.
- A bounded context.
- Only the techniques required for that role.
- An evidence standard shared across runs.

The model's output can remain nondeterministic. The method should remain recognizable.

The breakthrough was to move the main thread out of a coordinating agent. Git could carry shared state without giving one growing conversation control over every researcher's job.
