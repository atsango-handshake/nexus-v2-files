# Delivery 3-30 — Reviewer Feedback Summary

**Source file:** `delivery-3-30`
**Total tasks reviewed:** 160
**Date compiled:** 2026-03-30

---

## 1. Dimension Score Counts

### Task Realism
| Verdict | Count | % |
|---------|-------|---|
| Pass | 155 | 96.9% |
| Fail | 5 | 3.1% |

### Task Clarity
| Verdict | Count | % |
|---------|-------|---|
| Pass | 89 | 55.6% |
| Fail | 71 | 44.4% |

### Task Difficulty
| Level | Count | % |
|-------|-------|---|
| Entry-Level | 48 | 30.0% |
| Mid-Level | 110 | 68.8% |
| Senior / Specialist | 2 | 1.3% |

### Grading Methodology
| Verdict | Count | % |
|---------|-------|---|
| Pass | 101 | 63.1% |
| Fail | 59 | 36.9% |

### Overall Verdict
| Verdict | Count | % |
|---------|-------|---|
| Accept | 57 | 35.6% |
| Revise | 97 | 60.6% |
| Reject | 6 | 3.8% |

---

## 2. Main Themes by Dimension

### 2.1 Task Realism *(97% Pass — strongest dimension)*

Realism continues to be the most reliable dimension. Almost all tasks use authentic tools, file formats, and domain-appropriate workflows. The 5 failures are narrow, specific edge cases.

**Why tasks pass:**
- Standard professional workflow steps — QC, adapter trimming, peak calling, genome binning, read alignment, variant calling — are immediately recognized as real bioinformatics work.
- Authentic tools (deepTools, MACS2, DADA2, Sniffles, MetaBAT2), data types (BAM, FASTQ, VCF, bigWig), and proper conda environments are present.

> *"Running MACS2 peak calling on CUT&RUN data with IgG controls and comparing peak counts between histone marks is standard epigenomics work that an actual bioinformaticist would do."*
> *"The tools, data types, and overall environment setup accurately reflect genuine daily bioinformatics work."*

**Why tasks fail (5 cases):**

1. **Missing raw reads** — Forces non-standard k-mer-based workarounds that no professional would use with real data available.
   > *"In a real experiment, the raw read files would be available. While the task is accomplishable without them using k-mer multiplicities as a coverage estimate, a real experiment would not be limited by the lack of this data, making this problem artificially constrained."*

2. **Inaccessible reference database** — UNITE fungal ITS database unavailable, making taxonomy tasks unrealistic rather than merely difficult.
   > *"The inaccessibility of the UNITE fungal ITS reference database presents a significant obstacle — a human analyst would typically prioritize acquiring it or an equivalent before proceeding."*

3. **Contrived aggregation logic** — Combining genome bins from three different algorithms with no professional motivation.
   > *"The complexity of this task is artificially constructed. The overall process would involve obtaining values from existing output summaries rather than processing a full assembly, mapping, and binning pipeline."*

4. **Unusual metric isolated from practical context** — Metrics that professionals would only compute as part of a broader investigation, never in isolation.

---

### 2.2 Task Clarity *(44% Fail — primary failure driver)*

Task Clarity is the single most problematic dimension. 71 of 160 tasks fail — the highest failure rate of any dimension in this batch. Failures cluster tightly around four patterns.

**Theme 1 — Tool not specified, but answer is tool-dependent (~25+ tasks)**
Different valid tools produce materially different numerical outputs. The gold answer is only reachable by guessing the intended tool.
> *"The prompt asks for distinct read lengths 'after adapter trimming' but critically fails to specify which trimming tool or parameters to use. Multiple valid tools are available: Trimmomatic yields 91, Trim Galore yields 28, fastp yields 5, and AdapterRemoval yields 23. The gold answer (23) is derived specifically from AdapterRemoval."*
> *"'Neural network-based variant calling' is a category, not a tool name. Medaka and Clair3 are both neural network-based, both widely used, and both reasonable choices given only the prompt."*
> *"The task never specifies which tool should be used for adapter counting. Different tools give very different counts depending on their parameters."*

**Theme 2 — Critical pipeline parameters left unspecified (~20+ tasks)**
The tool is named or implied, but key parameters that change the output (broad vs. narrow peak mode, MACS2 q-value, DADA2 filter settings, normalization method) are absent.
> *"The prompt asks for results of 'the DESeq2 PCA analysis' as though there is a single canonical result, but no pre-computed PCA output is provided. Each step involves critical parameter choices (merge distance, featureCounts settings, VST vs. rlog normalization) that legitimately alter the final PC1 variance percentage."*
> *"The question asks for MACS2 peak counts but doesn't specify narrow versus broad peak calling mode. A competent professional would use broad calling for H3K27me3 — the biologically correct approach — and get the opposite answer from the gold standard."*

