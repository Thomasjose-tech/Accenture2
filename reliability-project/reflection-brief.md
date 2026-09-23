
---

### 2. `reflection-brief.md`

```markdown
# Reflection Brief

## 1. Key Reliability Findings

The three systems demonstrated different approaches to making automated workflows reliable and observable.

### Insurance Policy Extraction

The insurance pipeline showed the importance of validation and routing.

The supplied tests demonstrated that extraction results can be checked for:

- Missing required information
- Invalid formats
- Consistency failures
- Validation failures requiring retry
- Successful completion after retry

The routing tests also showed that extraction confidence is not sufficient by itself to determine whether a result should be automatically approved.

The calibration output provided a particularly important example:

```text
umbrella exclusions n=2 conf=0.93 acc=0.00 brier=0.865