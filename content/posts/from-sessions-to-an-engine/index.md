+++
title = "From Agentic Sessions to an Engine"
date = "2026-09-10"
summary = "How isolated coding agents, Git coordination, and separate post-processing became an execution engine for continuous security research."
series_key = "Industrializing Security Research"
series_part = 4
+++

_Part 4 of [Industrializing Security Research](/posts/industrializing-security-research/)._

The earlier parts established three constraints:

- [Research roles should repeat a technique](/posts/one-context-is-not-a-security-team/#repeatability-means-repeating-the-technique).
- [Coordination should not rewrite those roles](/posts/task-swarms-blur-roles/#task-coordination-is-not-technique-isolation).
- [Triage should start from a fresh context](/posts/triage-needs-fresh-context/#post-processing-must-be-separate-by-design).

This post covers the execution engine I used to make those constraints concrete.

---

## The Ice-Breaker Test

My first test of the shift, on early March, was deliberately boring. Two dockerized Claude agents read the same project README and committed summaries. A post-processing agent then reviewed the result.

No vulnerabilities. No heroic prompt. Just a test that proved the containers could start, the agents could work independently, Git coordination held up, and the output could be harvested.

It was the multi-agent version of an [ice-breaker test](/posts/ice-breaker-test/). Once that path worked, I replaced the summaries with security objectives and started increasing the pressure.

---

## Overengineered?

At the [interactive skills-suite stage](/posts/one-context-is-not-a-security-team/#my-first-system-was-a-skill-suite), I only knew that I needed stable research identities and fresh context for triage. `claude-swarm` came with much more: Git synchronization, repeated sessions, retries, cost tracking, dashboards, and more.

It looked excessive. Then I used all of it.

The skills suite had exposed the first constraints. Sustained operation exposed the rest.

---

## Agents Coordinate Through Git, Not a Leader

[`claude-swarm`](https://github.com/protocol-security/claude-swarm) removes the coordinator from [the leader-and-workers topology described in Part 2](/posts/task-swarms-blur-roles/#flagship-swarms-keep-a-main-thread). It runs coding agents in isolated Docker containers. The host creates a bare Git repository from the project. Every agent gets its own workspace, but all of them push to the shared repository.

Each agent then runs a simple loop:

1. Fetch the latest shared work.
2. Start a fresh agent session with its assigned prompt.
3. Commit and push any result.
4. Repeat until it stops producing commits.

When one agent publishes useful work, the others see it on a later iteration. They can extend it, challenge it, or avoid repeating it. If two agents collide, Git makes the conflict visible instead of silently choosing a winner.

The tool also implements [the research-and-triage boundary from Part 3](/posts/triage-needs-fresh-context/#post-processing-must-be-separate-by-design). [Post-processing runs as a separate agent and context by design](https://github.com/protocol-security/claude-swarm/blob/e3637afb9a4deb2d6c932fc3852fa98b1f17e79f/USAGE.md#post-processing). It receives committed research, not the researchers' accumulated conversations. The host then harvests the agent branches back into the project.

The agents do not need to remember previous sessions. The durable memory is the Git history.

---

## Isolation Gives Each Technique a Runtime Boundary

[Part 1 defined repeatability as applying the intended research technique across runs](/posts/one-context-is-not-a-security-team/#repeatability-means-repeating-the-technique). Isolation turns that requirement into an execution boundary.

- One agent maps the most critical surface.
- Another hunts a specific impact class.
- Another searches for variants of known findings.

They receive separate prompts and contexts. Their committed work becomes shared context through Git, without a leader redirecting them into different jobs.

This matters because security research produces overlap. Two agents may find the same root cause through different paths. One may find a plausible bug while another finds the guard that kills it. Keeping both lines attributable makes later review much easier.

---

## Every Run Leaves Operational Evidence

The dashboard keeps agent status, sessions, tokens, cache use, throughput, duration, and cost visible. Commits record the model, tool version, run, prompt, and context that produced them.

---

## The Engine Supplies Pieces; Consumers Arrange Them

`claude-swarm` does not prescribe a research process. It provides composable execution pieces:

- Isolated workspaces and container lifecycle
- Git coordination and attribution
- Repeated fresh sessions
- A separate post-processing stage
- Driver, observability, and cost interfaces

The consumer decides how to arrange them: agent identities, prompts, targets, revisions, scheduling, triage policy, and publication.

In my deployment, I used those pieces to build scheduled research, full and diff modes, target rotation, conditional triage, and publication. Those are properties of the consumer layer, not `claude-swarm`. Later posts will cover how they work.

The same boundary let the engine move beyond its historical name. The driver layer now supports Claude Code, Codex CLI, Gemini CLI, Kimi Code, and Qwen Code.

The security-specific consumer layer defines the threat model, evidence bar, prior-art archive, and final authority. I built that layer around the engine. It became the Swarm Standard.

---

## A Sense of the Operating Scale

In roughly six months, my deployment crossed:

- More than 300 runs that produced commits
- More than 1,600 attributable commits
- More than 600 raw candidate reports across Stacks Core, sBTC, and Leather

Almost every candidate went through separate post-processing. A candidate report is not a vulnerability. Many are duplicates, weak claims, invalid threat models, or real defects with overstated impact. [Independent triage matters more as raw output grows](/posts/triage-needs-fresh-context/#triage-needs-the-opposite-mandate).

These are not quality metrics or a benchmark. They show the industrialization level: research running repeatedly across targets, producing enough material that session-by-session handling no longer works.

---

## Built at Protocol Security

[Nikos Baxevanis](https://nikosbaxevanis.com) started `claude-swarm` as part of the [EF Protocol Security team](https://github.com/protocol-security) and led most of its development. It gave us an engine to build on, so we could focus on the security process.

Thanks to Nikos and everyone at Protocol Security for open-sourcing the tool!

I am very happy to have since joined the team.
