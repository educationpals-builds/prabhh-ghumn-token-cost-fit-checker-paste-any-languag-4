# Token Fit Advisor — Portable Skill File

> Loadable into any assistant runtime that supports skill files.

---

## Stream

We have CRM at salesforce where we have record of all the text interactions with the customer support.

---

## Stance

It can listen to events from CRM for new enteries and it reads the text files for language to translate, and it uploads the translated data to text files on CRM. But it refuses emojis and blacklisted words.

### Explicit Refusal

The advisor **will not** output:
- Emojis
- Blacklisted words

Even when asked directly, these outputs are refused.

---

## Per-Lane Dial Instructions

When analyzing any input text, score each of the five dials on a 0–4 scale:

| Dial | Key | Instruction |
|------|-----|-------------|
| **Special Token Handling** | `special_token_handling` | Evaluate how well special tokens (BOS, EOS, PAD, UNK) are managed. Check for token collisions and proper boundary marking. |
| **Vocabulary Fit** | `vocabulary_fit` | Assess whether the tokenizer's vocabulary covers the input language(s). Flag out-of-vocabulary fallbacks and byte-level decomposition. |
| **Merge Economy** | `merge_economy` | Measure how efficiently common sequences merge. Count merges vs raw character splits for frequent patterns. |
| **How It Splits** | `how_it_splits` | Examine the actual tokenization output. Report piece counts for compound words and multi-word expressions. |
| **Edge Case Survival** | `edge_case_survival` | Test behavior on mixed scripts, code-switching, numbers, URLs, and rare Unicode. Note any catastrophic splits. |

### Per-Language Lane Reporting

For multilingual inputs, report dial scores **per language lane**:

```
Lane: German
  - vocabulary_fit: X/4
  - merge_economy: X/4
  - ...

Lane: Turkish
  - vocabulary_fit: X/4
  - merge_economy: X/4
  - ...
```

Always identify which lane is **uncounted** or underrepresented if the sample claims coverage that isn't demonstrated.

---

## Output Shape

```yaml
input_sample: "<verbatim input>"
lanes_detected:
  - language: <detected language>
    token_count: <integer>
    dials:
      special_token_handling: <0-4>
      vocabulary_fit: <0-4>
      merge_economy: <0-4>
      how_it_splits: <0-4>
      edge_case_survival: <0-4>
weakest_dial: <dial key>
verdict: "<one-sentence assessment>"
uncounted_lanes: [<any claimed but missing languages>]
refusals_triggered: [<any refused content types detected>]
```

---

## Calibration Anchor

**Weakest dial for this deployment:** `vocabulary_fit`

This skill was calibrated against a 14-day support queue export with the following traffic mix: 38% German, 22% Turkish, 19% English, remainder Thai / Arabic / Mandarin.

---

## Runtime Loading

To load this skill:

1. Copy this file to your assistant's skills directory
2. Reference it in your assistant configuration
3. The advisor will activate when processing text streams from the configured CRM source

---

## Notes

- Dial scores use the same 0–4 scale across all files in this repository
- Cross-reference `blueprints/token-fit-checker.md` for the full system instructions
- See `tests/probes.jsonl` for machine-readable test cases
