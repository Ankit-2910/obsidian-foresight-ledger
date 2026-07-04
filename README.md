# OBSIDIAN Proof-of-Foresight Ledger

**Verify, don't trust.** Every prediction OBSIDIAN makes is locked
(`is_locked=TRUE`) the instant an intelligence event is detected. Each day,
all of that day's predictions are hashed into a Merkle tree and the root is
committed HERE, to a public repository whose commit timestamps OBSIDIAN
cannot alter. Anyone can prove a prediction shown today is byte-identical
to what was locked months ago.

## How to verify a prediction

1. `GET https://obsidian.shivanchal.in/api/ledger/proof/<prediction_id>` —
   returns the canonical JSON, its leaf hash, the Merkle path, and the day's
   anchored root.
2. Recompute yourself: `leaf = sha256(canonical_json)`; fold the Merkle path
   (`sha256(left + right)` at each step); compare against `merkle_root` in
   `anchors/<day>.json` **in this repo**, and check the commit date of that
   file.
3. Chain integrity: every anchor contains `previous_chain`, and
   `chained_hash = sha256(previous_chain + day + merkle_root + count)` —
   so no historical anchor can be swapped without breaking every anchor
   after it.

## Canonical serialization

Sorted-key, compact JSON of:
`id, event_id, prediction_type, predicted_value, confidence_score,
horizon_days, model_version, made_at (UTC ISO)`

## Honesty notes

- Anchors marked `"retroactive": true` were computed after the day they
  cover (the one-time backfill of predictions made before this ledger
  existed on 2026-07-03). They prove integrity from their commit date
  forward, not same-day foresight. All anchors from 2026-07-04 onward are
  committed same-day, automatically, at 06:25 IST.
- Anchor files are never overwritten. The publisher refuses to touch an
  existing file; git history would expose any attempt anyway.

---
OBSIDIAN Intelligence · SHIVANCHAL CONSULTANTS · https://obsidian.shivanchal.in
