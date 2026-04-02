# [int] Apr-1 Tasks 319 — Reviewer Feedback Summary & Remediation Plan

**Total tasks reviewed:** 319
**Date compiled:** 2026-04-01

---

## 1. Dimension Score Counts

### Task Realism
| Verdict | Count | % |
|---------|-------|---|
| Pass | 310 | 97.2% |
| Fail | 9 | 2.8% |

### Task Clarity
| Verdict | Count | % |
|---------|-------|---|
| Pass | 181 | 56.7% |
| Fail | 138 | 43.3% |

### Task Difficulty
| Level | Count | % |
|-------|-------|---|
| Entry-Level | 84 | 26.3% |
| Mid-Level | 226 | 70.8% |
| Senior / Specialist | 9 | 2.8% |

### Grading Methodology
| Verdict | Count | % |
|---------|-------|---|
| Pass | 179 | 56.1% |
| Fail | 140 | 43.9% |

### Overall Verdict
| Verdict | Count | % |
|---------|-------|---|
| Accept | 99 | 31.0% |
| Revise | 198 | 62.1% |
| Reject | 22 | 6.9% |

---

## 2. Main Themes by Dimension

### 2.1 Task Realism *(97% Pass — consistently strong)*

Realism remains the most reliable dimension. 310 of 319 tasks pass. The 9 failures are tightly patterned.

**Why tasks pass:**

**Theme 1 — Standard QC step in a recognized pipeline**
Tasks map to named, repeatable operations in established workflows (CUT&RUN, ATAC-seq, MAG, ampliseq, NanoSeq). Reviewers invoke pipeline names and tool ecosystems to confirm authenticity.
> *"Computing FRiP scores is a standard quality control metric in CUT&RUN/CUT&Tag epigenomics pipelines. The setup includes realistic BAM files, IgG controls, chromosome subset data, and appropriate conda environments featuring SEACR and MACS2."*
> *"Running MACS2 peak calling on CUT&RUN data with an IgG control and computing mean fold enrichment is a routine step in chromatin profiling workflows."*

**Theme 2 — Authentic tools, file formats, and naming conventions**
Even for simple tasks, genuine ecosystems (samtools, deepTools, DADA2, CheckM), realistic file formats (BAM/FASTQ/GFF/narrowPeak), and accurate naming conventions (e.g., `.mLb.clN.sorted.bam`) ground tasks in real professional practice.
> *"The specific .mLb.clN.sorted.bam naming convention accurately reflects realistic, standard nf-core pipeline outputs."*
> *"The tools available in the environment (DAS_Tool, Prokka, Prodigal, CheckM) are standard for this domain, and the data structure mirrors real nf-core/mag pipeline outputs."*

**Why tasks fail (9 cases):**

**Theme 3 — Contrived metric with no professional motivation**
Some tasks pass on tools and data but are flagged for asking a question a professional would never ask in isolation (PCA on genome-wide coverage in CUT&RUN, summing peaks across all experimental conditions, averaging GC content across binned samples).
> *"The request for PCA on genome-wide coverage is somewhat unusual for CUT&RUN data — a peak-based PCA would be more common."*
> *"There is very little rationale for summing peaks between different groups. These experiments are almost always performed to analyze alterations in chromatin accessibility between treatments."*

---

### 2.2 Task Clarity *(43% Fail — primary failure driver)*

138 of 319 tasks fail clarity. Five tightly recurring patterns account for nearly all failures.

