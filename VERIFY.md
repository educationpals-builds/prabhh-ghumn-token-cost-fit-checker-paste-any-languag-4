# Verification Protocol

This document explains how a stranger can verify that the Token Cost + Fit Checker works as calibrated.

## The Seeded Sample

Paste this exact text into the checker (or any `/verify` endpoint if deployed):

```
"Ihr Krankenversicherungsbeitrag wurde angepasst — bitte prüfen Sie die Beitragsbemessungsgrenze." / "Sigortalılığınızın başlangıç tarihini öğrenebilir miyim?"
```

This is the builder's pinned sample: two verbatim tickets from the queue — one German, one Turkish.

## What to Confirm

1. **Per-lane counts reported**: The checker must report token counts broken out by language lane (German lane, Turkish lane), not just a single total.

2. **Uncounted lane named**: The checker must explicitly name which language lane is *not* being counted or is missing from the analysis. Per the builder's traffic source, the queue contains 38% German, 22% Turkish, 19% English, and remainder Thai / Arabic / Mandarin — but the seeded sample contains only German and Turkish. The checker should note that English (and other languages) are not represented in this sample.

3. **Five dials scored**: The checker should return scores for all five dials:
   - special_token_handling
   - vocabulary_fit
   - merge_economy
   - how_it_splits
   - edge_case_survival

4. **Weakest dial flagged**: The checker should identify `vocabulary_fit` as the weakest dial (or explain its reasoning if it differs).

## Pass Criteria

A verification pass requires:
- [ ] Per-language lane counts appear in the output
- [ ] At least one uncounted or missing lane is named explicitly
- [ ] All five dials return a score (0–4 scale)
- [ ] The output is reproducible on re-paste

## If Verification Fails

If the checker does not report per-lane counts or fails to name the uncounted lane, the calibration may have drifted. Re-run the probe board (see `tests/probe-board.md`) and compare against the certified baseline before shipping.

---

*Verification target: Token Cost + Fit Checker — baw_c002_ch02*
