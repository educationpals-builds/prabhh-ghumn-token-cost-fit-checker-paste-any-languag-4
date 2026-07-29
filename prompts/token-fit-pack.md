# Token Fit Prompt Pack

Five standalone prompts for evaluating token cost and vocabulary fit. Each prompt targets one dial and can be used in any chat model. Paste the sample text where indicated.

---

## 1. Special Token Handling (Dial: `special_token_handling`)

```
You are a token analyzer focused on special token handling.

Given the following text sample, analyze how a tokenizer would handle special tokens, control characters, and boundary markers.

TEXT SAMPLE:
[paste sample here]

Report:
1. Count any special characters, control sequences, or unusual Unicode points
2. Identify tokens that would require special handling (BOS, EOS, PAD, UNK)
3. Rate special token handling on a 0–4 scale:
   - 0: Catastrophic — special tokens corrupt output
   - 1: Poor — frequent UNK tokens or mishandled boundaries
   - 2: Acceptable — some special tokens handled, some missed
   - 3: Good — most special tokens handled correctly
   - 4: Excellent — all special tokens handled cleanly

Output your rating and a one-sentence justification.
```

---

## 2. Vocabulary Fit (Dial: `vocabulary_fit`)

```
You are a token analyzer focused on vocabulary coverage.

Given the following text sample, analyze how well a standard multilingual vocabulary would cover the tokens.

TEXT SAMPLE:
[paste sample here]

Report:
1. Identify language(s) present in the sample
2. Flag any domain-specific terms, compound words, or rare vocabulary
3. Estimate out-of-vocabulary rate for a typical multilingual tokenizer
4. Rate vocabulary fit on a 0–4 scale:
   - 0: Catastrophic — majority of tokens out of vocabulary
   - 1: Poor — significant vocabulary gaps, many fallbacks
   - 2: Acceptable — some gaps but core meaning preserved
   - 3: Good — minor gaps, domain terms mostly covered
   - 4: Excellent — full vocabulary coverage

Output your rating and a one-sentence justification.
```

---

## 3. Merge Economy (Dial: `merge_economy`)

```
You are a token analyzer focused on merge efficiency.

Given the following text sample, analyze how efficiently a BPE or similar tokenizer would merge subwords.

TEXT SAMPLE:
[paste sample here]

Report:
1. Identify long compound words or agglutinative constructions
2. Estimate how many merges would be needed vs. character count
3. Flag any sequences that would resist efficient merging
4. Rate merge economy on a 0–4 scale:
   - 0: Catastrophic — extreme token bloat, no useful merges
   - 1: Poor — inefficient merging, high token-to-character ratio
   - 2: Acceptable — moderate efficiency, some bloat
   - 3: Good — efficient merging for most sequences
   - 4: Excellent — near-optimal merge economy

Output your rating and a one-sentence justification.
```

---

## 4. How It Splits (Dial: `how_it_splits`)

```
You are a token analyzer focused on split behavior.

Given the following text sample, analyze how a tokenizer would split the text and whether splits preserve meaning.

TEXT SAMPLE:
[paste sample here]

Report:
1. Identify words that would split at semantically awkward boundaries
2. Count approximate token pieces for the longest word
3. Flag any splits that would harm downstream tasks (NER, translation)
4. Rate split behavior on a 0–4 scale:
   - 0: Catastrophic — splits destroy meaning
   - 1: Poor — frequent bad splits, meaning degraded
   - 2: Acceptable — some awkward splits but recoverable
   - 3: Good — splits mostly preserve semantic units
   - 4: Excellent — clean splits aligned with morphology

Output your rating and a one-sentence justification.
```

---

## 5. Edge Case Survival (Dial: `edge_case_survival`)

```
You are a token analyzer focused on edge case handling.

Given the following text sample, analyze how a tokenizer would handle edge cases: mixed scripts, code-switching, numbers, punctuation clusters, or unusual formatting.

TEXT SAMPLE:
[paste sample here]

Report:
1. Identify any script mixing or code-switching
2. Flag numbers, dates, currency, or domain-specific formats
3. Note punctuation clusters or unusual whitespace
4. Rate edge case survival on a 0–4 scale:
   - 0: Catastrophic — edge cases cause failures
   - 1: Poor — many edge cases mishandled
   - 2: Acceptable — common edge cases handled, rare ones fail
   - 3: Good — most edge cases handled gracefully
   - 4: Excellent — robust across all edge cases

Output your rating and a one-sentence justification.
```

---

## Calibration Reference

These prompts were calibrated against the following sample:

> "Ihr Krankenversicherungsbeitrag wurde angepasst — bitte prüfen Sie die Beitragsbemessungsgrenze." / "Sigortalılığınızın başlangıç tarihini öğrenebilir miyim?" (two verbatim tickets from the queue)

**Traffic source:** 14-day support queue export: 38% German, 22% Turkish, 19% English, remainder Thai / Arabic / Mandarin

**Weakest dial identified:** vocabulary_fit

---

## Usage

1. Copy one prompt into any chat model
2. Replace `[paste sample here]` with your text
3. Record the 0–4 rating
4. Repeat for all five dials
5. The lowest-scoring dial is your constraint

For the full conversational checker, see `blueprints/token-fit-checker.md`.
