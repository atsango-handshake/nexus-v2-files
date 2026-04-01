# Delivery 4-1 — Reviewer Feedback Summary

**Total tasks reviewed:** 57
**Date compiled:** 2026-04-01

---

## 1. Dimension Score Counts

### Task Realism
| Verdict | Count | % |
|---------|-------|---|
| Pass | 57 | 100.0% |
| Fail | 0 | 0.0% |

### Task Clarity
| Verdict | Count | % |
|---------|-------|---|
| Pass | 23 | 40.4% |
| Fail | 34 | 59.6% |

### Task Difficulty
| Level | Count | % |
|-------|-------|---|
| Entry-Level | 10 | 17.5% |
| Mid-Level | 47 | 82.5% |
| Senior / Specialist | 0 | 0.0% |

### Grading Methodology
| Verdict | Count | % |
|---------|-------|---|
| Pass | 29 | 50.9% |
| Fail | 28 | 49.1% |

### Overall Verdict
| Verdict | Count | % |
|---------|-------|---|
| Accept | 10 | 17.5% |
| Revise | 44 | 77.2% |
| Reject | 3 | 5.3% |

---

## 2. Main Themes by Dimension

### 2.1 Task Realism *(100% Pass — perfect score)*

This is the first batch where every single task passes realism. Reviewers consistently grounded their assessments in the same three observations:

**Theme 1 — Tasks map to standard, established bioinformatics QC workflows**
Every task corresponds to a recognized, repeatable step in a real pipeline — QC extraction, adapter trimming, chimera detection, peak counting, structural variant parsing, etc.
> *"Calling peaks on a CUT&RUN library and counting the number of peaks is a common task. The workflow of aligning reads to obtain BAM files followed by calling peaks using MACS2 is an industry standard approach."*
> *"Calculating the percentage of reads lost during DADA2 denoising is a routine quality-control metric in amplicon sequencing analysis. The data, tools, and workflow accurately reflect what someone would genuinely encounter in this domain."*

**Theme 2 — Authentic tools, file formats, and naming conventions**
Reviewers confirmed realism by identifying genuine ecosystems (samtools, MACS2, deepTools, DADA2, QIIME2, ITSx, geNomad) and realistic file naming (e.g., `.mLb.clN.sorted.bam` from nf-core pipelines).
> *"The specific .mLb.clN.sorted.bam naming convention accurately reflects realistic, standard nf-core pipeline outputs."*
> *"The input data — a FASTA file of 46 ASV sequences and a UNITE UTAX reference database — are exactly the kind of files a bioinformatician would encounter when running a fungal ITS amplicon pipeline."*

**Theme 3 — Realism is independent of execution quality**
Reviewers explicitly separated the realism judgment from clarity and grading failures — a poorly specified or ungradeable task can still represent authentic professional work.
> *"The tools and setup shown in the HTML are consistent with normal bioinformatics practice, even though the task has other problems around reproducibility and grading."*
> *"Nothing about the setup is a contrived puzzle or an artificially constructed scenario."*

---

### 2.2 Task Clarity *(60% Fail — highest failure rate)*

34 of 57 tasks fail clarity — the most problematic dimension by far. Five tightly recurring failure patterns account for nearly all failures.

**Theme 1 — Tool or pipeline not specified; different valid tools produce different answers (~15+ tasks)**
The prompt asks for a specific numeric output but does not name which tool, aligner, or pipeline should generate it. Multiple legitimate choices produce incompatible results.
> *"The prompt asks for the 'average mismatch error rate' for Nanopore reads but does not specify which alignment tool or specific parameters (e.g., -x map-ont for minimap2) should be used. Since different tools produce slightly different alignment stats, a deterministic answer is impossible without these constraints."*
> *"The core question asks to identify a conjugation gene on a specific contig but fails to specify the required analytical tool. The gold answer is derived specifically from geNomad's plasmid summary output, yet the prompt never instructs the user to use geNomad."*
> *"The task states that the goal is to compute the mean taxonomic classification confidence score but does not define the classification method (e.g., QIIME2, SINTAX, BLAST)."*

**Theme 2 — Required intermediate or source file missing from /tmp/files/ (~10+ tasks)**
The gold answer derives from a pre-computed file (flagstat output, BAM, stats file, annotation GTF, DADA2_stats.tsv) that was not included in the task environment, forcing de novo computation that yields different results.
> *"The task omits the critical source file (NA12878_R1.flagstat) required to reach the gold answer. The missing file forces the model to perform a de novo alignment from raw FASTQ reads. Because the prompt does not specify alignment parameters, this leads to non-deterministic results — the model's valid recomputation yielded 86 instead of the expected 91."*
> *"The DADA2_stats.tsv is missing from /tmp/files/, and the task does not specify which pipeline parameters or primer sequences to use."*
> *"There is a significant ambiguity regarding the lack of a .gtf annotation file. This file is a necessary input for HOMER and should be consistent between the gold answer pipeline and the AI pipeline."*