**Theme 1 — Tool or pipeline not specified; different valid tools produce incompatible answers (~40+ tasks)**
The dominant failure. Prompts request a specific numeric output without naming the required tool (MACS2 vs. SEACR, AdapterRemoval vs. Trim Galore vs. fastp, Medaka vs. Clair3, MEGAHIT vs. SPAdes). Different valid tools produce completely different numerical outputs.
> *"The gold answer (23) is derived specifically from AdapterRemoval output, yet the task never directs the user to use this specific tool. Multiple valid tools are available: Trimmomatic yields 91, Trim Galore yields 28, fastp yields 5, and AdapterRemoval yields 23."*
> *"All ten runs consistently landed around 0.3486 instead of 0.3852 — the same wrong answer every time — telling you the model is making reasonable but different assumptions from whatever the gold standard pipeline used. That's a clarity failure."*
> *"'Neural network-based variant calling' is a category, not a tool name. Medaka and Clair3 are both neural network-based, both widely used, and both reasonable choices given only the prompt."*

**Theme 2 — Required intermediate or source file missing from /tmp/files/ (~25+ tasks)**
The gold answer derives from a pre-computed file (flagstat output, narrowPeak file, DADA2_stats.tsv, GTF annotation, stats file) not included in the task environment. The model must recompute from scratch with unspecified parameters, yielding different results.
> *"The task omits the critical source file (NA12878_R1.flagstat). The missing file forces the model to perform a de novo alignment from raw FASTQ reads. The model's valid recomputation yielded 86 instead of the expected 91."*
> *"The DADA2_stats.tsv is missing from /tmp/files/. The model ran DADA2 denoise-ccs with default parameters and obtained 24.8% — a large discrepancy driven entirely by unspecified parameter choices."*
> *"The question does not clearly say whether the answer should be taken from an existing narrowPeak file or whether peak calling should be run again from the BAM files."*

**Theme 3 — Binning tool unspecified in metagenomics tasks (~15+ tasks)**
Prompts asking for a bin-level metric (completeness, contig count, GC content) without specifying which of the three available binning tools (MetaBAT2, CONCOCT, MaxBin2) to use produce entirely different answers depending on which tool is chosen.
> *"The gold answer is based specifically on MaxBin2 output, but nothing in the prompt directs the user to use MaxBin2 over MetaBat2 or CONCOCT. The 20% pass rate across 10 attempts confirms the answer is highly dependent on which binning tool is arbitrarily selected."*
> *"The model ran four tools and got 4.17% for all using lineage_wf, while the gold answer shows 12.07% for MaxBin2 — a gap so large it suggests a completely different pipeline was used."*
> *"The environment provides MEGAHIT, SPAdes, and Flye, each producing fundamentally different assemblies and different L50 values. There is no single 'default' assembler for metagenomic short reads."*

**Theme 4 — Contradictory output format instructions (~10+ tasks)**
Prompts specify "Answer format: integer" in the question body and then "Put your answer as a decimal" in the wrapper tag — creating confusion about the expected type.
> *"There is an inconsistency in the answer format, which is required to be integer, and the next command asks to provide the answer as a decimal format."*
> *"The output requirements are somewhat mixed: the prompt states that the output is an integer, but it also instructs to provide an answer as a decimal."*

**Theme 5 — Ambiguous definition of the key computed metric (~15+ tasks)**
Prompts fail not from tool ambiguity but from an undefined or poorly-defined metric: "non-chimeric reads" without specifying the denominator, "basepair retention" without specifying which pipeline step, "mismatch error rate" without defining the formula.
> *"The problem does not define whether 'total reads' refers to pre-filtering, pre-denoising, or original sequencing reads. Because the expected denominator is not stated, the core expected output is not clear."*
> *"'Basepair retention' could refer to the cutadapt primer-trimming step, a quality-filtering step, a length-filtering step, or the cumulative retention across all preprocessing steps."*
> *"The problem asks for 'total base pair mismatches' but does not define whether this refers to the samtools NM-derived field or true base substitutions (NM minus indels)."*

---

### 2.3 Task Difficulty *(well-distributed; environmental gaps inflate some ratings)*

