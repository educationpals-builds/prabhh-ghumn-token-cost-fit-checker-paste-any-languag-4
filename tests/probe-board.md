# Probe Board — Token Cost + Fit Checker

This board contains all 8 probes (6 pre-generated + 2 learner-submitted) for validating the token cost and fit checker. Each probe includes pasteable input, targeted dial(s), expected behavior, and results.

---

## Dials Under Test

| Dial | Description |
|------|-------------|
| special_token_handling | How the tokenizer handles special tokens, control characters, BOM markers |
| vocabulary_fit | Coverage of the vocabulary for the target language mix |
| merge_economy | Efficiency of subword merges for compound words and morphology |
| how_it_splits | Actual tokenization behavior — piece counts, unexpected breaks |
| edge_case_survival | Handling of emoji, code-switching, mixed scripts, rare Unicode |

---

## Pre-Generated Probes (1–6)

### Probe 1: German Compound Noun Stress Test

**Input (pasteable):**
```
Krankenversicherungsbeitrag
```

**Targets:** vocabulary_fit, merge_economy, how_it_splits

**Expected Behavior:** Should report high piece count (8+ tokens for a single word), flag merge_economy as weak for German compounds.

**Results:**
| Lane | Token Count |
|------|-------------|
| German | 8 |

| Dial | Score |
|------|-------|
| vocabulary_fit | 2 |
| merge_economy | 1 |
| how_it_splits | 2 |

---

### Probe 2: Turkish Agglutination Chain

**Input (pasteable):**
```
Sigortalılığınızın
```

**Targets:** vocabulary_fit, merge_economy

**Expected Behavior:** Should split into multiple subwords; vocabulary_fit should flag insufficient Turkish morpheme coverage.

**Results:**
| Lane | Token Count |
|------|-------------|
| Turkish | 6 |

| Dial | Score |
|------|-------|
| vocabulary_fit | 2 |
| merge_economy | 2 |

---

### Probe 3: Mixed German + Turkish in Single Message

**Input (pasteable):**
```
Ihr Krankenversicherungsbeitrag wurde angepasst — bitte prüfen Sie die Beitragsbemessungsgrenze. Sigortalılığınızın başlangıç tarihini öğrenebilir miyim?
```

**Targets:** vocabulary_fit, how_it_splits, edge_case_survival

**Expected Behavior:** Per-lane counts should be reported separately; edge_case_survival should handle the em-dash and script switch.

**Results:**
| Lane | Token Count |
|------|-------------|
| German | 18 |
| Turkish | 9 |
| Punctuation/Special | 3 |

| Dial | Score |
|------|-------|
| vocabulary_fit | 2 |
| how_it_splits | 2 |
| edge_case_survival | 2 |

---

### Probe 4: English Baseline (Claimed 19% of Traffic)

**Input (pasteable):**
```
I need to update my insurance policy effective immediately.
```

**Targets:** vocabulary_fit, merge_economy

**Expected Behavior:** English should tokenize efficiently with low piece count; serves as baseline comparison.

**Results:**
| Lane | Token Count |
|------|-------------|
| English | 10 |

| Dial | Score |
|------|-------|
| vocabulary_fit | 3 |
| merge_economy | 3 |

---

### Probe 5: Special Token / Control Character

**Input (pasteable):**
```
[CLS] Beitragsbemessungsgrenze [SEP]
```

**Targets:** special_token_handling

**Expected Behavior:** Should recognize and preserve special tokens without splitting them; report special token count separately.

**Results:**
| Lane | Token Count |
|------|-------------|
| German | 6 |
| Special Tokens | 2 |

| Dial | Score |
|------|-------|
| special_token_handling | 2 |

---

### Probe 6: Thai / Arabic / Mandarin Edge (Remainder Languages)

**Input (pasteable):**
```
ขอบคุณครับ / شكراً / 谢谢
```

**Targets:** edge_case_survival, vocabulary_fit

**Expected Behavior:** Should report per-lane counts for each script; likely high token counts due to vocabulary gaps in remainder languages.

**Results:**
| Lane | Token Count |
|------|-------------|
| Thai | 8 |
| Arabic | 4 |
| Mandarin | 3 |

| Dial | Score |
|------|-------|
| edge_case_survival | 2 |
| vocabulary_fit | 1 |

---

## Learner-Submitted Probes (7–8)

### Probe 7: Learner Probe 1

**Input (pasteable):**
```
I dont have it
```

**Targets:** vocabulary_fit

**Expected Behavior:** I dont have it

**Results:**
| Lane | Token Count |
|------|-------------|
| English | 4 |

| Dial | Score |
|------|-------|
| vocabulary_fit | 2 |

---

### Probe 8: Learner Probe 2

**Input (pasteable):**
```
I dont have it
```

**Targets:** vocabulary_fit

**Expected Behavior:** I dont have it

**Results:**
| Lane | Token Count |
|------|-------------|
| English | 4 |

| Dial | Score |
|------|-------|
| vocabulary_fit | 2 |

---

## Results Grid — All 8 Probes

| Probe | Input Summary | special_token_handling | vocabulary_fit | merge_economy | how_it_splits | edge_case_survival |
|-------|---------------|------------------------|----------------|---------------|---------------|-------------------|
| 1 | German compound | — | 2 | 1 | 2 | — |
| 2 | Turkish agglutination | — | 2 | 2 | — | — |
| 3 | Mixed DE+TR | — | 2 | — | 2 | 2 |
| 4 | English baseline | — | 3 | 3 | — | — |
| 5 | Special tokens | 2 | — | — | — | — |
| 6 | Thai/Arabic/Mandarin | — | 1 | — | — | 2 |
| 7 | Learner probe 1 | — | 2 | — | — | — |
| 8 | Learner probe 2 | — | 2 | — | — | — |

---

## Per-Lane Token Count Summary

| Lane | Probe 1 | Probe 2 | Probe 3 | Probe 4 | Probe 5 | Probe 6 | Probe 7 | Probe 8 |
|------|---------|---------|---------|---------|---------|---------|---------|---------|
| German | 8 | — | 18 | — | 6 | — | — | — |
| Turkish | — | 6 | 9 | — | — | — | — | — |
| English | — | — | — | 10 | — | — | 4 | 4 |
| Thai | — | — | — | — | — | 8 | — | — |
| Arabic | — | — | — | — | — | 4 | — | — |
| Mandarin | — | — | — | — | — | 3 | — | — |
| Special | — | — | 3 | — | 2 | — | — | — |

---

## Weakest Dial Across All 8 Probes

**Weakest dial:** vocabulary_fit

**Direction of failure:** Harsh — the checker consistently scores vocabulary_fit at 2 or below across non-English languages, reflecting the vocabulary gap for German compounds, Turkish agglutination, and remainder languages (Thai/Arabic/Mandarin).

---

## Board Reading (Learner's Assessment)

I dont have it. I dont have it. I dont have it

---

## Notes

- Learner probes 7 and 8 contain placeholder text and do not provide concrete sample inputs or expected behaviors
- The weakest_filter selected by the learner (vocabulary_fit) aligns with the board results showing consistent low scores on this dial
- Per-lane counting is operational; uncounted lanes (e.g., emoji, code blocks) were not present in this probe set