**Theme 3 — MACS2 parameters unspecified for peak-calling tasks (~8+ tasks)**
MACS2 is highly sensitive to narrow vs. broad peak mode and q-value cutoff. Without these, the same BAM files can yield peak counts differing by hundreds or thousands.
> *"The prompt lacks methodological details — specifically failing to define whether to use narrow or broad peak calling and the q-value cutoff. The model's narrow peak total (2924) and broad peak total (3582) are both methodologically reasonable but yield substantially different results, and neither matches the gold answer of 3503."*
> *"The prompt asks for a FRiP score but completely fails to specify the peak-calling parameters (narrow vs. broad peaks, q-value cutoffs). All 10 model attempts applied a methodologically sound approach but consistently landed at ~0.35 instead of the expected 0.39."*

**Theme 4 — Primer sequences and cutadapt parameters absent for amplicon tasks (~5+ tasks)**
Amplicon tasks frequently omit primer sequences, minimum overlap settings, and error rate thresholds — parameters that directly determine trimming output and propagate through downstream read counts.
> *"The specific tool used should be specified. In addition, there are values that must be set in cutadapt such as minimum overlap and allowable error rate from the primer sequence. Lastly, the primer sequences are not given."*
> *"The prompt fails to provide the exact ITS primer sequences, does not specify the required cutadapt parameters, and ambiguously uses the phrase 'processed ITS reads.' The model attempted multiple valid cutadapt configurations, yielding widely varying read counts (971, 882, and 866)."*

**Theme 5 — What a passing clarity looks like**
Passing tasks unambiguously name the sample, metric, output format, and source file — leaving no methodological guesswork.
> *"The prompt clearly specifies the sample (H3K4me3 replicate 1), the tool output to examine (SEACR peaks), the two metrics to compare, and the expected answer format (yes/no string)."*
> *"The problem statement provides a specific metric, a precise numerical threshold, and a defined output format. The clarity is further evidenced by the 100% perfect rate across all 10 model attempts."*

---

### 2.3 Task Difficulty *(Mid-Level dominant; no Senior tasks)*

The batch skews heavily toward Mid-Level (82%). No Senior/Specialist tasks are present.

**Entry-Level (10 tasks — 18%):** Single-command extractions, file parsing, or binary comparisons from existing outputs. No biological judgment or pipeline design required.
> *"The task involves locating an existing MACS2 narrowPeak file and extracting the maximum value from a specific column. This can be completed using simple command-line tools without parameter tuning, peak-calling expertise, or biological interpretation."*
> *"A junior professional could complete this task with minimal guidance. It requires extracting a specific module from FastQC output files, computing simple means, and comparing two values."*

**Mid-Level (47 tasks — 82%):** Multi-step pipeline coordination, tool-chain knowledge, and domain-specific judgment (e.g., choosing broad vs. narrow MACS2 mode for a specific histone mark).
> *"A professional needs working knowledge of the CUT&RUN workflow: converting BAM files to fragment bedgraphs, running SEACR with an IgG control, and understanding the SEACR output column format. An entry-level practitioner would not know the BAM-to-bedgraph conversion steps from memory."*
> *"Selecting the correct mode for H3K27me3 requires recognizing that the --broad flag is required. This requires specific knowledge of the histone modification and what sort of peaks it will produce in CUT&RUN sequencing."*
> *"Correctly interpreting fingerprint AUC values — especially that lower AUC indicates stronger enrichment — is not immediately intuitive. It requires familiarity with chromatin profiling quality metrics and experience with deepTools."*

**Calibration note:** A small number of tasks are rated Mid-Level partly because prompt ambiguity artificially inflates perceived complexity. Stripped of the missing specifications, the underlying mechanics are Entry-Level.
> *"Stripped of the artificial ambiguity caused by the poorly specified prompt, the mechanical execution fundamentally requires running a standard cutadapt command on a single sample and performing simple division."*

---

### 2.4 Grading Methodology *(49% Fail — near-even split)*

28 tasks fail grading — almost exactly half the batch. Four distinct failure modes account for all cases.

**Theme 1 — Gold answer irreproducible from provided data; correct pipelines score 0.0 (~12+ tasks)**
The gold answer is anchored to a hidden pre-computed file or an undocumented pipeline. Any solver following standard professional practice arrives at a different value and is penalized.
> *"The transcript shows that a deepTools run using the documented BAMs and standard options yields a PC1 eigenvalue of 1527.37, which still scores 0.0. The grading effectively rewards guessing the unseen configuration instead of correctly running and interpreting the tools."*
> *"A reasonable workflow from the visible FASTQ files produced 47.34%, while the gold answer is 11.52%. All attempts received 0.0."*
> *"Valid, methodologically sound approaches using Picard yield ~262–269, while the expected answer is 300. Correct professional practice is penalized."*

