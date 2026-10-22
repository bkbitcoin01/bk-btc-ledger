# THE BK-BTC LEDGER — RULE

**Dated 18 October 2026 · Version 2.0 · Run 3**

This is the rule of run 3 of the BK-BTC Ledger. It is dated the day it was written so that anyone can see when it was created and what it binds the record to. The public record is one unbroken series of sealed days from 17 September 2026: run 1 (genesis 17 September 2026, closed 11 October 2026) and run 2 (genesis 12 October 2026) both ran under RULE.md 1.0, and this book is the first to number the runs in public. Run 2 ended sealed at its last stamp under RULE.md 1.0; every reveal it owes is still owed and will be published on that rule's schedule. Run 3 begins at 1.000 BTC under this book.

This book differs from RULE.md 1.0 in four things only: the account's idle cash may sit in dollars (§1), the daily envelope carries six new lines (§5), the method's rules are held in a sealed private rulebook (§7), and the slot-sizing sentence in §6 now says what the ledger does: per-slot compounding, as it has since run 2. Everything else is RULE.md 1.0's text. Each addition is marked *(new in this book)* and the one correction *(changed in this book)* where it appears.

---

## 0. What this record proves — and what it does not

**This record proves integrity and sequence. It does not prove profitability.**

It shows that a list of positions and prices was written on the day it claims, has not been altered since, and belongs to an unbroken dated series. That is all cryptography can do.

**No timestamp makes a paper trade real.** No exchange order is placed here. Nobody's money is at risk. Prices are the board's own BTC-denominated closes, not fills, and nothing accounts for fees, slippage, or the gap between a quoted price and an executed one. A perfectly verified record of paper trades is still a record of paper trades.

If you are looking for proof that this method makes money, this document is not it, and I am not claiming otherwise.

## 1. What the record is

- **Genesis** is the first stamp after this document's hash is anchored, with the run starting at **1.000 BTC**. The genesis envelope names this book's version and the version of the method's private rulebook in force (§7). *(new in this book)*
- **1 BTC** divided into **3 slots of 33.3%**, each buying the coin the method rates highest that it does not already hold.
- A position is held until the method exits it. A freed slot buys the next coin in line when the method allows a buy; the method may also decline to buy for a time, and says nothing public about when or why. *(new in this book; RULE.md 1.0 said the freed slot buys immediately)*
- **The account's idle cash sits in Bitcoin or in a dollar stablecoin (USDT), as the method's rule directs.** While it sits in dollars, no new coin is bought. Every move between the two is a dated, logged event, recorded with the amount, the exchange rate used and its source, sealed with the day and revealed on the schedule in §3. The record stays denominated in Bitcoin: parked dollars are valued in Bitcoin at the day's rate, and the dollar value of the whole account is shown beside the Bitcoin total, never in its place. *(new in this book)*
- **The record is gross.** No fee, slippage or conversion cost is computed or shown. *(new in this book)*
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
- **Everything else in a day's record**, including the running BTC total, the dollar equivalent, every idle-cash move and the code fingerprints, reveals on the same 90-day schedule, tied to the day it was sealed. *(the dollar equivalent and the idle-cash moves are new in this book)*
- **Publication is continuous; promotion is quarterly.** Each item publishes to the public record automatically as it clears its 90 days — no batching, no waiting for a convenient date. Four times a year (1 January, 1 April, 1 July, 1 October) a consolidated page summarises everything revealed that quarter. **The quarterly page is an announcement, not a release**: nothing is withheld for it, and no item's publication ever waits on it. The first consolidated page is **1 January 2027**.
- **Anti-hiding backstop.** Two rules stop a losing position being hidden by never closing it:
  1. **Any position still open 180 days after entry is revealed anyway**, while still held — named, not just counted in the aggregate.
     180 is chosen against the method's measured hold distribution rather than picked as a round number: across 196 closed historical positions the median hold is 27 days, the 99th percentile is 126 days, and the longest ever recorded is 211 days. A 180-day trigger therefore leaves essentially every legitimate hold undisturbed (1 in 196 would have reached it) while still being a threshold this method can actually cross. A longer trigger — 250 or 365 days — would never have fired on any position in the record, which would make it decoration rather than a safeguard. By the time it fires, the entry is six months old.
  2. **The running BTC total reveals on the normal 90-day schedule regardless** — and it includes the marks on open positions. Sealed losers therefore still show up in the aggregate, on time.

**I do not choose what to reveal. The rule decides.** A loss publishes on the same schedule as a win.

## 4. What happens if the project stops

