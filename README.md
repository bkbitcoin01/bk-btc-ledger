# THE BK-BTC LEDGER

A forward, cryptographically timestamped record of what the Boss Method Scoreboard rated highest — sealed the day it was written, published on a fixed schedule, and checkable by anyone without my cooperation.

**It began on 17 September 2026.** Every trading day since, a SHA-256 hash of that day's sealed record has been posted publicly to X and Reddit and anchored to the Bitcoin blockchain. The contents behind those hashes are released on the schedule in [RULE.md](RULE.md) — closed positions 90 days after they close.

---

## What this proves, and what it does not

**It proves integrity and sequence.** That a list of positions and prices was written on the day it claims, has not been altered since, and belongs to an unbroken dated series.

**It does not prove profitability.** No exchange order is placed. Nobody's money is at risk. Prices are the board's own BTC-denominated closes, not fills, and nothing accounts for fees, slippage, or the difference between a quoted price and an executed one. **A perfectly verified record of paper trades is still a record of paper trades.**

This is not financial advice, and nothing here is a recommendation to buy or sell anything.

## Why it is sealed rather than published live

Open positions are what paying members receive. If every position were published the moment it was taken, there would be nothing to subscribe to. That is a commercial choice and it is stated plainly rather than dressed up as something else.

Sealing costs you nothing as a verifier: every line is committed in advance, so when it is revealed you can prove it was written on the day it claims, unchanged. What you cannot do is see it early.

## The rule

The full rule is in **[RULE.md](RULE.md)**. Its hash is anchored to Bitcoin, so it cannot be quietly rewritten later. In short:

- A position is revealed **90 days after it closes**.
- Open positions stay sealed — **except** that any position still open **180 days** after entry is revealed anyway, and the running BTC total (which includes marks on open positions) reveals on the normal schedule. Neither a losing position nor a bad stretch can be hidden by simply never closing it.
- **I do not choose what to reveal.** The rule decides. A loss publishes on the same schedule as a win.
- A missed day is **never backfilled**; the next stamp records the miss and its reason.
- Publication is continuous. A quarterly page summarises it, but nothing waits for that page.

## What is in this repository

| Path | What it is |
|---|---|
| `RULE.md` | The rule, as anchored. Its SHA-256 is published and timestamped to Bitcoin |
| `RULE.md.ots` | The OpenTimestamps proof for `RULE.md` |
| `VERIFY.md` | How to check any of this yourself, in three levels of effort |
| `HASHES.md` | Every daily posted hash, in order, from genesis |
| `2026/` | Reveals and commitment files for 2026, published as they clear the schedule |

Each year gets its own folder inside this repository. The repository is public from day one and accumulates in the open — which is itself part of the evidence. A record that appears fully formed at the end of a year proves far less than one that was built where anyone could watch.

## What to expect, and when

The first positions were opened on **18 September 2026**. Nothing has closed yet, so the first reveal is due 90 days after the first exit. **Until then this repository will look sparse, and that is correct** — `HASHES.md` grows daily, and reveals appear only when the rule releases them.

## Verification in one line

Download a published commitment file, drop it into any SHA-256 tool, and compare the result with the hash posted on X and Reddit for that date. That single check proves the file existed then and has not changed since. [VERIFY.md](VERIFY.md) covers the deeper checks, including the Bitcoin anchor and per-line proofs.

## What would falsify this

- A revealed line that does not verify against its sealed commitment.
- A trading day missing from the sequence with no explanation in a later stamp.
- An item committed and never published after its eligibility date passed — countable, because each day's commitment file is published at reveal and lists every item that day contained.

All three are checkable by anyone. If one of them happens, say so publicly.

---

*The method itself, its code and its internals are not published here and are not part of this record.*

*The Boss Method · Brandon Kelly LLC*
