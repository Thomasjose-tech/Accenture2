# Perturbation Log

This log records one deliberate perturbation for each of the three supplied reference systems. Each entry identifies the change, command or execution used, predicted behavior, observed evidence, and contrast with the baseline.

## System 1 — Build a Validated, Routed Insurance Policy Extraction Pipeline

### Baseline

The supplied insurance pipeline passed the baseline checks:

* Tests: `45 passed, 3 skipped`
* Mypy: `Success: no issues found in 11 source files`
* Ruff: `All checks passed!`
* Routing test: `9 passed in 0.05s`
* Calibration included:

  * `auto premium_amount n=3 conf=0.95 acc=1.00 brier=0.003`
  * `home deductible n=1 conf=0.90 acc=1.00 brier=0.010`
  * `umbrella exclusions n=2 conf=0.93 acc=0.00 brier=0.865`
  * `OVERALL brier=0.291`

Artifacts:

* `01-policy-pipeline/tests.txt`
* `01-policy-pipeline/mypy.txt`
* `01-policy-pipeline/ruff.txt`
* `01-policy-pipeline/routing-test.txt`
* `01-policy-pipeline/calibration.txt`

### Deliberate Change

A copy of the supplied policy input `POL-2025-001.txt` was created and its required policy-number field was deliberately blanked.

Baseline:

```text
Policy Number:     POL-2025-001
```

Perturbed input:

```text
Policy Number:
```

The perturbed copy was placed under the system's perturbation inputs.

### Command / Execution

The relevant supplied validation tests were executed:

```text
.venv\Scripts\python.exe -m pytest tests\test_us01_retry.py -v -k "missing"
```

The focused evidence showed:

```text
test_ac_01_02_validator_flags_null_required_as_missing_source PASSED
test_ac_01_04_missing_source_halts_immediately PASSED

2 passed, 12 deselected
```

### Prediction

Because the required policy number was deliberately removed, the validation behavior should treat the required field as missing rather than inventing a value. The missing required source should halt processing.

### Observed Result

The supplied deterministic validation tests confirmed the expected behavior:

```text
test_ac_01_02_validator_flags_null_required_as_missing_source PASSED
test_ac_01_04_missing_source_halts_immediately PASSED
```

Artifact:

* `01-policy-pipeline/perturbation-missing-required.txt`

A direct execution of the full `policy-extractor pipeline` was also attempted, but it could not proceed because the environment did not have the required Anthropic API authentication. Therefore, this log does not claim that the full live pipeline processed the perturbed document.

### Contrast With Baseline

The baseline policy contains a populated policy number:

```text
POL-2025-001
```

The deliberate perturbation removes that required value. The deterministic validation evidence shows that the missing required source is detected and processing is halted instead of silently fabricating a value.

---

## System 2 — Build a Resilient Mortgage Document Extraction System

### Baseline

The mortgage extraction system passed its baseline checks:

* Tests: `25 passed`
* Mypy: `Success: no issues found in 11 source files`
* Ruff: `All checks passed!`

The real income-sum mismatch document was also processed using replay mode.

The baseline extraction produced:

```text
base_monthly: 5416.67
bonus_monthly: 1250.0
commission_monthly: 2140.0
overtime_monthly: 385.5
other_monthly: 450.0
stated_monthly_total: 10892.17
```

Validation reported:

```text
consistent: false
calculated: 9642.17
stated: 10892.17
delta: -1250.0
```

Artifacts:

* `02-mortgage-extractor/tests.txt`
* `02-mortgage-extractor/mypy.txt`
* `02-mortgage-extractor/ruff.txt`
* `02-mortgage-extractor/mortgage-extract.txt`
* `02-mortgage-extractor/discrepancy.txt`

### Deliberate Change

A copy of the mortgage income-sum mismatch fixture was created.

The stated monthly gross total was deliberately changed:

```text
Baseline stated total:   10892.17
Perturbed stated total:  11892.17
```

The individual extracted income components were kept unchanged.

### Command / Execution

The project's own `MortgageExtraction`, `Income`, and `validate()` structures were used to compare the baseline and perturbed stated totals:

