+++
title = "Shipping to Echidna"
date = "2026-05-14"
summary = "Four PRs into Echidna, the OG smart contract fuzzer that taught the whole space how this is done."
+++

_Writing this one late. Echidna doesn't wait for blog posts - but it deserves one._

[Echidna](https://github.com/crytic/echidna) is the fuzzing OG. Every smart contract fuzzing tool that came after it - [Rendezvous](/posts/rendezvous-clarity-fuzzer/) included - learnt from it.

Landing PRs upstream, then, doesn't feel like contributing. It feels like giving back.

Four PRs in:

- [`#1428`](https://github.com/crytic/echidna/pull/1428)
- [`#1454`](https://github.com/crytic/echidna/pull/1454)
- [`#1484`](https://github.com/crytic/echidna/pull/1484)
- [`#1499`](https://github.com/crytic/echidna/pull/1499)

---

## [`#1428`](https://github.com/crytic/echidna/pull/1428) — Independent Coverage Directory

Coverage reports were getting dumped in the corpus directory. Confusing. Easy to lose, easy to nuke.

Added a `coverageDir` config field and a `--coverage-dir` CLI flag. Falls back to `--corpus-dir` so nothing breaks for existing users. Coverage now lives where it should.

_Shipped in `v2.3.0`._

---

## [`#1454`](https://github.com/crytic/echidna/pull/1454) — Live Shrinking Status

Shrinking ran silent. You stared at the status line and prayed it was still alive - or killed Echidna too early and lost the reproducer.

Each worker now streams progress directly into the status line:

```
shrinking: W2:4592/5000(23)
```

Multi-worker output. Completed workers drop off automatically. Length computed on demand - no extra state. You can watch the shrinker work.

> "Shoutout to @BowTiedRadone for the PR. It's great to have this extra bit of logging to prevent killing Echidna too early while debugging a broken invariant." — [@rappie_eth](https://x.com/rappie_eth/status/1975646950406910315)

_Shipped in `v2.3.0`._

---

## [`#1484`](https://github.com/crytic/echidna/pull/1484) — Foundry Contract Name Collision

Generated Foundry tests crashed `forge`. The generated contract was named `Test` and collided with `forge-std/Test.sol`.

Renamed to `FoundryTest`. Wired up CI to run `forge init` and validate the generated tests compile end-to-end.

Small fix. Unblocked the Foundry path.

_Shipped in `v2.3.1`._

---

## [`#1499`](https://github.com/crytic/echidna/pull/1499) — Foundry Test Support, Across the Board

The big one. Stretched Foundry support across **stateless**, **stateful invariant**, and **assertion** modes.

- Added `assume()` filtering
- Added `assertX` helpers in assertion mode
- Killed the fallback-function bug in test generation
- Killed the null-byte argument bug
- Renamed the legacy `dapptest` naming to `foundry` across the codebase

Foundry users now get first-class treatment.

The `assume()` work also surfaced a bug one layer deeper - in [hevm](https://github.com/argotorg/hevm), the EVM Echidna runs on. `vm.assume(false)` was silently passing instead of reverting with the `FOUNDRY::ASSUME` magic error, which broke input filtering. Reported and tracked in [hevm#963](https://github.com/argotorg/hevm/issues/963). Closed.

_Shipped in `v2.3.2`._

---

## Why This Matters

Echidna is critical infrastructure for the Ethereum smart contract ecosystem. Every contribution upstream sharpens the tool that thousands of auditors and developers already depend on.

This is also the same school of thought that shapes how I think about [Rendezvous](/posts/rendezvous-clarity-fuzzer/) for Clarity: build the fuzzer that the language deserves, then keep sharpening it.

---

## Thanks

A genuine thank you to [@gustavo-grieco](https://github.com/gustavo-grieco), [@aviggiano](https://github.com/aviggiano), and [@rappie](https://github.com/rappie) for the warm welcome and for pointing me to the right resources. Their guidance turned what could have been weeks of ramp-up into landed fixes.
