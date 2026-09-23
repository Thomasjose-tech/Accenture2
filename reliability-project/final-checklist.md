# Final Submission Checklist

## System 1 — Insurance Policy Extraction

### Evidence

* [x] Test suite executed
* [x] Test result captured in `tests.txt`
* [x] Mypy executed
* [x] Mypy result captured in `mypy.txt`
* [x] Ruff executed
* [x] Ruff result captured in `ruff.txt`
* [x] Offline routing fallback executed
* [x] Routing result captured in `routing-test.txt`
* [x] Calibration report generated
* [x] Calibration result captured in `calibration.txt`
* [x] Retry/validation perturbation executed
* [x] Perturbation result captured in `perturbation.txt`

### Results

```text
45 passed, 3 skipped
Success: no issues found in 11 source files
All checks passed!
9 passed in 0.05s
13 passed, 1 skipped
```

The three skipped tests were live API tests because no `ANTHROPIC_API_KEY` was configured.

The offline routing fallback was used as permitted by the project instructions.

---

## System 2 — Mortgage Document Extraction

### Evidence

* [x] Full test suite executed
* [x] Test result captured in `tests.txt`
* [x] Mypy executed
* [x] Mypy result captured in `mypy.txt`
* [x] Ruff executed
* [x] Ruff result captured in `ruff.txt`
* [x] Informal square-footage case tested
* [x] Missing bonus case tested
* [x] Mathematical mismatch case tested

### Results

```text
25 passed
Success: no issues found in 11 source files
All checks passed!
```

Required individual cases:

```text
1 passed — informal square footage normalization
1 passed — missing bonus returns None
1 passed — income sum mismatch is flagged
```

---

## System 3 — Supply Chain Risk Investigation

### Evidence

* [x] Full test suite executed
* [x] Test result captured in `tests.txt`
* [x] Mypy executed
* [x] Ruff executed
* [x] Normal offline briefing generated
* [x] Timeout briefing generated
* [x] Multi-source evidence classification observed
* [x] Source timeout behavior observed

### Results

```text
34 passed, 2 warnings
Success: no issues found in 8 source files
All checks passed!
```

The two warnings were related to the Hugging Face cache symlink behavior on Windows.

The normal briefing classified evidence into:

* Well-Established
* Contested
* Incomplete

The timeout scenario showed graceful degradation when the logistics source became unavailable.

---

# Documentation

* [x] `perturbation-log.md`
* [x] `reflection-brief.md`
* [x] `final-checklist.md`

---

# Environment

* [x] `environment.txt` created
* [x] Python version recorded
* [x] Windows operating-system information recorded

---

# Final Evidence Folder

The final evidence folder should contain:

```text
reliability-project/
│
├── 01-policy-pipeline/
│   ├── tests.txt
│   ├── mypy.txt
│   ├── ruff.txt
│   ├── routing-test.txt
│   ├── calibration.txt
│   └── perturbation.txt
│
├── 02-mortgage-extractor/
│   ├── tests.txt
│   ├── mypy.txt
│   ├── ruff.txt
│   ├── appraisal_informal_sqft.txt
│   ├── income_missing_bonus.txt
│   └── income_sum_mismatch.txt
│
├── 03-supply-chain/
│   ├── tests.txt
│   ├── mypy.txt
│   ├── ruff.txt
│   ├── briefing.txt
│   └── timeout-briefing.txt
│
├── environment.txt
├── perturbation-log.md
├── reflection-brief.md
└── final-checklist.md
```

---

# Final Status

All three reference systems were executed successfully using their supplied workflows.

The evidence includes:

1. Baseline test results.
2. Static-analysis results.
3. Routing/calibration evidence.
4. Perturbation evidence.
5. Mortgage extraction edge cases.
6. Supply-chain multi-source briefing.
7. Supply-chain timeout behavior.
8. Environment information.
9. Perturbation log.
10. Reflection brief.
11. Final checklist.

The Insurance Policy Extraction system used the permitted offline routing fallback because a live API key was not available.