**Theme 3 — Ambiguous terminology and metric definitions (~15+ tasks)**
Key terms go undefined: "weakest" E-value, "sample clustering," "total alignments," "basepair retention," "statistical peaks." Each admits multiple valid interpretations.
> *"The prompt asks for the 'weakest' bacterial rRNA E-value. While reasonable to interpret as 'highest,' it is unclear precisely what is meant — it could theoretically mean 'lowest.'"*
> *"The phrase 'sample clustering analysis' is too ambiguous. Different methodological approaches can rank reference pairs differently."*
> *"The problem asks for 'total base pair mismatches' but does not explicitly define whether this refers to the samtools NM-derived field or true base substitutions (NM minus indels)."*

**Theme 4 — Metagenomics binning tool ambiguity (~10+ tasks)**
A specific recurring failure: prompts provide data processable by three different binning tools (MetaBAT2, MaxBin2, CONCOCT) without specifying which one, producing fundamentally different bin sets.
> *"The question asks about 'the genome bin with the highest completeness' but fails to specify which binning tool to use. Three different algorithms are available in the environment, and each produces entirely different bins with different completeness values."*
> *"The phrase 'all metagenome-assembled genomes' is critically ambiguous — it can refer to raw bins from individual binning tools (27 total) or to the refined, non-redundant set from DAS_Tool (2 bins)."*

**Theme 5 — Tasks that pass clarity: single deterministic extraction**
Passing tasks specify one file and one extraction step, leaving no room for interpretation.
> *"The question explicitly asks for total alignments including primary, secondary, and supplementary — exactly what the first line of samtools flagstat reports. There is no ambiguity."*
> *"The task clearly states the specific question, the answer format, the location of the input file, and the environment in which computation must occur."*

---

### 2.3 Task Difficulty *(generally well-calibrated; environmental gaps inflate some ratings)*

**Entry-Level (48 tasks — 30%):** One tool, one command, read a labeled value from output. No parameter judgment required.
> *"Running NanoPlot on a FASTQ file and reading a single value is about as basic as nanopore bioinformatics gets. It's a one-command operation with well-documented tools, no parameter tuning required."*
> *"The task involves running FastQC and extracting a reported metric from the output. Basic familiarity with sequencing QC tools is sufficient — no complex analysis or domain-specific judgment needed."*

**Mid-Level (110 tasks — 69%):** Multi-step pipeline, domain knowledge for tool selection, matching experimental controls (e.g., IgG), informed parameter choices.
> *"The task requires working knowledge of MACS2 peak calling, understanding of CUT&RUN experimental design (treatment vs. IgG control pairing), correct identification of paired-end data formats, and ability to parse MACS2 output files."*
> *"A mid-level bioinformatician who has run ATAC-seq or ChIP-seq pipelines before would handle this task without major difficulty: adapter trimming, alignment, duplicate removal, MAPQ filtering, and samtools read counting — all standard working knowledge."*

**Senior/Specialist (2 tasks — 1%):** Missing reference databases force inventive non-standard workarounds, iterative troubleshooting, and deep multi-tool ecosystem knowledge.
> *"This extensive pipeline engineering — locating bundled reference genomes from a Conda package, creating a custom BLAST database from scratch, deciphering complex taxonomic strings, and cross-validating with SINTAX — goes well beyond basic analysis and demands Senior/Specialist knowledge."*
> *"The model had to inspect multiple bin sets, handle missing databases, determine alternative taxonomy routes, troubleshoot CheckM failures, and interpret protein-marker-based classification."*

**Calibration flag:** Missing databases and primers artificially inflate effective difficulty above the intended level. The correct fix is to provide the missing resource, not to raise the difficulty rating.
> *"The lack of a provided reference genome and no internet access to acquire one forces a non-standard alternative workflow that is both less-standard and more prone to incomplete analysis."*
> *"Although the required computation is a simple percentage calculation (entry-level), the prompt suggests a complete amplicon denoise workflow, creating a misaligned operation that artificially inflates difficulty."*

---

### 2.4 Grading Methodology *(37% Fail — second-highest concern)*

**Theme 1 — Error tolerance too narrow, punishes valid alternative pipelines (~20+ tasks)**
The most common grading failure: tolerance is sized for rounding differences, but real variation from tool or parameter differences can shift results by 10–100%.
> *"All 10 model attempts scored 0.0 despite executing competent, well-reasoned adapter analysis. The rubric fails to distinguish between a technically sound alternative approach and a complete failure."*
> *"The chosen transcript's model answer of 722,494 received a score of 0.0 despite executing a methodologically sound and complete pipeline. The tolerance band of ~3% is too narrow given the unspecified preprocessing pipeline."*

