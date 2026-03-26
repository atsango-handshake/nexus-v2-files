# Mar 25 — Reviewer Feedback Summary

**Total tasks reviewed:** 103
**Date compiled:** 2026-03-25

---

## 1. Dimension Score Counts

### Task Realism
| Verdict | Count | % |
|---------|-------|---|
| Pass | 99 | 96.1% |
| Fail | 4 | 3.9% |

### Task Clarity
| Verdict | Count | % |
|---------|-------|---|
| Pass | 70 | 68.0% |
| Fail | 33 | 32.0% |

### Task Difficulty
| Level | Count | % |
|-------|-------|---|
| Entry-Level | 26 | 25.2% |
| Mid-Level | 70 | 68.0% |
| Senior / Specialist | 7 | 6.8% |

### Grading Methodology
| Verdict | Count | % |
|---------|-------|---|
| Pass | 49 | 47.6% |
| Fail | 54 | 52.4% |

### Overall Verdict
| Verdict | Count | % |
|---------|-------|---|
| Accept | 32 | 31.1% |
| Revise | 58 | 56.3% |
| Reject | 13 | 12.6% |

---

## 2. Main Themes by Dimension

### 2.1 Task Realism *(96% Pass — lowest-risk dimension)*

Realism is the strongest dimension across the batch. The 4 failures are instructive edge cases rather than systemic problems.

**Why tasks pass:**
- Tasks involve standard QC workflows professionals genuinely run: duplicate rates in CUT&RUN, adapter contamination in FASTQ files, alignment statistics via samtools, Q10 thresholds on Nanopore reads.
- Tools (NanoPlot, MACS2, SEACR, STAR, DADA2, CheckM, MetaBAT2) and file formats (BAM, FASTQ, VCF, bigWig) are authentic.

**Why tasks fail (4 cases):**
1. **Contrived question** — Asking "which sample has the highest peak value?" or "sum all intergenic peaks across all samples" — metrics a professional would never compute in isolation.
   > *"Finding which sample contains the highest peak value is not a question a professional would actually care about... this feels like a contrived task made to run a specific set of tools to get to a premeditated answer."*
2. **Pre-cleaned data** — Mitochondrial DNA and low-quality reads already removed, eliminating a key real-world preprocessing step.
3. **Isolated metric with no analytical utility** — Using coding density as the sole QC metric, which professionals don't rely on standalone.

---

### 2.2 Task Clarity *(32% Fail — second-highest failure rate)*

Clarity failures are systemic and tightly clustered. Nearly every failure traces back to one of the following patterns:

**Theme 1 — Unspecified tool or workflow (~15+ tasks)**
The prompt asks for a metric (TPM, N50, FRiP, coverage depth) without specifying which tool to use. Because multiple valid tools exist and produce different outputs, the correct approach is ambiguous.
> *"The phrase 'alignment-based TPM' is doing a lot of work here and it's not defined anywhere in the prompt. A bioinformatician would reasonably wonder whether that means StringTie, RSEM, or featureCounts-derived TPM."*
> *"The task asks for a FRiP score but doesn't specify the exact MACS parameters needed to reproduce it — narrow or broad peaks, whether to use '--keep-dup all,' which input replicate pair, or q-value cutoff."*

**Theme 2 — Ambiguous terminology (~8 tasks)**
Key terms go undefined: "consensus peaks," "consistency," "maximum coverage depth," "confidence score of 1.0," "statistical peaks." Solvers must guess what the task actually wants.
> *"There is too much ambiguity surrounding the term 'consensus peaks.' This could mean peaks called when replicates are analyzed separately, or peaks called when replicates are merged together first."*
> *"The prompt asks to determine if peak numbers between biological replicates are 'consistent' without providing a definition for this. Consistency is a subjective term."*

**Theme 3 — Missing input file specifications (~5 tasks)**
Prompts reference data that isn't described or linked, or don't indicate that relevant outputs already exist in the working directory.
> *"The problem does not specify how to derive taxonomic classification data from the accompanying files. No mention of a taxonomy table or classification pipeline is referenced."*

**Theme 4 — Implicit pipeline, explicit gold answer**
The gold answer was generated from one specific pipeline that isn't named. Every other valid pipeline produces a different answer, all of which receive zero credit.
> *"One would normally assume that the data should be denoised. It appears the human took a simple summation of the reads without further denoising. If this is what is desired, it needs to be specified."*

**Theme 5 — Contradictory instructions (~3 tasks)**
Prompts ask for an integer but label the format as decimal, use words with two distinct bioinformatics meanings, or mix ATAC-seq and ChIP-seq framing within the same task.
> *"The prompt contains a biological contradiction: it requests FRiP 'after chromatin accessibility peak calling,' implying ATAC-seq (no input control), yet the workspace contains INPUT files, implying ChIP-seq."*

---

### 2.3 Task Difficulty *(well-calibrated overall)*

The difficulty distribution reflects the expected work distribution for mid-career bioinformatics AI training. Reviewers noted a consistent pattern: **tool complexity, not biological knowledge, is the primary difficulty gate**.

**Entry-Level (26 tasks):** Single-tool execution with well-documented pipelines; answers findable online with minimal guidance. Some reviewers flagged that the simplest tasks may be too easy for meaningful AI training.
> *"This is a very simple problem that should be easily solvable by any entry-level bioinformatician. The problem may border on being overly simple."*