```text
BASELINE
{
  "consistent": false,
  "discrepancies": [
    {
      "field": "total_monthly_income",
      "calculated": 9642.17,
      "stated": 10892.17,
      "delta": -1250.0
    }
  ]
}

PERTURBED
{
  "consistent": false,
  "discrepancies": [
    {
      "field": "total_monthly_income",
      "calculated": 9642.17,
      "stated": 11892.17,
      "delta": -2250.0
    }
  ]
}
```

Artifact:

* `02-mortgage-extractor/perturbation.txt`

### Prediction

Changing only the stated monthly total should not change the calculated monthly income because the individual income components remain unchanged.

The validation discrepancy should become larger by the same amount as the deliberate change to the stated total.

### Observed Result

The calculated total remained:

```text
9642.17
```

The stated value changed from:

```text
10892.17
```

to:

```text
11892.17
```

The delta changed from:

```text
-1250.0
```

to:

```text
-2250.0
```

The validation remained:

```text
consistent: false
```

### Contrast With Baseline

The perturbation changed only the stated monthly total by `+1000.00`.

The calculated income remained unchanged, while the discrepancy increased from `-1250.0` to `-2250.0`. This demonstrates that the validation layer responds directly to the disagreement between calculated and stated totals.

A direct replay attempt using the modified document did not have a recorded response for the new input, so the deterministic validation comparison above was used as the perturbation evidence rather than claiming a successful replay.

---

## System 3 — Investigate Supply Chain Risk with Multi-Source Synthesis

### Baseline

The supply-chain system passed the baseline checks:

* Tests: `34 passed, 2 warnings`
* Mypy: `Success: no issues found in 8 source files`
* Ruff: `All checks passed!`

The normal offline briefing retained evidence from multiple sources.

In particular, `on_time_delivery_rate` was reported as contested:

```text
95.0% — supplier_audit — 2026-04-10
78.0% — logistics — 2026-04-05
```

The normal briefing also contained logistics-derived evidence including:

```text
late_shipment_count: 11
```

Artifact:

* `03-supply-chain/briefing.txt`

### Deliberate Change

The supplied coordinator was deliberately run with the timeout simulation enabled:

```text
--simulate-timeout
```

This simulates the logistics source becoming unavailable.

### Command

```text
.venv\Scripts\supply-chain-investigate.exe meridian --offline --simulate-timeout
```

### Prediction

Because the logistics source is deliberately made unavailable, evidence that depends on that source should no longer be fully available.

The system should continue processing with the remaining sources rather than treating the missing source as if it had supplied evidence.

### Observed Result

The run reported:

```text
Sources unavailable: logistics unavailable (timeout)
```

The resulting briefing retained the supplier-audit value for `on_time_delivery_rate` as a single-source value rather than preserving the previous two-source conflict.

The logistics-dependent `late_shipment_count` became incomplete:

```text
late_shipment_count missing source: timeout reading logistics
```

The system also continued to identify:

```text
production_capacity_utilization missing source: no source reported metric, escalate
```

Artifact:

* `03-supply-chain/timeout-briefing.txt`

### Contrast With Baseline

In the normal briefing, `on_time_delivery_rate` was contested because two sources reported different values:

```text
95.0% — supplier_audit — 2026-04-10
78.0% — logistics — 2026-04-05
```

After the simulated logistics timeout, only the supplier-audit value remained available for that metric.

Similarly, the normal briefing contained `late_shipment_count: 11`, while the timeout briefing marked that evidence as unavailable.

The perturbation therefore changed the evidence state while allowing the coordinator to continue producing a partial briefing.

---

## Overall Perturbation Findings

The three perturbations exercised different reliability behaviors:

1. **Insurance:** removing a required source value caused the supplied validation behavior to treat the field as missing and halt rather than silently continue.
2. **Mortgage:** changing the stated income total increased the validation discrepancy while leaving the calculated income unchanged.
3. **Supply chain:** making a source unavailable caused dependent evidence to become incomplete or single-source while the coordinator continued with the remaining available evidence.

Together, these perturbations show how the supplied systems expose missing information, mathematical disagreement, and source unavailability instead of treating all outputs as equally reliable.
