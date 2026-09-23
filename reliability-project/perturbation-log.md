# Perturbation Log

## System 1 — Build a Validated, Routed Insurance Policy Extraction Pipeline

### Baseline Evidence

The final HITL routing solution was executed using the supplied test suite.

- Test result: `45 passed, 3 skipped`
- Mypy: `Success: no issues found in 11 source files`
- Ruff: `All checks passed!`
- Offline routing fallback: `9 passed`
- Calibration report was generated successfully.

### Perturbation

The extraction/validation behavior was exercised using the supplied retry and validation tests.

Command:

```text
.venv\Scripts\python.exe -m pytest tests\test_us01_retry.py -v