**Mid-Level (70 tasks):** Requires knowing which tool to reach for (e.g., SEACR for CUT&RUN vs. MACS2 for ChIP-seq), running multi-step pipelines, and making reasonable parameter choices — without needing to innovate.
> *"The key skill gate here is knowing to use SEACR over MACS2 for CUT&RUN data — that's not obvious to someone who only knows ChIP-seq workflows, but it's also not an esoteric judgment call for anyone who has processed CUT&RUN data before."*

**Senior/Specialist (7 tasks):** Multi-tool metagenomics or multi-modal pipelines requiring awareness of multiple software ecosystems, non-obvious tool selection, and real-time troubleshooting.
> *"The model showed awareness of several software packages (prodigal, CAT, GTDB-Tk, mmseqs2, checkM, prokka, and all 3 binners), which would likely be knowledge only accessible to a professional who has worked in this specific discipline for 10+ years."*

**Calibration flag:** Several reviewers noted that pre-processed data (alignments pre-computed, reads already trimmed) artificially lowers difficulty and reduces training signal value.

---

### 2.4 Grading Methodology *(52% Fail — highest-risk dimension)*

Grading is the single most-failed dimension. More than half of all tasks have a broken rubric. Five failure modes account for nearly all cases:

**Theme 1 — Binary all-or-nothing scoring (~20+ tasks)**
No partial credit for correct methodology. A model running the right pipeline but arriving at a slightly different numeric value scores identically to a model that hallucinated.
> *"The grading is all or nothing with no partial credit, but the correct answer and the model's answer are essentially tied — separated by <0.0001. A model that ran the right pipeline, filtered correctly, and computed CV properly still gets zero."*
> *"The current grading system fails to properly distinguish between proper reasoning and misinterpretation, as it only focuses on the final numerical result without considering the methodological approach."*

**Theme 2 — Error tolerance miscalibrated (~15+ tasks)**
Tolerances are either so wide they award credit to clearly wrong answers, or so narrow they penalize natural variation from valid pipelines.
> *"A tolerance of +/-5 on a ground truth of 6 is so permissive that it would give partial credit to a model that called 1 peak or 11 peaks."*
> *"The high error tolerance prevented the discernment between good and bad work."*
> *"An error tolerance of greater than +/- 0.5 is not needed... this is shown by a 100% pass rate but 0% perfect answer rate."*

**Theme 3 — Gold answer unverifiable or wrong (~10 tasks)**
The expected answer cannot be reproduced from the provided inputs, or was derived from data not included in the workspace.
> *"The model runs NanoPlot, reads the output, and reports 8.7, which is exactly what the file shows. Despite this, it is marked wrong because the system expects 9.7."*
> *"The grading methodology fails because it relies on an unverifiable ground truth. If a grading system awards zero credit to methodologically sound model attempts simply because the expected gold answer is completely unreproducible from the given inputs, it cannot distinguish correct work from flawed execution."*

**Theme 4 — Grading rewards answer-matching, not analytical work (~8 tasks)**
A model can guess the correct output label without running any analysis and receive full credit. The rubric doesn't verify that the pipeline was actually executed.
> *"A model could guess the correct label without performing any analysis and still receive full credit. The rubric does not verify whether MACS2 was run, whether peak counts were actually computed, or whether the reasoning aligns with the data."*

**Theme 5 — Prompt ambiguity cascades into an invalid rubric**
When the prompt doesn't specify a workflow, the gold answer becomes meaningless — any answer from a different (but valid) pipeline is penalized, making the rubric unable to distinguish correct from incorrect work.
> *"Because of the ambiguity in the prompt, the grading system does not reward answers generated by a valid and reproducible workflow. The workflow generates different answers depending on how consensus peaks are defined."*

---

### 2.5 Overall Verdict Patterns

**Accept (32 tasks — 31.1%):** Consistently characterised by a deterministic, single-pathway workflow, a verifiable gold answer confirmed to match that workflow, and a tolerance that meaningfully separates correct from incorrect answers.
> *"All core dimensions pass. The task is clearly defined and aligned with standard bioinformatics workflows. The ground truth is well-supported with reproducible evidence, and the grading methodology fairly evaluates performance."*

**Revise (58 tasks — 56.3%):** Core biological concept is sound and realistic; issues are in execution — unclear phrasing, rubric needs rework, tolerance miscalibrated, or gold answer needs to be re-derived using a now-specified pipeline. Actionable pattern across Revise cases:
1. Add tool/parameter specification to the prompt.
2. Re-derive the gold answer using that specified pipeline.
3. Set tolerance to reflect only stochastic variation (rounding, minor version differences), not methodological variation.

**Reject (13 tasks — 12.6%):** Gold answer is unverifiable or wrong AND the grading is broken — considered unfixable without rebuilding from scratch (new data, new gold answer, new rubric). Often accompanied by a task concept that is either contrived or too narrow.
> *"It seems highly likely that the AI was not provided with the appropriate data files to successfully perform the analysis. The AUC value of 0.0008 that was calculated implies a nearly empty file."*

---

## 3. Quick Reference: Pass Rates by Dimension

```
Task Realism         ████████████████████░  96.1% Pass
Task Clarity         █████████████░░░░░░░░  68.0% Pass
Task Difficulty      (distribution, not pass/fail)
Grading Methodology  █████████░░░░░░░░░░░░  47.6% Pass
Overall Accept Rate  ██████░░░░░░░░░░░░░░░  31.1% Accept
```

**Bottom line:** The batch's core biological concepts are strong (high realism), but task execution quality needs significant work. Fixing grading methodology and improving clarity specification will together resolve the majority of Revise and Reject cases.
