# Final Submission Checklist

## System 1 — Insurance Policy Extraction

### Baseline Evidence

* [x] Full supplied test suite executed
* [x] Test result captured in `tests.txt`
* [x] Mypy executed
* [x] Mypy result captured in `mypy.txt`
* [x] Ruff executed
* [x] Ruff result captured in `ruff.txt`
* [x] Offline routing fallback executed
* [x] Routing result captured in `routing-test.txt`
* [x] Calibration report generated
* [x] Calibration result captured in `calibration.txt`
* [x] Retry evidence captured

### Perturbation Evidence

* [x] Required policy number deliberately removed from a copy of `POL-2025-001.txt`
* [x] Perturbed input documented
* [x] Missing-required validation behavior tested
* [x] Evidence captured in `perturbation-missing-required.txt`
* [x] Direct live pipeline limitation documented because the required Anthropic API authentication was unavailable

### Reliability Reflection

* [x] Human-review routing case traced to the reviewer-disagreement signal
* [x] High-confidence versus correctness issue discussed
* [x] Calibration evidence discussed
* [x] `umbrella exclusions` calibration result referenced

### Baseline Results

```text
45 passed, 3 skipped
Success: no issues found in 11 source files
All checks passed!
9 passed in 0.05s
13 passed, 1 skipped
```

Artifacts:

```text
01-policy-pipeline/
├── tests.txt
├── mypy.txt
├── ruff.txt
├── routing-test.txt
├── calibration.txt
└── perturbation-missing-required.txt
```

---

## System 2 — Mortgage Document Extraction

### Baseline Evidence

* [x] Full supplied test suite executed
* [x] Test result captured in `tests.txt`
* [x] Mypy executed
* [x] Mypy result captured in `mypy.txt`
* [x] Ruff executed
* [x] Ruff result captured in `ruff.txt`
* [x] Informal square-footage case tested
* [x] Missing-bonus case tested
* [x] Mathematical mismatch case tested
* [x] Actual mortgage extraction CLI run captured
* [x] Actual discrepancy CLI run captured

### Extraction Evidence

* [x] `income_missing_bonus.txt` captured
* [x] `mortgage-extract.txt` captured
* [x] Missing bonus represented as `null`
* [x] Validation result captured

### Discrepancy Evidence

* [x] `income_sum_mismatch.txt` captured
* [x] `discrepancy.txt` captured
* [x] `total_monthly_income` discrepancy captured
* [x] Calculated value captured
* [x] Stated value captured
* [x] Delta captured

### Perturbation Evidence

* [x] Copy of income-sum mismatch fixture created
* [x] Stated monthly total deliberately changed from `10892.17` to `11892.17`
* [x] Baseline validation result captured
* [x] Perturbed validation result captured
* [x] Calculated value remained `9642.17`
* [x] Delta changed from `-1250.0` to `-2250.0`
* [x] Perturbation evidence captured in `perturbation.txt`
* [x] Replay limitation documented because the modified input had no recorded response

### Reliability Reflection

* [x] Null-not-fabricated behavior explained
* [x] Nullable extraction behavior tied to the actual output
* [x] Mathematical discrepancy explained
* [x] Perturbation effect explained

### Baseline Results

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

Artifacts:

```text
02-mortgage-extractor/
├── tests.txt
├── mypy.txt
├── ruff.txt
├── appraisal_informal_sqft.txt
├── income_missing_bonus.txt
├── income_sum_mismatch.txt
├── mortgage-extract.txt
├── discrepancy.txt
└── perturbation.txt
```

---

## System 3 — Supply Chain Risk Investigation

### Baseline Evidence

* [x] Full supplied test suite executed
* [x] Test result captured in `tests.txt`
* [x] Mypy executed
* [x] Mypy result captured in `mypy.txt`
* [x] Ruff executed
* [x] Ruff result captured in `ruff.txt`
* [x] Normal offline briefing generated
* [x] Normal briefing captured in `briefing.txt`
* [x] Multi-source evidence classification observed
* [x] Contested evidence observed
* [x] Incomplete evidence observed