**Entry-Level (84 tasks — 26%):** Simple file parsing or direct metric extraction from a named file with a clearly labeled field. No pipeline design, parameter judgment, or biological interpretation required.
> *"The task involves locating the correct computeMatrix output file and extracting a clearly labeled value — requiring only basic familiarity with bioinformatics file formats rather than advanced analytical expertise."*
> *"Running a read-trimming tool on a FASTQ file and counting distinct read lengths using awk | sort | uniq | wc -l is a basic bioinformatics task."*

**Mid-Level (226 tasks — 71%):** Requires knowing which tool to reach for, understanding experimental design (e.g., matching IgG controls), multi-step pipeline coordination, and domain-specific parameter choices.
> *"You need to know how to call peaks with MACS2, understand paired-end mode, know to pair the right input control with treatment sample, and be able to count reads overlapping with samtools. A beginner wouldn't know which tools to reach for or how to set up the MACS2 command."*
> *"Handling PacBio CCS long reads requires knowing non-default parameters (PacBioErrfun, BAND_SIZE=32). This goes far beyond entry-level file parsing and requires a mid-level understanding of amplicon sequencing workflows."*

**Senior/Specialist (9 tasks — 3%):** Missing environment resources force multi-tool improvisation, iterative troubleshooting, and expert judgment across several software ecosystems simultaneously.
> *"The model had to inspect multiple bin sets, handle missing databases, determine alternative taxonomy routes, troubleshoot CheckM failures, and interpret protein-marker-based classification."*
> *"The model determined that MACS2 was not the appropriate method because the peaks were too broad/sparse. The workflow tries different methods and converts file types to address compatibility issues with each method."*

**Calibration flag:** Missing files and databases inflate effective difficulty above the intended level. The correct fix is to provide the missing resource, not raise the difficulty rating.
> *"This is a much more advanced workflow, but with the correction of adding in the reference database this problem would be mid-level."*
> *"The matrix file was missing — the model could not simply extract the real answer. The task's difficulty is inflated by the environment gap, not by genuine analytical complexity."*

---

### 2.4 Grading Methodology *(44% Fail — second-highest concern)*

**Theme 1 — Gold answer irreproducible from provided data; correct pipelines score 0.0 (~40+ tasks)**
The most critical failure: the gold answer was generated from a pipeline run whose intermediate files are not in the model's environment. Any correct workflow systematically produces a different value.
> *"The model runs NanoPlot, reads the output, and reports 8.7, which is exactly what the file shows. Despite this, it is marked wrong because the system expects 9.7. The grader is not rewarding correct analysis — it favors an incorrect benchmark value."*
> *"All three failures trace to the same root cause: the missing critical source file renders the gold answer irreproducible and the grading unfair across valid methodological choices."*
> *"The model runs a completely valid pipeline on all available bigWig files, but the task does not succeed because the gold answer was generated from a referenced analysis named 'h3k4me3_R1', which does not exist."*

**Theme 2 — Tolerance miscalibrated: too wide or too narrow (~35+ tasks)**
Wide tolerances produce 100% pass / 0% perfect rate patterns — every attempt passes but none is correct. Narrow tolerances penalize correct pipelines for minor computational differences.
> *"90.0% pass rate and 0.0% perfect rate means most attempts were good enough to pass even though none matched the expected answer exactly. The grader is rewarding answers that are only close instead of separating right from wrong."*
> *"The grading system does not distinguish between a population standard deviation (ddof=0) and a sample standard deviation (ddof=1). The model reports population std 9 out of 10 times, yet it passes each time despite being incorrect."*
> *"The model derived an answer of 1.9 and got a score of 0.0 despite a well-documented workflow. A tolerance of 0.1 around a value of 2.0 is too narrow."*

