# THE BK-BTC LEDGER — REVEAL RULE

---

## 0. What this record proves — and what it does not

**This record proves integrity and sequence. It does not prove profitability.**

It shows that a list of positions and prices was written on the day it claims, has not been altered since, and belongs to an unbroken dated series. That is all cryptography can do.

**No timestamp makes a paper trade real.** No exchange order is placed here. Nobody's money is at risk. Prices are the board's own BTC-denominated closes, not fills, and nothing accounts for fees, slippage, or the gap between a quoted price and an executed one. A perfectly verified record of paper trades is still a record of paper trades.

If you are looking for proof that this method makes money, this document is not it, and I am not claiming otherwise.

## 1. What the record is

- Began **17 September 2026** (genesis), when the rule was sealed *before* any coin was chosen.
- **1 BTC** divided into **3 slots of 33.3%**, each buying the highest-rated coin on the Boss Method Scoreboard not already held.
- A position is held until the system exits it; the freed slot immediately buys the next coin in line.
- Every trading day is stamped in order. See §2 for what happens when a day is missed.

Each day a SHA-256 hash is posted publicly to X and Reddit and anchored to the Bitcoin blockchain via OpenTimestamps. The contents behind that hash stay sealed until §3 releases them.

## 2. Missed days — the outage policy

This runs on one machine. Power cuts, hardware failures and travel happen.

- **A missed day is never backfilled.** A stamp is only ever produced on the day it belongs to.
- **The next stamp records the miss and the reason** — in the envelope, where it is sealed with everything else and revealed on the same schedule.
- **An outage of any length is accounted for in full.** If fourteen days are missed, the next stamp names all fourteen — as an explicit date range with its reason — not merely that "some days" were lost. A partial explanation is treated as no explanation.
- A gap in the public sequence therefore means exactly one of two things: an outage that the next stamp explains, or something I do not want you to see. **Both are visible; only one comes with an explanation attached.**

## 3. The reveal rule

**A position is revealed 90 days after it closes.**

- **Closed positions reveal.** Ninety days after exit, the line is published with its proof.
- **Open positions stay sealed** — that is what paying members receive, stated plainly rather than dressed up.
- **Everything else in a day's record**, including the running BTC total and the code fingerprints, reveals on the same 90-day schedule, tied to the day it was sealed.
- **Publication is continuous; promotion is quarterly.** Each item publishes to the public record automatically as it clears its 90 days — no batching, no waiting for a convenient date. Four times a year (1 January, 1 April, 1 July, 1 October) a consolidated page summarises everything revealed that quarter. **The quarterly page is an announcement, not a release**: nothing is withheld for it, and no item's publication ever waits on it. The first consolidated page is **1 January 2027**.
- **Anti-hiding backstop.** Two rules stop a losing position being hidden by never closing it:
  1. **Any position still open 180 days after entry is revealed anyway**, while still held — named, not just counted in the aggregate.
     180 is chosen against the method's measured hold distribution rather than picked as a round number: across 196 closed historical positions the median hold is 27 days, the 99th percentile is 126 days, and the longest ever recorded is 211 days. A 180-day trigger therefore leaves essentially every legitimate hold undisturbed (1 in 196 would have reached it) while still being a threshold this method can actually cross. A longer trigger — 250 or 365 days — would never have fired on any position in the record, which would make it decoration rather than a safeguard. By the time it fires, the entry is six months old.
  2. **The running BTC total reveals on the normal 90-day schedule regardless** — and it includes the marks on open positions. Sealed losers therefore still show up in the aggregate, on time.
- **Days 1 to 3 (17–19 September 2026)** were sealed before per-line commitments existed and can only be revealed whole. They publish once every position listed in them has closed and cleared 90 days, which will also expose what was open on those days. Stated in advance, deliberately.

**I do not choose what to reveal. The rule decides.** A loss publishes on the same schedule as a win.

## 4. What happens if the project stops

If I stop running the board, or stop publishing, **every reveal already owed is still owed** and will be published on schedule. I will post a dated notice that the record has ended, so the end is a stated fact rather than an unexplained silence.

## 5. How a day is sealed

1. The envelope is written: a plain-text file listing the date, the allocation, the code fingerprints, every open and closed position, and the BTC totals.
2. Each line becomes an **item**, hashed with its own 32-byte secret salt: `leaf = SHA256(salt ‖ item_text)`. Without salts, short lines like `ZEC - day 2 - +5.8%` could be brute-forced by enumeration; the salt is what makes a sealed line sealed.
3. Leaves, in envelope order, form a **Merkle tree**. Odd level → the last node is duplicated.
4. A **commitment file** records the SHA-256 of the whole envelope and the Merkle root.
5. **The SHA-256 of the commitment file is the number posted publicly** and anchored to Bitcoin.

