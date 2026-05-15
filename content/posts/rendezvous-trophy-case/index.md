+++
title = "Rendezvous Gets a Trophy Case"
date = "2026-05-15"
summary = "A thank-you DM turned into a trophy case - a new home for the bugs Rendezvous caught before prod did."
+++

A DM landed. That's the whole reason this post exists.

> Thank you for the work, Radu. You basically saved my life. It is an edge case that I think would have never happened... until it would have. I would have been on the hook for the sum of 0.2% of depositors across both-sided pools.

— [Rapha-btc](https://github.com/Rapha-btc), author of [jing-contracts-v3](https://github.com/Rapha-btc/jing-contracts-v3); the case is written up in the project's [Rendezvous test notes](https://github.com/Rapha-btc/jing-contracts-v3/blob/master/tests/rv/README.md).

Read that last line again. Not a unit test that went red. Real money, both sides of the pools, that would have walked out the door. The edge case that "would never happen" - until it would have.

---

## The Trophy Case

So I built a place to keep these: [Rendezvous PR #259](https://github.com/stacks-network/rendezvous/pull/259).

A trophy case. Bugs [Rendezvous](https://github.com/stacks-network/rendezvous) caught before production did. Receipts.

If Rendezvous saved you a bad day - **submit yours.** [The case is open.](https://github.com/stacks-network/rendezvous/blob/fc1aab2c65f04b4ec88d7163dfd6a5c193607389/README.md#trophy-case) The whole point is to show, in public, what didn't make it to mainnet.

---

## Why It Matters

Rendezvous helps you not lose money in prod, and it makes you discover the ugly things fast - even while you're still just testing.

That's how the edge case above surfaced. Discovered with **Rendezvous `v1.0.0-rc1`**.

And it doesn't stop at fresh contracts. With the [simnet remote data](https://www.hiro.so/blog/test-your-code-with-mainnet-data-in-clarinet-simnet) feature Rendezvous accepts, you can point it at your _already-deployed_ contract version and ask the uncomfortable question: from this specific block height, what can actually happen to it? Mainnet state, real history, fuzzed forward.

Rendezvous V1 is just around the corner. More on that in a future post.

---

Got a trophy? [The case is here.](https://github.com/stacks-network/rendezvous/blob/fc1aab2c65f04b4ec88d7163dfd6a5c193607389/README.md#trophy-case)
