# Run-Local Guide

Three ways to run the token-fit checker probes locally, from zero tooling to full CI.

---

## Rung 1: Manual — Paste Protocol

For each probe in `tests/probes.jsonl`, follow this protocol:

1. Open your chat model of choice
2. Load the system instructions from `blueprints/token-fit-checker.md`
3. Paste the probe input **exactly as written** — do not reformat or "clean up" the text
4. Compare the checker's output against the expected behavior

### Byte-Preservation Warning

**Critical:** Many probes contain non-ASCII characters (German umlauts, Turkish dotted-i, compound nouns). Copy-paste can silently corrupt these bytes:

- Use raw copy (not "smart quotes" conversion)
- Disable auto-correct in your paste target
- If your terminal mangles UTF-8, use a file-based approach instead

### Manual Checklist

| Probe | Input | Target Dial | Expected Behavior |
|-------|-------|-------------|-------------------|
| (see probes.jsonl for full list) | paste verbatim | check dial name | compare output |

Record each result. Mark PASS if the checker's dial rating matches expected direction, FAIL otherwise.

---

## Rung 2: Script — Embedded Runner

A minimal runner that reads `tests/probes.jsonl` and prints a graded grid.

```python
#!/usr/bin/env python3
"""Token-fit probe runner. Reads probes.jsonl, calls the model, prints results."""
import json
import os
import sys

def load_probes(path="tests/probes.jsonl"):
    probes = []
    with open(path, "r", encoding="utf-8") as f:
        for line in f:
            if line.strip():
                probes.append(json.loads(line))
    return probes

def run_probe(probe, client):
    # Replace with your model call; this is a placeholder structure
    response = client.chat(messages=[
        {"role": "system", "content": open("blueprints/token-fit-checker.md").read()},
        {"role": "user", "content": probe["input"]}
    ])
    return response

def grade(probe, result):
    # Compare result against probe["expected"] and probe["invariant"]
    # Return "PASS" or "FAIL"
    return "PASS" if probe["expected"].lower() in result.lower() else "FAIL"

def main():
    api_key = os.environ.get("MODEL_API_KEY")
    if not api_key:
        print("Set MODEL_API_KEY in environment", file=sys.stderr)
        sys.exit(1)
    
    probes = load_probes()
    results = []
    for p in probes:
        out = run_probe(p, client=None)  # wire your client here
        g = grade(p, out)
        results.append({"id": p["id"], "name": p["name"], "grade": g})
    
    print("=" * 50)
    print("PROBE BOARD RESULTS")
    print("=" * 50)
    for r in results:
        print(f"{r['id']:12} {r['name']:30} {r['grade']}")
    
    passed = sum(1 for r in results if r["grade"] == "PASS")
    total = len(results)
    print("-" * 50)
    print(f"Gate verdict: {passed}/{total} probes passed")

if __name__ == "__main__":
    main()
```

### Usage

```bash
export MODEL_API_KEY="your-key-here"
python3 run_probes.py
```

The script prints a graded grid showing each probe's pass/fail status and the overall gate verdict.

---

## Rung 3: Eval Tool / CI Integration

Load `tests/probes.jsonl` into any eval runner so the board re-runs automatically on prompt or stance changes.

### Loading into an Eval Framework

The `probes.jsonl` format is designed for direct import:

```json
{"id": "probe_01", "name": "...", "input": "...", "targets": [...], "expected": "...", "invariant": "..."}
```

Each line is a self-contained test case. Most eval tools accept JSONL directly or with minimal transformation.

### CI Pipeline Example

```yaml
# .github/workflows/probe-check.yml
name: Probe Board Check
on:
  push:
    paths:
      - 'blueprints/**'
      - 'skills/**'
      - 'prompts/**'
jobs:
  run-probes:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Run probe board
        env:
          MODEL_API_KEY: ${{ secrets.MODEL_API_KEY }}
        run: python3 run_probes.py
```

This re-runs the full probe board whenever the checker's instructions change.

---

## Diffing Against the EP-Certified Board

To compare your local run against the EducationPals-certified board on the listing:

1. Run the local board and save output:
   ```bash
   python3 run_probes.py > local_results.txt
   ```

2. Download the certified board results from the EP listing (if available as artifact)

3. Diff the two:
   ```bash
   diff local_results.txt certified_results.txt
   ```

Any divergence indicates drift — either in your local model, the prompt, or the stance. Investigate before shipping changes.

### What Counts as Drift

- A probe that passed on certification now fails locally → regression
- A probe that failed on certification now passes locally → improvement (re-certify to claim it)
- Per-lane counts differ by more than 10% → tokenizer or model version changed

Re-run the board after any change to `blueprints/token-fit-checker.md`, `skills/token-fit-advisor.skill.md`, or the underlying model version.
