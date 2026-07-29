# Lane Fit Sheet — Calibration Record

This data sheet documents the seeded samples, per-language lane counts, advisor dial strips, drift rulings, and stance adjustments that calibrate the token cost + fit checker.

---

## Seeded Samples

### Sample 1: German Insurance Ticket
```
Ihr Krankenversicherungsbeitrag wurde angepasst — bitte prüfen Sie die Beitragsbemessungsgrenze.
```

### Sample 2: Turkish Insurance Inquiry
```
Sigortalılığınızın başlangıç tarihini öğrenebilir miyim?
```

**Source context:** "Ihr Krankenversicherungsbeitrag wurde angepasst — bitte prüfen Sie die Beitragsbemessungsgrenze." / "Sigortalılığınızın başlangıç tarihini öğrenebilir miyim?" (two verbatim tickets from the queue)

---

## Per-Language Lane Counts

| Lane | Traffic Share | Notes |
|------|---------------|-------|
| German | 38% | Primary lane |
| Turkish | 22% | Secondary lane |
| English | 19% | Claimed but not represented in sample |
| Thai | Remainder | Part of remainder mix |
| Arabic | Remainder | Part of remainder mix |
| Mandarin | Remainder | Part of remainder mix |

**Traffic source:** 14-day support queue export: 38% German, 22% Turkish, 19% English, remainder Thai / Arabic / Mandarin

**Builder's split observation:** I dont see any english reference even though it claims 19% english

---

## Advisor Dial Strips

The five dials scored by the advisor on the seeded samples:

| Dial | Score (0–4) |
|------|-------------|
| special_token_handling | 2 |
| vocabulary_fit | 2 |
| merge_economy | 2 |
| how_it_splits | 2 |
| edge_case_survival | 2 |

**Weakest dial identified:** vocabulary_fit

---

## Builder's Drift Ruling

**Advisor run verdict:** I dont have this information on me right now so I cant provide.

*Note: Drift ruling incomplete — calibration data not provided by builder.*

---

## Stance Line Added

Based on the calibration runs, the following stance governs the advisor:

**Advisor stance:** It can listen to events from CRM for new enteries and it reads the text files for language to translate, and it uploads the translated data to text files on CRM. But it refuses emojis and blacklisted words.

**Explicit refusal:** Emojis and blacklisted words

---

## Cost Measurement Record

**Builder's cost note:** I dont know I am not a language translator

*Note: Per-lane cost measurements incomplete — builder did not provide counted vs uncounted measurement details.*

---

## Calibration Status

This sheet anchors the checker's calibration. The German+Turkish sample pair serves as the verification anchor across:
- README.md (worked example)
- charter.md (pinned sample)
- VERIFY.md (stranger verification)
- tests/probe-board.md (seeded probes)

**Incomplete calibration fields logged:**
- advisor_run_verdict: No dial comparison provided
- cost_note: No measurement details provided