The commitment file is not published **before** reveal, because its item names would disclose when positions close.

**At reveal time the commitment file for that day IS published**, in full. By then the day is at least 90 days old and its closes are being disclosed anyway. This is what makes **completeness** checkable rather than trusted: the published commitment lists every item that day contained, so anyone can count the items that were sealed against the items that have been revealed, and see any item that was committed and never published. Unrevealed items stay sealed — their leaves are only hashes, and without the salt they cannot be brute-forced — but their **existence and number are public**.

**Canonical form.** Everything hashed is UTF-8, LF line endings, no trailing whitespace, and is preserved byte-for-byte as sealed. Verification is performed on those exact bytes.

## 6. Known limits, stated rather than discovered

- **Day 1 (18 September 2026) used stale prices.** Its entries for ZEC, UNI and CAKE were recorded at the **16 September 17:48 board prices**, two days old, because the automated noon chain had never successfully run — a misconfiguration found on 19 September. That day's stamp came from a manual run at 17:03 local, not the noon process the commitment describes. Every return computed from those entry prices inherits the discrepancy. **This note is repeated in the day-1 reveal itself**, so it appears wherever those numbers appear.
- **Slot sizing on a refill** splits the current total evenly across the three slots. It is a track-record approximation, not per-slot compounding accounting, and the running total drifts slightly from a per-slot ledger over many rolls.
- **Prices are the board's noon values**, not the exact exchange print at that minute.
- **Days 1–3 (17, 18, 19 September 2026) use the older all-or-nothing structure** (§3) **and predate the code fingerprints**. The first envelope carrying a fingerprint is 20 September 2026. For those three days the method-change disclosure in §7 has nothing to rest on, and the code that produced them is evidenced only by the project's dated version history, which is weaker: I control that history. Treat the first three days as the least verifiable part of this record.

## 7. Method changes

The method may change. When it does, the envelope's **code fingerprints** — SHA-256 prefixes of the scanner, the backtester and the stamping script — change with it, and that is visible in the sealed record for the day it happened.

- A change is **disclosed, not hidden**: the fingerprints are part of every envelope and reveal on the normal schedule.
- The record therefore may span more than one version of the method. Where that matters to a revealed result, it will be stated in the reveal.

## 8. How to check a reveal

**Level 1 — no tools.** Drag the commitment file into any SHA-256 website. It must equal the hash posted on X and Reddit that day, and the value anchored in Bitcoin. This proves the file existed then and has not changed.

**Level 2 — command line, trusting none of my code.**
```
printf '%s' '<salt>' | xxd -r -p > s.bin
printf '%s' '<line>' > i.txt
cat s.bin i.txt | shasum -a 256          # the leaf
ots verify <date>.commitment.json.ots    # the Bitcoin anchor
```
Combine the leaf with each proof hash in the order given, hashing each pair, and the result is that day's Merkle root. (Use `printf '%s'`, not `echo` — a `%` inside a return like `+50.5%` would otherwise be eaten by the shell.)

Published verification instructions accompany each reveal. If a browser-based verifier is released later, it will be a single static file with its source public — but the method above never depends on it.

## 9. What would falsify this

- **If a revealed line fails to verify against its sealed commitment, that day's record is void** and I will say so publicly, investigate in the open, and publish what happened. A verification failure on one day does not automatically condemn every other day — each day stands or falls on its own proof — but an unexplained failure is a reason to distrust all of it.
- **If a trading day is missing** and no later stamp explains it under §2, treat that gap as unexplained.
- **If an item was committed and its eligibility date has passed without publication**, that is a failure of this rule. The published commitment files (§5) make this countable by anyone: sealed items minus revealed items should leave only items that are not yet eligible.
- **If a quarterly consolidated page omits an item that was published during that quarter**, the page is wrong and the continuously published record governs.
- All of these checks can be run by anyone, without my cooperation.

---

**Rule version:** 1.0 — the first published version of this rule (drafts v1–v4 were pre-publication and were never anchored)
**Effective:** the first stamp after this document's hash is anchored
**Anchored:** SHA-256 of this file, posted to X and Reddit and timestamped to Bitcoin
**Supersedes:** nothing. Days 1–3 keep the structure they were sealed with.
