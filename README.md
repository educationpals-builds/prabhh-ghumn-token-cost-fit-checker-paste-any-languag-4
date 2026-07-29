# Token Cost + Fit Checker

A conversational checker that evaluates multilingual text samples for tokenizer fit, vocabulary coverage, and cost implications. Built for teams making embedding and inference decisions under vocabulary constraints.

---

## How This Checker Was Built

This checker was calibrated against a real support queue containing German, Turkish, English, Thai, Arabic, and Mandarin tickets. The builder walked through a five-dial evaluation framework, pinned a sample, scored it, rendered a verdict, and defined the conditions under which that verdict would flip.

The result is a checker you can paste any language mix into and get back:
- Per-language lane token counts
- Five dial scores (0–4 each)
- A fit verdict with the deciding dial named
- The cost of being wrong

---

## Worked Example: The Builder's Own Sample + Verdict

**Pinned Sample (verbatim):**

> "Ihr Krankenversicherungsbeitrag wurde angepasst — bitte prüfen Sie die Beitragsbemessungsgrenze." / "Sigortalılığınızın başlangıç tarihini öğrenebilir miyim?" (two verbatim tickets from the queue)

**Traffic Source:**

> 14-day support queue export: 38% German, 22% Turkish, 19% English, remainder Thai / Arabic / Mandarin

**What This Decides:**

> Picks the vocabulary for the on-device assistant — the embedding table is capped and inference is billed per token

**Decision Deadline:**

> Thursday's architecture review

**Five Dial Scores:**

| Dial | Score (0–4) |
|------|-------------|
| special_token_handling | 2 |
| vocabulary_fit | 2 |
| merge_economy | 2 |
| how_it_splits | 2 |
| edge_case_survival | 2 |

**Weakest Dial:** vocabulary_fit

**Verdict:**

> The vocabulary accuracy metrics should be more than 90% accurate for it to be acceptable.

---

## One-Paste Rebuild Block

To rebuild this checker from scratch or fork it for your own calibration:

1. Clone this repository
2. Open `charter.md` — the full calibration run lives there
3. Paste the system instructions from `blueprints/token-fit-checker.md` into your assistant
4. Load `tests/probes.jsonl` to run the evaluation board
5. Verify with the protocol in `VERIFY.md`

```
git clone <this-repo>
cd token-cost-fit-checker
cat blueprints/token-fit-checker.md  # system instructions
cat tests/probes.jsonl               # machine-readable probes
```

---

## Repository Structure

| Path | Purpose |
|------|---------|
| `charter.md` | The builder's full calibration run |
| `blueprints/token-fit-checker.md` | One-paste system instructions |
| `prompts/token-fit-pack.md` | 5 standalone prompts, one per dial |
| `METHOD.md` | The framework and its acronym |
| `VERIFY.md` | Stranger verification protocol |
| `skills/token-fit-advisor.skill.md` | Portable skill file for any assistant runtime |
| `data/lane-fit-sheet.md` | Calibration record with seeded samples |
| `tests/probe-board.md` | All 8 probes with results grid |
| `tests/pass-gate.md` | Gate metric, threshold, re-run cadence |
| `tests/probes.jsonl` | Machine-readable probe export |
| `tests/run-local.md` | Run-anywhere guide (manual, script, CI) |
| `STORY.md` | The builder's first-person story |

---

## The Five Dials

This checker evaluates text against five dimensions:

1. **special_token_handling** — How the tokenizer handles special characters, punctuation, currency symbols
2. **vocabulary_fit** — Whether the tokenizer's vocabulary covers the language mix without excessive unknown tokens
3. **merge_economy** — How efficiently common sequences merge into single tokens
4. **how_it_splits** — The actual piece count for compound words and agglutinative forms
5. **edge_case_survival** — Behavior on emoji, code-switching, mixed scripts, rare Unicode

---

## Quick Start

Paste any multilingual text into the checker. You'll get back:

- Token count per language lane
- A score (0–4) for each of the five dials
- The weakest dial identified
- A fit verdict with cost implications

See `VERIFY.md` for the verification protocol using the seeded German+Turkish sample.

<!-- educationpals-build-verified -->
