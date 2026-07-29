# METHOD.md

## The TFLEC Framework

This checker is built on the **TFLEC** framework — five dials that together determine whether a tokenizer fits a given text stream.

| Letter | Dial | What it measures |
|--------|------|------------------|
| **T** | special_**T**oken_handling | How the tokenizer treats special tokens (BOS, EOS, PAD, UNK) and whether they leak into or corrupt the output |
| **F** | vocabulary_**F**it | Whether the tokenizer's vocabulary covers the language mix without excessive UNK fallback or byte-level fragmentation |
| **L** | merge_economy (token **L**ength) | How efficiently the tokenizer merges subwords — fewer tokens per semantic unit means lower inference cost |
| **E** | how_it_splits (**E**xplosion) | Whether compound words, code, or mixed scripts explode into unexpectedly many pieces |
| **C** | edge_**C**ase_survival | Whether the tokenizer handles emoji, zalgo, RTL, code-switching, and other edge cases without corruption |

---

## How the dials work

Each dial is scored 0–4:

- **0** — Fails outright; unusable for this traffic
- **1** — Severe issues; would require major workarounds
- **2** — Marginal; works but with notable cost or quality penalty
- **3** — Acceptable; minor issues that can be monitored
- **4** — Strong fit; no concerns for this traffic type

The **weakest dial** determines the overall fit. A tokenizer scoring 4/4/4/1/4 is a 1 for that traffic — the chain breaks at its weakest link.

---

## Per-lane reporting

For multilingual traffic, each dial is evaluated **per language lane**. A tokenizer might score 4 on vocabulary_fit for English but 1 for Turkish. The per-lane breakdown surfaces these gaps before they become production surprises.

---

## When to use TFLEC

Run a TFLEC check when:

1. Selecting a tokenizer for a new language mix
2. Evaluating whether an existing tokenizer handles a traffic shift
3. Comparing tokenizer options for cost (merge_economy) vs. coverage (vocabulary_fit) tradeoffs
4. Auditing edge-case handling before deploying to user-facing systems

The framework assumes you have sample text from your actual traffic — not synthetic benchmarks. Paste real bytes, get a real read.
