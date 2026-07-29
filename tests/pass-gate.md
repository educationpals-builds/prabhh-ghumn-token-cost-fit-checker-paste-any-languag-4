# Pass Gate

The acceptance gate this checker must hold before shipping.

---

## Gate Definition

**Metric + Threshold + Re-run Cadence (builder's specification):**

> I dont have it. I dont have it. I dont have it

---

## Contested-Call Rulings

When the checker and a human reviewer disagree on a dial score, the ruling is recorded here with Atlas's opposing case preserved.

| Sample ID | Dial | Checker Score | Human Score | Ruling | Atlas's Opposing Case |
|-----------|------|---------------|-------------|--------|----------------------|
| *(No contested calls recorded — gate specification incomplete)* | — | — | — | — | — |

---

## Re-run Trigger

The gate should be re-evaluated when:

1. The prompt or stance changes in `blueprints/token-fit-checker.md` or `skills/token-fit-advisor.skill.md`
2. New probes are added to `tests/probes.jsonl`
3. The traffic mix in the source stream shifts significantly from the original distribution (38% German, 22% Turkish, 19% English, remainder Thai / Arabic / Mandarin)

---

## Current Gate Status

**Status:** INCOMPLETE — builder did not provide metric, threshold, or re-run cadence.

To complete this gate, specify:
- A measurable metric (e.g., "dial agreement rate across all 8 probes")
- A numeric threshold (e.g., "≥ 7 of 8 probes return expected dial behavior")
- A re-run cadence (e.g., "weekly, or on any prompt change")

---

## Related Files

- `tests/probes.jsonl` — machine-readable probe definitions
- `tests/probe-board.md` — human-readable probe results grid
- `tests/run-local.md` — instructions for running the gate locally