**Theme 2 — Tolerance too wide; 100% pass rate, 0% perfect rate (~8+ tasks)**
When the error band is so generous that every attempt passes regardless of methodology, the rubric cannot discriminate correct from incorrect work.
> *"The mean score was 0.850, the pass rate was 100.0%, and the perfect rate was 0.0%. Every attempt passed even though none matched the expected answer exactly — a strong sign the grader is not separating correct work from incorrect work."*
> *"The grading expectation is 1 peak, but an error tolerance of 5 allows the model to consistently 'pass' while not arriving at the gold answer. Consider being more stringent."*
> *"The error tolerance of 10% is too large and may result in encouraging inaccurate or incomplete workflows."*

**Theme 3 — Rubric validates the final label only, not whether the correct workflow was executed (~5+ tasks)**
String-match or numeric-only rubrics award full credit to guesses or incorrect workflows that happen to produce the right output label.
> *"The model did not reproduce the expected workflow and instead computed AUC values on a different scale, yet was assigned full credit. The current grading system can reward answers derived from incorrect or inconsistent workflows, as long as the final label matches."*
> *"The grading does not evaluate whether the solver actually carried out the SEACR workflow. A correct guess could receive full credit without demonstrating genuine task completion."*
> *"The current grading rewards correctly identifying the correct option when only two are given, leading to a 50% chance of success regardless of model output."*

**Theme 4 — Gold answer is factually wrong; the rubric encodes an incorrect answer (~3 tasks)**
A distinct failure mode where no amount of correct execution can pass the rubric because the anchor value itself is wrong.
> *"A correct, data-driven solution would identify Hypocreales as the order with the highest diversity, yet this receives a score of 0 because the rubric specifies Helotiales."*
> *"There is a concrete mismatch between the rubric's ground truth (478) and the actual output in the transcript (480). Genuinely correct work is systematically scored as imperfect."*

**Theme 5 — Well-calibrated abs_error rubrics that pass**
Accepted grading uses calibrated absolute error tolerances with pass-rate evidence confirming discriminative power.
> *"Those employing the correct scope achieved a score of 95.91% and a perfect score of 1.0. Those employing incorrect workflows achieved 92.66%, which corresponded to a failing score of 0.35."*
> *"The abs_error grading with a COUNT_SCALED tolerance of 127.3 (~5% of the gold answer) is well-calibrated. The 60% pass rate and 20% perfect rate across 10 attempts confirm the task discriminates between good and poor approaches fairly."*

---

### 2.5 Overall Verdict Patterns

**Accept (10 tasks — 18%):** All three actionable dimensions pass. Gold answer reproducible from workspace, clear methodology, calibrated tolerance confirmed by pass-rate evidence.
> *"All five dimensions pass. The task is realistic, unambiguous, appropriately scoped, has a verifiable ground truth, and is graded fairly."*

**Revise (44 tasks — 77%):** Core biological concept is sound and fixable. Reviewers noted a consistent "trickle-down" failure pattern: a single missing file or unspecified parameter causes clarity to fail, which makes the gold answer irreproducible, which breaks grading. The same concrete fixes resolve all three at once.
> *"All three failures are a trickle-down effect of the missing critical source file, which renders the gold answer irreproducible and the grading unfair."*
> *"The fixes are concrete and actionable: include the missing file in /tmp/files/ OR explicitly document the exact pipeline command and parameters."*

**Reject (3 tasks — 5%):** Gold answer is demonstrably incorrect, rewards wrong behavior, or has a structural ambiguity that cannot be resolved by adding files or parameters.
> *"The grading rewards an incorrect answer and penalizes correct analysis — this is a broken evaluation, not a minor issue."*
> *"The task has two critical structural problems: an ambiguous prompt that produces two valid interpretations, and a ground truth that is not independently verifiable."*

---

## 4. Quick Reference: Pass Rates by Dimension

```
Task Realism         ████████████████████  100.0% Pass
Task Clarity         ████████░░░░░░░░░░░░   40.4% Pass
Task Difficulty      (distribution, not pass/fail)
Grading Methodology  ██████████░░░░░░░░░░   50.9% Pass
Overall Accept Rate  ████░░░░░░░░░░░░░░░░   17.5% Accept
```

**Bottom line:** This batch has perfect realism scores — the underlying biology and professional framing are completely sound. The failure pattern is concentrated in **clarity** (60% fail) and **grading** (49% fail), and they share a single root cause: missing intermediate files and unspecified tools/parameters make the gold answer irreproducible, which then makes fair grading impossible. The fix is upstream and concrete — provide the files, name the tools, specify the parameters.
