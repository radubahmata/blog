+++
title = "Rendezvous: Let Clarity Fuzz Test Itself"
date = "2025-12-03"
summary = "From the PoX-4 stateful testing harness to the first native Clarity fuzzer - why we built Rendezvous and the principles it stands on."
+++

Fuzzing Clarity contracts in TypeScript means keeping two mental models in sync - one in TS, one in Clarity. The translation tax wore us down more than the bugs we were chasing.

So we built [Rendezvous](https://github.com/stacks-network/rendezvous): a fuzzer that lets Clarity test itself.

---

## From the TypeScript Harness to Rendezvous

In [The PoX-4 Bug Hunt](/posts/pox-4-bug-hunt/) I walked through the TypeScript stateful property testing harness - every contract command mirrored in a fast-check model. That setup still matters when you need valid signatures, curated wallets, or generators Clarity can't express on its own.

But most of the time, we just wanted to pound on the contract's state machine. Maintaining two languages slowed that down.

Here's how Rendezvous reframes the loop:

![Rendezvous testing flow diagram](/images/rendezvous-testing-diagram.png)

Rendezvous doesn't replace the TypeScript harness - it complements it. The TS rig is outside-in and fully scripted. Rendezvous keeps the inside-the-contract fuzzing honest. Use whichever fits the question you're asking.

---

## Why Let Clarity Test Itself

Clarity has its own integer bounds, principal rules, and asset semantics. Every time we marshaled data in TypeScript, we paid a translation tax. Even when the math worked, the cognitive overhead drained the team.

Rendezvous flips that. Tests stay in Clarity. The fuzzer runs under the same interpreter, the same deterministic semantics, and the same data structures as production contracts. TypeScript stays for orchestration. The fuzzing loop now speaks the language it defends.

The [Rendezvous docs](https://raw.githubusercontent.com/stacks-network/rendezvous/master/docs/chapter_3.md) go deeper into the philosophy.

---

## Two Stateful Modes

Tests live beside the contracts they stress, not in a separate harness. Rendezvous exposes two stateful modes:

- **Property tests** target a single property at a time. You describe it ("`stake` never reduces total locked STX"), Rendezvous generates inputs, exercises it, and shrinks failing cases.
- **Invariant tests** treat the contract as a state machine. All the public functions are in play. Rendezvous composes long random sequences of calls with random arguments. Invariants get checked on every step.

Both live in the same `.tests.clar` file today. The Clarinet team is prioritizing a refactor to simplify this further - [stx-labs/clarinet#2022](https://github.com/stx-labs/clarinet).

Start small, scale as needed. Stateful testing stays front and center either way.

---

## Staying Bilingual with Dialers

Sometimes a test needs more context - events, multiple contracts, wallets with specific mnemonics. **Dialers** are lightweight hooks that let TypeScript run before or after each Clarity step.

Let Clarity own the fuzzing loop. Reach for TypeScript only when you must observe events, inspect external balances, or choreograph actors. The [docs](https://raw.githubusercontent.com/stacks-network/rendezvous/master/docs/chapter_6.md) cover Dialers in detail.

---

## Getting Started

Rendezvous follows a Clarinet-aligned layout: contracts sit beside `{contract}.tests.clar`.

```bash
$ npm install @stacks/rendezvous
$ npx rv --help
```

The layout and CLI options are covered in the [docs](https://raw.githubusercontent.com/stacks-network/rendezvous/master/docs/chapter_5.md). From there, the [Quickstart Tutorial](https://github.com/stacks-network/rendezvous/blob/master/docs/chapter_7.md) is your ice-breaker test - run the sample property test, swap in your own contract, then chase richer invariants.

Stateful fuzzing rewards slow layering: prove the pipeline, then dial up the sequences.

---

## Why This Matters

Rendezvous lets Clarity contracts stress every code path without leaving the language. The TypeScript harness stays ready for orchestration-heavy scenarios. Together they cover both sides of the stateful testing story - one choreographs the world around the contract, the other pounds on it from the inside.

If you're already testing your contracts, this isn't a replacement. It's another tool that lets you go deeper without adding more moving parts.
