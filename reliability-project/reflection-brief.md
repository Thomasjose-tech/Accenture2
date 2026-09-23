# Reflection Brief

## 1. Insurance Policy Extraction

### 1a. What reliability behavior did the insurance system demonstrate?

The insurance pipeline demonstrated reliability through validation, retry behavior, routing, and explicit handling of missing required information.

The baseline system passed `45` tests with `3` skipped. Mypy reported `Success: no issues found in 11 source files`, and Ruff reported `All checks passed!`. The routing test also passed with `9 passed in 0.05s`.

The supplied retry and validation evidence shows that the system does not simply accept every extracted value. Missing required information can be detected and can prevent the workflow from continuing.

Artifacts:

* `01-policy-pipeline/tests.txt`
* `01-policy-pipeline/perturbation-missing-required.txt`
* `01-policy-pipeline/routing-test.txt`

### 1b. How did the routing decision relate to the reliability signal?

The routing evidence showed that a human-review case can be created from a combination of extraction confidence and reviewer disagreement.

The important signal was **reviewer disagreement**. The case had high extraction confidence, but the reviewer outcome disagreed with the automated result. Therefore, confidence alone was not sufficient to justify automatic approval.

In other words, the high confidence score did not remove the need for human review. The disagreement signal caused the case to be routed to `human_review`.

This is important because an automated system can be confident about an incorrect result. The routing mechanism provides a path for uncertain or conflicting cases to receive human attention.

Artifact:

* `01-policy-pipeline/routing-test.txt`

### 1c. What did calibration reveal about confidence and correctness?

The calibration evidence showed a particularly important failure mode:

```text
umbrella exclusions n=2 conf=0.93 acc=0.00 brier=0.865
```

The model had an average confidence of `0.93`, while the observed accuracy for that slice was `0.00`.

The overall calibration result was:

```text
OVERALL brier=0.291
```

This demonstrates that a high confidence value cannot automatically be treated as proof that the extracted value is correct.

The calibration result supports using additional validation and routing signals rather than relying on confidence alone.

Artifact:

* `01-policy-pipeline/calibration.txt`

---

## 2. Mortgage Document Extraction

### 2a. What did the extraction demonstrate about missing information?

The income verification extraction demonstrated that missing information can remain explicitly missing instead of being fabricated.

The recorded extraction identified the document as:

```text
type=income_verification
```

The structured income result included:

```text
"bonus_monthly": null,
"bonus_ytd": null,
"stated_monthly_total": null
```

The validation result for this document was:

```text
"consistent": true,
"discrepancies": []
```

The important behavior is that the system returned `null` for fields that were not available rather than inventing a numerical value.

Artifact:

* `02-mortgage-extractor/mortgage-extract.txt`
* `02-mortgage-extractor/income_missing_bonus.txt`

### 2b. How did the mathematical validation expose a discrepancy?

The income-sum mismatch document produced a structured mathematical discrepancy.

The extracted income components produced a calculated monthly total of:

```text
9642.17
```

The stated monthly total was:

```text
10892.17
```

The validation report therefore returned:

```text
{
  "field": "total_monthly_income",
  "calculated": 9642.17,
  "stated": 10892.17,
  "delta": -1250.0
}
```

The overall validation result was:

```text
"consistent": false
```

This makes the disagreement observable instead of hiding it inside a single final result.

Artifact:

* `02-mortgage-extractor/discrepancy.txt`
* `02-mortgage-extractor/income_sum_mismatch.txt`

### 2c. Why is returning `null` preferable to fabricating a missing value?

The extraction behavior demonstrates that a missing value should remain explicitly missing when the source document does not provide enough information.

For example, the missing-bonus extraction returned:

```text
"bonus_monthly": null
```

rather than estimating or inventing a bonus amount.

This is supported by the nullable structure of the extraction model and by the actual recorded output. Keeping the value as `null` preserves the distinction between:

* information that was actually extracted from the document, and
* information that was not available.

This is preferable for downstream validation because a fabricated value could appear valid and influence later calculations without having source support.

Artifacts:

* `02-mortgage-extractor/mortgage-extract.txt`
* `02-mortgage-extractor/income_missing_bonus.txt`

---

## 3. Multi-Source Supply Chain Synthesis

### 3a. How did the system handle conflicting sources?

The normal supply-chain briefing preserved a conflict in `on_time_delivery_rate` rather than forcing the two sources into one unsupported value.

The two reported values were:

```text
95.0% — supplier_audit — 2026-04-10
78.0% — logistics — 2026-04-05
```

