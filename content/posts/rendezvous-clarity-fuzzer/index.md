+++
title = "Rendezvous: Let Clarity Fuzz Test Itself"
date = "2025-12-03"
summary = "From the PoX-4 stateful testing harness to the first native Clarity fuzzer, here’s why we built Rendezvous and the principles it stands on."
+++

## From the TypeScript Stateful Property Testing Harness to Rendezvous

In [The PoX-4 Bug Hunt](/posts/pox-4-bug-hunt/) I walked through the TypeScript stateful property testing harness that mirrored every contract command in a fast-check model. That setup still matters: if you need valid signatures, curated wallets, or any custom generator that Clarity alone can’t express easily, stay there.

Most of the time, we just wanted to pound on the contract’s state machine. Duplicating logic across two languages slowed that down, so we built [Rendezvous](https://github.com/stacks-network/rendezvous).

![Rendezvous testing flow diagram](/images/rendezvous-testing-diagram.png)

Rendezvous doesn’t replace the stateful property testing harness; it complements it. Think of the TypeScript rig as the outside-in, fully scripted sibling, while Rendezvous keeps the inside-the-contract fuzzing honest. Use whichever path fits the question you’re asking.

## Why Let Clarity Test Itself

Under the hood, Clarity has its own integer bounds, principal rules, and asset semantics. Every time we marshaled data in TypeScript we had to keep two mental models in sync: one in TS, one in Clarity. Even when the math worked, that translation tax wore the team down more than the bugs we were chasing.

Rendezvous flips that. Tests stay in Clarity, so the fuzzer runs under the same interpreter, the same deterministic semantics, and the same data structures as production contracts. We keep the TypeScript layer for orchestration, but the day-to-day fuzzing loop now speaks the language it defends. If you want the longer version, the [Rendezvous docs](https://raw.githubusercontent.com/stacks-network/rendezvous/master/docs/chapter_3.md) go deeper into this philosophy.

## Principles and Stateful Testing Modes

Rendezvous keeps the methodology simple and Clarity-first: tests live beside the contracts they stress, not in a separate harness. From there the tool exposes two stateful modes that matter most for fuzzing: **property-based tests** and **invariant tests**.

- **Property tests** hit a single function. You describe a property (“`stake` never reduces total locked STX”), and Rendezvous generates inputs, runs the function, and shrinks failing cases.
- **Invariant tests** treat the contract as a state machine. You choose the public functions that are in play, Rendezvous composes long random sequences of calls with random arguments, and invariants are checked on every step.

Both live in the same `.tests.clar` file today, with an upcoming refactor our friends on the Clarinet team are prioritizing ([stx-labs/clarinet#2022](https://github.com/stx-labs/clarinet)) to simplify things even further. Start small, scale as needed; stateful testing stays front and center either way.

## Staying Bilingual with Dialers

Sometimes the test needs more context—events, multiple contracts, or wallets with specific mnemonics. For that, Rendezvous comes with **Dialers**: lightweight hooks that let TypeScript run before or after each Clarity step.

Dialers keep the “bilingual” workflow intact. Let Clarity own the fuzzing loop, and hook TypeScript only when you must observe events, inspect external balances, or choreograph actors. It’s the same partnership we used on PoX, just better defined (there’s a dedicated section in the [docs](https://raw.githubusercontent.com/stacks-network/rendezvous/master/docs/chapter_6.md) if you want to dig into Dialers in detail).

## Getting Started Quickly

Rendezvous follows a Clarinet-aligned layout: contracts sit beside `{contract}.tests.clar`, and you install the tool with `npm install @stacks/rendezvous`. A quick `npx rv --help` confirms the CLI is ready (the layout and options are covered in more depth in the [docs](https://raw.githubusercontent.com/stacks-network/rendezvous/master/docs/chapter_5.md)).

From there, the quickstart acts as your ice-breaker test. Run the sample property test, swap in your own contract, and only then chase richer invariants ([Quickstart Tutorial](https://github.com/stacks-network/rendezvous/blob/master/docs/chapter_7.md)). Stateful fuzzing rewards slow layering: prove the pipeline, then dial up the sequences.

## Closing Thoughts

Rendezvous exists so Clarity contracts can stress every code path without leaving the language, while the original TypeScript harness stays ready for orchestration-heavy scenarios. Together they cover both sides of the stateful testing story: one choreographs the world around the contract, the other pounds on the contract from inside.

If you’re already testing your contracts, this isn’t a replacement; it’s another tool that lets you go deeper without adding more moving parts. Use the harness when you need a full environment, reach for Rendezvous when you want Clarity itself to do the heavy lifting.

There’s much more coming on Rendezvous: deeper dives into real-world test files, patterns for invariants, and lessons learned as it gets picked up by the community.