### Perturbation Evidence

* [x] `--simulate-timeout` deliberately enabled
* [x] Logistics timeout behavior observed
* [x] Timeout briefing captured in `timeout-briefing.txt`
* [x] Source-unavailable message captured
* [x] Difference between normal and timeout briefing documented

### Reliability Reflection

* [x] `on_time_delivery_rate` conflict documented
* [x] `95.0%` from `supplier_audit` dated `2026-04-10` documented
* [x] `78.0%` from `logistics` dated `2026-04-05` documented
* [x] Reason for preserving both values explained
* [x] Timeout behavior explained
* [x] Provenance and incomplete evidence discussed

### Baseline Results

```text
34 passed, 2 warnings
Success: no issues found in 8 source files
All checks passed!
```

Artifacts:

```text
03-supply-chain/
├── tests.txt
├── mypy.txt
├── ruff.txt
├── briefing.txt
└── timeout-briefing.txt
```

---

# Perturbation Requirements

* [x] One deliberate perturbation documented for each system
* [x] System 1: required policy number removed
* [x] System 2: stated monthly income changed from `10892.17` to `11892.17`
* [x] System 3: logistics timeout simulated
* [x] Each perturbation includes the change
* [x] Each perturbation includes the execution command or execution method
* [x] Each perturbation includes the predicted behavior
* [x] Each perturbation includes observed output
* [x] Each perturbation compares the result with baseline behavior

Primary artifact:

```text
perturbation-log.md
```

---

# Reflection Requirements

* [x] Insurance reliability behavior explained
* [x] Human-review routing signal traced
* [x] Calibration result discussed
* [x] Confidence versus correctness discussed
* [x] Mortgage extraction behavior explained
* [x] Null-not-fabricated behavior explained
* [x] Mathematical discrepancy explained
* [x] Supply-chain source conflict explained
* [x] `95.0%` versus `78.0%` conflict documented
* [x] Timeout behavior explained
* [x] Cross-system reliability principle included
* [x] Section 4 synthesis included
* [x] Evidence filenames referenced throughout

Primary artifact:

```text
reflection-brief.md
```

---

# Environment

* [x] `environment.txt` created
* [x] Python version recorded
* [x] Windows operating-system information recorded
* [x] Environment/tool information captured

Artifact:

```text
environment.txt
```

---

# Documentation

* [x] `perturbation-log.md`
* [x] `reflection-brief.md`
* [x] `final-checklist.md`
* [x] `environment.txt`

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
│   └── perturbation-missing-required.txt
│
├── 02-mortgage-extractor/
│   ├── tests.txt
│   ├── mypy.txt
│   ├── ruff.txt
│   ├── appraisal_informal_sqft.txt
│   ├── income_missing_bonus.txt
│   ├── income_sum_mismatch.txt
│   ├── mortgage-extract.txt
│   ├── discrepancy.txt
│   └── perturbation.txt
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

The three supplied reference systems have baseline evidence, static-analysis evidence, and system-specific reliability evidence captured.

The final documentation records:

1. Baseline test results.
2. Static-analysis results.
3. Insurance routing evidence.
4. Insurance calibration evidence.
5. Insurance missing-required-field perturbation.
6. Mortgage extraction edge-case evidence.
7. Mortgage mathematical discrepancy evidence.
8. Mortgage stated-total perturbation.
9. Supply-chain multi-source synthesis.
10. Supply-chain timeout perturbation.
11. Environment information.
12. Perturbation log.
13. Reflection brief.
14. Final checklist.

The evidence also records the execution limitations encountered during perturbation work. The Insurance live pipeline could not authenticate without the required Anthropic API credentials, and the modified Mortgage input did not have a recorded replay response. Deterministic supplied-system validation evidence was used where applicable rather than presenting those executions as successful live runs.
