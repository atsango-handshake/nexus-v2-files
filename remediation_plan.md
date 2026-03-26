## 3. Remediation Plan

### Priority 1 — Grading (Immediate Action Required)
**Problem:** 54/103 tasks have broken rubrics. Binary scoring and miscalibrated tolerances are the root cause.

| Action | Detail |
|--------|--------|
| Require methodology scoring | Rubrics must award partial credit for correct pipeline steps, independent of final numeric match |
| Tolerance calibration guide | Set tolerance relative to metric type: exact match for integer counts, ≤ ±0.5 for single-decimal metrics, ≤ ±2% for percentages; never wider than needed to absorb rounding only |
| Gold answer reproducibility check | Fellows must submit a step-by-step reproducibility trace showing how the gold answer was derived; if it can't be reproduced from workspace files, the task is blocked |
| No-tool-gating rubrics | Rubric should not require a specific unlisted tool; if the only valid path is one specific tool, that tool must be named in the prompt |

---

### Priority 2 — Clarity (High Impact, Fixable at Source)
**Problem:** 33/103 tasks fail because the prompt is underspecified, leaving multiple valid interpretations.

| Action | Detail |
|--------|--------|
| Mandatory task spec checklist | Before submission, fellows must confirm: tool named, version or parameters stated, input files listed, output format defined, any filtering/cutoff criteria specified |
| "One valid path" rule | Every task must have exactly one clearly described workflow path that leads to the gold answer; if multiple valid paths exist, either lock in one or design a range-based rubric |
| Terminology glossary | Domain terms with dual meanings (e.g., "consensus peaks," "alignment-based TPM," "consistency") must be defined inline in the prompt |
| Contradiction audit | Prompts referencing experimental assays (ATAC-seq vs. ChIP-seq) must be reviewed for internal consistency before submission |

---

### Priority 3 — Realism (Low Priority, Spot Fixes)
**Problem:** Only 4 failures, but the pattern is instructive.

| Action | Detail |
|--------|--------|
| "Would a professional actually ask this?" test | Prompts asking for aggregated or decontextualized metrics (e.g., "sum all peaks across all samples") should be challenged at creation |
| Preserve realistic preprocessing complexity | Don't pre-clean data unless cleaning is part of what the task is testing; pre-processed data reduces both realism and difficulty training signal |

---

### Priority 4 — Difficulty Calibration (Ongoing)
**Problem:** Difficulty is generally well-calibrated, but pre-processing shortcuts artificially lower difficulty and reduce training value.

| Action | Detail |
|--------|--------|
| Preprocessing flag | If data is pre-aligned or pre-trimmed, note this explicitly and consider whether difficulty should be downgraded accordingly |
| Entry-level minimum complexity bar | Tasks that execute a single tool and read one output line should be reviewed for whether they provide sufficient training signal |

---