The briefing classified this information as contested.

Both values were retained because they came from different sources and different dates. Combining them into a single number would remove the provenance of the individual observations and could imply a reconciliation that the supplied evidence did not establish.

Preserving the disagreement makes the uncertainty visible to the person using the briefing.

Artifact:

* `03-supply-chain/briefing.txt`

### 3b. What happened when the logistics source timed out?

The supply-chain coordinator was deliberately run with:

```text
--simulate-timeout
```

The execution reported:

```text
Sources unavailable: logistics unavailable (timeout)
```

The resulting briefing continued with the information that remained available, while logistics-dependent information became incomplete.

For example, `late_shipment_count` was no longer available because the logistics source had timed out.

The `on_time_delivery_rate` evidence also changed from a contested two-source result to the remaining supplier-audit value as a single-source observation.

This demonstrates that source failure affected the evidence state without causing the entire synthesis to silently treat unavailable information as available.

Artifacts:

* `03-supply-chain/briefing.txt`
* `03-supply-chain/timeout-briefing.txt`

### 3c. What reliability principle did the multi-source system demonstrate?

The supply-chain system demonstrated the value of preserving provenance, disagreement, and incomplete evidence.

The normal run retained conflicting values instead of silently reconciling them. The timeout run retained the remaining available evidence while explicitly reporting the unavailable logistics source.

This allows a reviewer to distinguish between:

* information corroborated by multiple sources,
* information reported by only one source,
* conflicting observations, and
* information that could not be obtained.

Artifacts:

* `03-supply-chain/briefing.txt`
* `03-supply-chain/timeout-briefing.txt`

---

## Environment and Evidence

The work was performed on the supplied Windows project environments.

The captured baseline evidence includes:

### Insurance

```text
45 passed, 3 skipped
Success: no issues found in 11 source files
All checks passed!
9 passed in 0.05s
```

Artifacts:

* `01-policy-pipeline/tests.txt`
* `01-policy-pipeline/mypy.txt`
* `01-policy-pipeline/ruff.txt`
* `01-policy-pipeline/routing-test.txt`
* `01-policy-pipeline/calibration.txt`
* `01-policy-pipeline/perturbation-missing-required.txt`

### Mortgage

```text
25 passed
Success: no issues found in 11 source files
All checks passed!
```

Artifacts:

* `02-mortgage-extractor/tests.txt`
* `02-mortgage-extractor/mypy.txt`
* `02-mortgage-extractor/ruff.txt`
* `02-mortgage-extractor/mortgage-extract.txt`
* `02-mortgage-extractor/discrepancy.txt`
* `02-mortgage-extractor/perturbation.txt`

### Supply Chain

```text
34 passed, 2 warnings
Success: no issues found in 8 source files
All checks passed!
```

Artifacts:

* `03-supply-chain/tests.txt`
* `03-supply-chain/mypy.txt`
* `03-supply-chain/ruff.txt`
* `03-supply-chain/briefing.txt`
* `03-supply-chain/timeout-briefing.txt`

The supply-chain mypy check was executed with the Python-version setting required by the installed environment.

---

# 4. Overall Reliability Synthesis

A common principle across the three systems is:

> **Make uncertainty and failure observable, then route the result to an appropriate next action.**

The insurance system demonstrated this through validation, routing, and human review. The mortgage system demonstrated it by preserving missing values as `null` and exposing mathematical discrepancies. The supply-chain system demonstrated it by preserving source disagreement and identifying unavailable sources during a timeout.

The evidence also demonstrates that **confidence is not the same as correctness**. The insurance calibration slice for `umbrella exclusions` had confidence `0.93` but accuracy `0.00`. Therefore, a high confidence score by itself is not sufficient evidence that an output is correct.

The same principle applies to the other systems. A structured extraction should not be trusted merely because it has a complete-looking shape; the mortgage validation layer checks whether calculated and stated values agree. Similarly, a synthesized supply-chain value should not be treated as established simply because it appears in a briefing; its source count, provenance, disagreement, or missing-source state matters.

The perturbations reinforced this principle:

* Removing a required insurance value exposed a missing-source condition.
* Changing the mortgage stated total changed the reported discrepancy while leaving the calculated value unchanged.
* Simulating a logistics timeout changed the available evidence while allowing the remaining sources to continue contributing.

Across the three systems, reliability therefore comes from making the system's assumptions, uncertainty, disagreement, and failure modes visible rather than hiding them behind a single automated answer.
