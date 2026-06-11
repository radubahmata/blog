+++
title = "What's Left When the World Becomes Agentic?"
date = "2026-06-11"
summary = "A retroactive look at the last six months: the question I kept dodging, and the answer I landed on."
+++

_Writing this one retroactively. The shift already happened. The question just took a while to name._

It started as a quiet dread.

Was all the knowledge I accumulated worthless? [AFL++](https://github.com/AFLplusplus/AFLplusplus). [libFuzzer](https://llvm.org/docs/LibFuzzer.html). [proptest](https://github.com/proptest-rs/proptest). [fast-check](https://github.com/dubzzz/fast-check). [Echidna](/posts/shipping-to-echidna/). [Rendezvous](/posts/rendezvous-clarity-fuzzer/). [Clarity stateful PBT](/posts/pox-4-bug-hunt/). [Schemathesis](https://github.com/schemathesis/schemathesis). And the rest of the rig. The advanced testing techniques I'd spent years sharpening, or even exploring - did they have a shelf life now that agents could read code faster than I could open a file?

What happens to security research when the world goes agentic?

I didn't answer that question for a while. I just kept working.

---

## The Punchline First, Since This Is Retroactive

A lot of what's in my handbook is already well-known worldwide. So here's where I landed, plainly:

**AI is not taking over.** AI is the binder. The thing that takes every piece of knowledge I've already built up - fuzzing rigs, invariant patterns, the muscle memory from chasing real bugs - and stitches it into something I can _scale_.

Nothing I learned was wasted. Quite the opposite. The deeper the prior knowledge, the sharper the agent that wraps around it.

---

## January: The Bet

Back in January, I was back at the Stacks Foundation. I'd automated the internal fuzzers we were running - the rigs were green, the pipelines clean. Saturated.

Around the same time, the AI slop started flowing in from ImmuneFi. Reports that read fluent and meant nothing. All of them discarded.

But the signal was obvious: it's a matter of time. Garbage today, real findings tomorrow. I needed to get ahead of the trend, not get buried by it. Lucky for me, [Jesse Wiley](https://github.com/wileyj) backed the bet early and made the runway possible.

Meanwhile, the hype around Claude Code was climbing fast. People were vibing prototypes into existence. Pretty demos.

But security research as a reverse-engineering discipline? Not there yet. Far too few had pointed these agents at a real codebase and gotten a real bug back.

So I asked the dumb question: _what if?_

What if I wrote a basic `AGENTS.md` that searched for vulnerabilities?

The first version was bad. False positives everywhere. The agent would flag a constant-time comparison as a timing side-channel and then, on the next file, miss the kind of bug that actually matters - a malformed dust transaction that crashes every node in a DLT and turns a 0.0001-something fee into a reboot-persistent, network-wide outage - the kind that doesn't lift until a new release ships.

I kept going.

---

## March: Simone Joined

Around March, [Simone Orsi](https://github.com/orsissimo) joined this effort.

I brought the security background - years of fuzzing, stateful invariants, the muscle memory of chasing bugs that don't want to be found. He brought a terrifying passion for AI and automation. The kind that doesn't sleep.

Those two things, pointed at the same target, compounded.

The false-positive rate dropped. The agents stopped flailing and started finding. The handbook stopped being _mine_ and started being _ours_ - which, it turns out, is the only way this actually scales.

---

## Where That Bet Sits Today

The same loop, refined for six months, now carries real weight on the protocol I'm paid to secure:

- **6 extreme criticals**
- **11 high-severity vulnerabilities**
- More mediums and lows than I've bothered to count

Caught early. Before they could ship anywhere they shouldn't.

Alongside that, we built a live archive of findings - a corpus the agent (well, it's no longer _one agent_, but a crazy-ass tool that deserves its own series of posts coming up) reads, learns the shape of, and reuses. The archive doubles as our de-duplication layer: when an external bounty report lands, we can prove we already had it. **Roughly 90% of incoming external reports** match something we'd already caught internally.

That was the right move. Not the prompt-engineering part. The _archive_ part. The accumulated knowledge, made queryable.

Glad that [Stacks Labs](https://x.com/stackslabs) believed in the effort, picked it up, and gave us resources to keep it going - and, now, to industrialize it.

---

## What I'm Doing Now

Scanning is amazing. Once the prompts are dialed in, there's not much left to do but hit run. Keep that.

Then expand _input_ coverage. The next level is _scaling_ security research - not "find me a bug in this file", not "review this PR".

Instead: hand the advanced testing techniques to agentic harnesses and let them run at a scale no human team could match. Fuzzers. Stateful property tests. Invariant suites. Driven across the whole protocol, every commit, every branch, around the clock.

The goal hasn't changed: **security for degens and the protocols they're trusting with their money**. The mechanism has. AI is the leverage. Advanced testing techniques are the weapon. The knowledge is still mine.

---

The low-hanging fruit gets patched. Surface-level bugs get scanned out by every agent on the market. What's left underneath is the deep work - state machines, invariants, cross-contract races, consensus edge cases. Those don't surface from reading code. They surface from _exercising_ it.

> **Advanced testing techniques are not the past. They are the future.**

That's what's left when the world becomes agentic. And that's where (my) agents are going next - guided by the people who already know how to break a system from the inside out.