If I stop running the board, or stop publishing, **every reveal already owed is still owed** and will be published on schedule. I will post a dated notice that the record has ended, so the end is a stated fact rather than an unexplained silence.

## 5. How a day is sealed

1. The envelope is written: a plain-text file listing the date, the allocation, the code fingerprints, every open and closed position, and the BTC totals — the total beside its two parts, the realized total and the unrealized total (realized: the sum of the closed legs' returns, in BTC; unrealized: the open positions (a coin, or idle cash parked in dollars) marked against their entry prices, in BTC; the total is never shown without both). It also lists the cash position in Bitcoin and in dollars, the day's exchange rate and its source, each idle-cash move of the day, and the version of the method's private rulebook in force. *(the two parts and the last four are new in this book)*
2. Each line becomes an **item**, hashed with its own 32-byte secret salt: `leaf = SHA256(salt ‖ item_text)`. Without salts, short lines like `ZEC - day 2 - +5.8%` could be brute-forced by enumeration; the salt is what makes a sealed line sealed.
3. Leaves, in envelope order, form a **Merkle tree**. Odd level → the last node is duplicated.
4. A **commitment file** records the SHA-256 of the whole envelope and the Merkle root.
5. **The SHA-256 of the commitment file is the number posted publicly** and anchored to Bitcoin.

The commitment file is not published **before** reveal, because its item names would disclose when positions close.

**At reveal time the commitment file for that day IS published**, in full. By then the day is at least 90 days old and its closes are being disclosed anyway. This is what makes **completeness** checkable rather than trusted: the published commitment lists every item that day contained, so anyone can count the items that were sealed against the items that have been revealed, and see any item that was committed and never published. Unrevealed items stay sealed — their leaves are only hashes, and without the salt they cannot be brute-forced — but their **existence and number are public**.

**Canonical form.** Everything hashed is UTF-8, LF line endings, no trailing whitespace, and is preserved byte-for-byte as sealed. Verification is performed on those exact bytes.

## 6. Known limits, stated rather than discovered

- **Slot sizing on a refill** is per-slot compounding: a freed slot buys its next coin with the cash it came back with, so each slot's gains and losses carry into its next coin; when more than one slot is filled at the same noon, the cash on hand is split evenly across the slots being filled. While the idle cash is parked in dollars there is no refill. *(changed in this book: RULE.md 1.0 described an even split of the current total across the three slots; the ledger has compounded per slot since run 2)*
- **Prices are the board's noon values**, not the exact exchange print at that minute.
- **The exchange rate for an idle-cash move** is one source's value at or before noon, not an exchange fill. A move's value in a later reveal is that rate and no other. *(new in this book)*

## 7. Method changes — and the private rulebook

The method may change. When it does, the envelope's **code fingerprints** — SHA-256 prefixes of the scanner, the backtester and the stamping script — change with it, and that is visible in the sealed record for the day it happened.

- A change is **disclosed, not hidden**: the fingerprints are part of every envelope and reveal on the normal schedule.
- The record therefore may span more than one version of the method. Where that matters to a revealed result, it will be stated in the reveal.

*(new in this book, from here to the end of §7)*

**The method's rules are private.** They are written in full in a **private rulebook** with its own version number. On genesis day the private rulebook is sealed exactly as a daily envelope is: each line salted and hashed, the lines formed into a Merkle tree, the commitment hashed. The hash of that commitment is printed in this book's footer and carried in the genesis envelope. That proves the rules existed on this date and have not changed since, without showing them. Each day's envelope names the version of the private rulebook in force.

**Two kinds of change, and what each one does:**

- **A change to how the method decides** (any rule held in the private rulebook) produces a new version of the private rulebook, sealed the same way, and a new dated edition of this book carrying its hash, announced and anchored on the day it takes effect. **The run continues** from where it stands.
- **A change to anything this book states** (the account and its three slots, the idle-cash condition, the denomination in Bitcoin, the gross record, or the rules for sealing, revealing and checking) **ends the run**. The run ends sealed at its last stamp, with every reveal it owes still owed, and a new run begins at 1.000 BTC under a new dated book.

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

**Rule version:** 2.0, dated 18 October 2026
**Effective:** the first stamp after this document's hash is anchored
**Anchored:** SHA-256 of this file, posted to X and Reddit and timestamped to Bitcoin
**Supersedes:** RULE.md 1.0, for run 3 onward. Every day sealed before run 3's genesis keeps the rule it was sealed under.
**Private rulebook hash:** 4005ee2143768519ee7a8608ad578fb2bf24ece831461b7ff2ee6d7a2a2e4f96