**Theme 2 — Gold answer anchored to a hidden or inaccessible file (~15+ tasks)**
The ground truth references a pre-computed file not present in the task environment, making it unreachable by any sound workflow.
> *"The grading system relies on a single fixed value, but the gold answer cannot be independently verified. The model consistently produced 0.1123, not 0.1639."*
> *"Because the exact parameters and target files for the analysis are inaccessible, the model's valid, logically sound attempt is heavily penalized (scoring 0.0). The grading cannot distinguish between a model that doesn't know how to use deepTools and one that simply could not decipher the hidden parameters."*
> *"The task fails to provide the required input file (h3k4me3_R1.macs2_peaks.narrowPeak) in /tmp/files/, nor does it provide the MACS2 parameters needed to generate the correct answer."*

**Theme 3 — Grading rewards tool guessing over analytical correctness (~10 tasks)**
When the prompt is ambiguous about tools but the rubric is anchored to one tool's output, grading measures tool-guess accuracy rather than pipeline quality.
> *"A model that executes a flawless Clair3 pipeline scores 0.0, while a model that runs Medaka sloppily scores higher. The grading is measuring tool choice more than pipeline quality — the wrong thing to reward when tool choice was never specified."*
> *"The grading fails because it rewards arriving at a MaxBin2-specific answer rather than rewarding genuine task completion and correct methodology."*
> *"The grading punishes good work. A binary rubric with a single correct answer causes the biologically appropriate choice (broad peaks for H3K27me3) to score 0.0, identical to someone who doesn't run MACS2 at all."*

**Theme 4 — Tolerance too permissive, rewards incorrect workflows (~8 tasks)**
Some tolerances are so wide that methodologically wrong approaches fall within the acceptable range, eliminating discriminative power.
> *"The error tolerance for this problem is likely too large — 172% of the gold answer. While the pass rate requires a score over 0.5, this still could result in very large variations being counted as accurate."*
> *"The grading uses a very large absolute error tolerance (10,954.9) relative to the correct value (219,098), allowing substantially incorrect answers derived from an incorrect workflow to receive partial credit."*

**Theme 5 — Binary/string grading is appropriate for categorical answers**
Contrasted against the failures, string-match rubrics are endorsed when the answer is a single unambiguous category (phylum, sample name, species, gene).
> *"A binary yes/no rubric with a case-insensitive string match for 'Ascomycota' is entirely appropriate for a straightforward taxonomic classification question with a single definitive categorical answer."*
> *"A correct workflow reliably produces the correct count; incorrect methods yield different results and are penalized. The tolerance does not meaningfully enable guessing to succeed."*

---

### 2.5 Overall Verdict Patterns

**Accept (57 tasks — 36%):** Deterministic, single-pathway workflow; gold answer verifiable from provided workspace files; tolerance calibrated to stochastic variation only; pass rate evidence confirms calibration.
> *"All dimensions pass. The task represents realistic CUT&RUN analysis work, the question is explicitly clear, and the grading is fair and appropriately calibrated."*
> *"All five dimensions pass. The task is realistic, unambiguous, entry-level in difficulty, has a verifiable ground truth, and is graded fairly."*

**Revise (97 tasks — 61%):** Core biological concept is sound; issues are fixable. The three most frequent revision requests, in order:
1. Name the required tool and specify the critical parameters in the prompt.
2. Re-derive the gold answer from what the provided workspace data actually produces.
3. Add missing intermediate files or reference databases to the environment.

> *"The prompt must specify the expected preprocessing pipeline — at minimum the aligner, the filtering flags, and whether duplicate removal is expected — so the gold answer is reproducible."*
> *"Explicitly specify the tool: add 'Use AdapterRemoval v2.3.2 with default settings for adapter trimming.' Once this is done, reduce the error tolerance to no more than 5%."*

**Reject (6 tasks — 4%):** Gold answer unreachable by any documented workflow AND grading is broken — not fixable without rebuilding from scratch.
> *"The grading system is fundamentally broken because the documented reproducible DESeq2 pipeline yields 1.1929 but the grading reinforces 1.639 as the only correct answer, producing a 0% pass rate and penalizing genuine correct work."*
> *"There is too much uncertainty regarding the pipeline to be utilized. Variations in the tool and reference database used can cause dramatically different answers, making this task impossible to consistently validate as stated."*

---

## 4. Quick Reference: Pass Rates by Dimension

```
Task Realism         ████████████████████░  96.9% Pass
Task Clarity         ███████████░░░░░░░░░░  55.6% Pass
Task Difficulty      (distribution, not pass/fail)
Grading Methodology  ████████████░░░░░░░░░  63.1% Pass
Overall Accept Rate  ███████░░░░░░░░░░░░░░  35.6% Accept
```

**Bottom line:** Biological concepts and professional framing are strong (realism at 97%). The dominant failure mode is **task clarity** — unspecified tools and parameters prevent reproducibility — which then cascades into **grading failures** when gold answers only match one unnamed tool's output. Fixing tool specification and parameter documentation at the prompt-authoring stage will resolve the majority of both Revise verdicts and Grading Fail marks.