**Theme 3 — Exact-match grading penalizes valid alternatives when prompt is underspecified (~25+ tasks)**
When clarity fails (tool unspecified, parameter missing), a tight exact-match rubric anchored to one hidden tool's output assigns zero to every valid alternative — making the grading a tool-guessing test rather than a methodology quality test.
> *"This grading logic uses a rigid numerical target with a narrow 0.1 tolerance, actively penalizing good-faith, technically valid solutions (like using MACS2 instead of SEACR) that produce different baselines. The 0% pass rate across 10 attempts proves that the tolerance cannot bridge the gap between distinct algorithmic approaches."*
> *"The grading does not account for applying correct methodological approach — it relies on a single expected answer derived from an unspecified peak calling approach. Correct answers may be penalized if they do not match the expected workflow."*

**Theme 4 — Final label-only grading rewards guessing (~10 tasks)**
Binary or string-match rubrics for small-choice tasks (two samples, yes/no, categorical labels) can be passed by guessing without running any analysis.
> *"The grading does not consider how the answer was obtained. The correct answer could be achieved through guesswork and still be marked correct if it matches the expected output string."*
> *"The current grading rewards correctly identifying the correct option when only two are given — leading to a 50% chance of success regardless of model output."*

**Theme 5 — Well-calibrated abs_error with pass-rate confirmation (Pass pattern)**
Accepted grading uses absolute error tolerances verified by model attempt data showing the tolerance discriminates correct from incorrect work.
> *"The abs_error tolerance of ±9.33 (~10% of 87) is reasonable for a contig count that can vary modestly with depth file inputs. The 90% pass rate and 10% perfect rate confirm the grading is functional and discriminating."*
> *"The grading metric of 5% tolerance consistently rewarded proper methodological execution. When bowtie2 is implemented correctly, the model-derived answer met the grading criteria for all 10 runs."*

---

### 2.5 Overall Verdict Patterns

**Accept (99 tasks — 31%):** All dimensions pass. Gold answer reproducible from workspace, clear methodology, calibrated tolerance. High pass rate cited as confirming evidence.
> *"No issues present that would require revision. All core dimensions pass: realistic, clearly defined, appropriately scoped, and fully verifiable."*
> *"The 100% perfect score across all model attempts demonstrates that the grading system reliably rewards correct reasoning and execution without ambiguity."*

**Revise (198 tasks — 62%):** Core biological concept is sound; issues are execution-level and concretely fixable. Reviewers consistently prescribe named, specific remedies.
> *"The fix is straightforward: specify 'broad peak calling' (such as, 'using MACS2 with the --broad flag') and clarify the assay type."*
> *"Three specific fixes required: (1) specify the tool and parameters in the prompt; (2) either install the required tool in the environment or update the gold answer; (3) re-evaluate the grading tolerance to align with the specified pipeline."*

**Reject (22 tasks — 7%):** Gold answer demonstrably wrong, unverifiable, or structurally incompatible with a correct workflow — not fixable without rebuilding from scratch.
> *"All model attempts scored 0.0, suggesting the grading does not tolerate valid alternative interpretations and fails to recognize partial progress or correct reasoning."*
> *"The gold answer is incorrect and unverifiable. The problem does not explicitly indicate the pipeline needed for computation, making it impossible to consistently validate."*

---

## 3. Quick Reference: Pass Rates by Dimension

```
Task Realism         ████████████████████░  97.2% Pass
Task Clarity         ████████████░░░░░░░░░  56.7% Pass
Task Difficulty      (distribution, not pass/fail)
Grading Methodology  ████████████░░░░░░░░░  56.1% Pass
Overall Accept Rate  ███████░░░░░░░░░░░░░░  31.0% Accept
```

**Bottom line:** At 319 tasks, this is the largest batch analyzed. The pattern is consistent with prior batches: biological concepts and professional framing are strong (realism 97%), while **clarity** (43% fail) and **grading** (44% fail) remain the primary failure modes. Both trace almost entirely to the same upstream cause — unspecified tools, missing intermediate files, and undefined metric formulas. The Reject rate (7%) is the highest of any batch, driven by tasks where the gold answer is demonstrably wrong or unverifiable. Fixing prompt specification and environment file completeness at the authoring stage will resolve the majority of Revise and Reject outcomes.
