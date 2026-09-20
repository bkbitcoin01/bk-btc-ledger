# HOW TO VERIFY

Three levels. The first takes a minute and needs no tools. None of them require anything I host.

## Level 1 — does this file date from when he says?

1. Download a published commitment file, e.g. `2026/commitments/2026-09-20.commitment.json`.
2. Drop it into any SHA-256 tool (for example emn178.github.io/online-tools/sha256_checksum.html), or run `shasum -a 256 <file>`.
3. Compare with the hash posted on X and Reddit on that date, listed in [HASHES.md](HASHES.md).

If they match, that exact file existed on that date and has not changed since.

## Level 2 — is that date proven by Bitcoin, not just by a social post?

Each commitment file has an OpenTimestamps proof (`.ots`) beside it.

```
pip install opentimestamps-client
ots verify 2026-09-20.commitment.json.ots
```

This checks the hash against a Bitcoin block. It relies on no website, no company and no account.

## Level 3 — does a revealed line really belong to that sealed day?

A reveal file contains, for each published line: the text, its 32-byte salt, and a Merkle proof.

```
printf '%s' '<salt>' | xxd -r -p > s.bin     # the salt, as raw bytes
printf '%s' '<line>' > i.txt                 # the line, exactly as published
cat s.bin i.txt | shasum -a 256              # this is the leaf
```

Then fold the leaf together with each hash in the proof, in the order given — for a step marked `left`, hash `proof_hash || current`; for `right`, hash `current || proof_hash`:

```
printf '%s%s' '<left>' '<right>' | xxd -r -p | shasum -a 256
```

The final value must equal `merkle_root` in that day's commitment file.

Use `printf '%s'`, not `echo`: a `%` inside a return like `+50.5%` will otherwise be eaten by the shell.

## Checking completeness

Each day's commitment file lists **every** item that day contained, by index and level, with no contents. Count the items marked `CLOSED` against the lines actually published for that day. An item that was committed, is past its eligibility date, and has never appeared is a failure of the rule — and it is countable without my help.

## Canonical form

Everything hashed is UTF-8, LF line endings, no trailing whitespace, and is preserved byte-for-byte as sealed. Verification is performed on those exact bytes. A file that has been re-saved by an editor may hash differently; use the published file as downloaded.
