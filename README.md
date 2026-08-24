# Test-Case Generation from UML Artifacts and Environmental Impact Analysis

This repository contains the implementation, generated outputs, prompt templates, evaluation scripts, baseline experiments, and aggregated results for a pipeline that generates test-case specifications from UML-based design artifacts, software requirement reports, and source code.

The repository also contains the evaluation used in the study, including alignment analysis, element-level semantic coverage, additional objective-level coverage metrics, human expert validation, redundancy analysis, direct UML and requirement-based baseline evaluations, and sustainability analysis under Google Colab and fixed-region local-GPU execution settings.

The repository demonstrates the phase-wise workflow using one representative example, **Report 2**. The complete original software project reports and source-code artifacts are not publicly included because of confidentiality restrictions. Representative artifacts, derived outputs, baseline inputs and results, evaluation outputs, raw sustainability examples, and aggregated experimental results are provided to support inspection and reproducibility of the reported findings.

---

# Repository Overview

The repository contains:

- Fine-tuning implementation for UML/non-UML classification
- Phase-wise generation notebooks
- Representative UML extraction and description outputs
- Design-based, requirement-based, and code-derived test-case specifications
- Alignment score implementation and representative alignment outputs
- Element-level semantic coverage evaluation
- Additional objective-level coverage evaluation
- Human expert validation implementation, expert-scored data, and statistical results
- Redundancy analysis for Phase 1, Phase 2, and Phase 3
- Direct UML-to-test-case baseline generation
- Direct UML baseline comparison with the main Phase 1 pipeline
- External requirement-based baseline implementation, benchmark inputs, and results
- Representative raw CodeCarbon outputs from Google Colab and local-GPU settings
- Original Google Colab sustainability results
- Fixed-region local-GPU sustainability results
- Prompt templates
- Aggregated experimental results

---

# Fine-Tuning and UML Classification

## `Fine_tuning_UML.ipynb`

This notebook contains the fine-tuning implementation for the UML/non-UML classifier.

The classifier is based on CLIP with LoRA applied across 12 transformer layers and is used to distinguish UML-containing pages from non-UML pages in heterogeneous software reports.

The classification dataset contains UML diagrams together with non-UML document content such as text-heavy pages, screenshots, tables, layouts, and other report elements.

---

## Fine-Tuned UML Classifier Checkpoint

The current fine-tuning notebook is named:

`Fine_tuning_UML.ipynb`

This replaces the earlier notebook name `Fine_tuning_UML_12Layers.ipynb` used in the previous repository documentation. The fine-tuning configuration itself remains based on CLIP with LoRA applied across 12 transformer layers.

The notebook saves the fine-tuned classifier checkpoint as:

`uml_clip_lora_classifier_N12.pt`

The checkpoint file should be available in the working environment at the path configured in the UML extraction notebook.

The checkpoint is loaded by:

`Phase_1_Part_1.ipynb`

for UML diagram identification and extraction.

---

# Phase-Wise Implementation

## Phase 1 Part 1: UML Diagram Extraction

**File:**

`Phase_1_Part_1.ipynb`

This notebook identifies and extracts UML-containing pages from mixed-content software design reports.

The notebook loads the fine-tuned UML/non-UML classifier and applies it to report pages containing heterogeneous content.

### Representative Output

`Report_2_uml_pages (uml diagrams).pdf`

This file contains the UML pages extracted from the representative software design report.

**Note:** Phase 1 Part 1 is implemented programmatically using the fine-tuned classifier and does not use a natural-language prompt.

---

# Phase 1 Part 2: UML Description Generation

**File:**

`Phase_1_Part_2.ipynb`

This notebook processes the UML diagram pages extracted during Phase 1 Part 1 and generates structured textual UML descriptions using different Vision-Language Models (VLMs).

The generated descriptions capture information such as:

- UML type
- Components
- Actors and lifelines
- Relationships and links
- Main behavioral flows
- Conditions and branches
- Loops
- Notes and annotations
- Overall observations derived from visible UML information

### Representative Output

`Report_2_uml_pages (uml description).txt`

This file contains UML-derived textual descriptions generated from the extracted UML diagrams.

---

# Phase 1 Part 3: Design-Based Test-Case Generation

**File:**

`Phase_1_Part_3.ipynb`

This notebook generates design-based test-case specifications from the UML descriptions produced during Phase 1 Part 2.

Different Large Language Models (LLMs) are used to generate structured test cases containing fields such as:

- Test Case ID
- Title
- Module
- Source UML Pages
- Preconditions
- Test Data
- Test Steps
- Expected Result

### Representative Output

`Report_2_uml_pages_design_testcases.csv`

---

# Phase 2: Requirement-Based Test-Case Generation

**File:**

`Phase_2.ipynb`

This notebook generates requirement-based test-case specifications from software requirement reports.

Generation is restricted to information explicitly available in the requirement documents. Document-specific content such as cover pages, tables of contents, references, formatting, and section numbering is excluded from test-case generation.

### Representative Output

`Report_2_Req_testcases.csv`

---

# Phase 3: Code-Based Test-Case Generation

**File:**

`Phase_3.ipynb`

This notebook generates code-derived test-case specifications from available source-code context using code-oriented language models.

The implementation analyzes available code structure, methods, branches, user-interface signals, and backend behavior to derive corresponding test-case specifications.

### Representative Output

`Code File 2 testcases using qwen coder.csv`

The complete original source-code repositories are not publicly provided because of confidentiality restrictions.

---

# Metric Terminology Note

The terminology used in some repository notebooks and result files reflects the experimental implementation and may differ from the notation adopted in the revised manuscript. The following mapping should be used when interpreting the repository results:

- **Semantic Coverage / ELSC** → **Element-Level Semantic Coverage (α)**
- **Alignment Score** → **Alignment Score (β)**
- **COC** → **Co-occurrence-Aware Test Objective Coverage (γ)**
- **CBTC** → **Condition-Bound Transition Coverage (δ)**
- **ERPC / Oracle-Level Coverage** → **Expected-Result Presence Coverage (ε)**

In the original study, the primary measures were referred to as **Semantic Coverage** and **Alignment Score**. During manuscript revision, Semantic Coverage was refined as Element-Level Semantic Coverage and represented by **α**, while Alignment Score is represented by **β**. The complementary objective-level measures represented by **γ**, **δ**, and **ε** were introduced during the revised evaluation.

Some repository notebooks, spreadsheets, and generated result files retain abbreviations or original column names to preserve the experimental artifacts in their generated form. The notation **α, β, γ, δ, and ε** used in the revised manuscript should therefore be considered the corresponding manuscript terminology when interpreting these repository results.

---

# Alignment Score and Element-Level Semantic Coverage

**File:**

`AlignmentScore&SemanticCoverage.ipynb`

This notebook performs the primary cross-artifact semantic evaluation.

It computes:

- Design-to-code alignment
- Requirement-to-code alignment
- Element-level semantic coverage between UML-derived information and design-based test cases

The alignment score evaluates semantic correspondence between test-case specifications generated from different software artifacts.

The semantic coverage analysis evaluates the preservation of UML-derived information in the generated design-based test cases.

The evaluation uses:

`BAAI/bge-large-en-v1.5`

as the embedding model.

These measures represent semantic correspondence and information preservation. They should not be interpreted as executable test correctness, runtime validation, code coverage, mutation score, or fault-detection effectiveness.

A T4 GPU in Google Colab was used for this evaluation.

---

# Alignment Score Result Evidence

Detailed alignment outputs are provided in the `Outputs` directory.

The repository includes:

`Outputs/Alignment_Score_Codellama`

as a representative detailed alignment-result structure.

The same alignment-scoring implementation in:

`AlignmentScore&SemanticCoverage.ipynb`

was applied to the corresponding outputs generated using the other evaluated code-oriented models, including DeepSeek-Coder and Qwen-Coder.

Therefore, the CodeLlama folder is provided as a representative detailed example of how the alignment analysis is organized; it does not indicate that alignment was evaluated only for CodeLlama.

The aggregated alignment results across the evaluated model combinations are retained in:

`UMLFinalResults.xlsx`

under the **Alignment Score** sheet.

This allows the evaluation implementation, representative detailed outputs, and aggregated model-wise results to be inspected together.

---

# Additional Coverage Metrics

**File:**

`Additional_Semantic_Coverage.ipynb`

This notebook extends the primary element-level semantic coverage analysis with additional objective-level measures.

The additional evaluation includes the following metrics.

## Co-occurrence-Aware Test Objective Coverage

This metric evaluates whether semantically related UML information forming a test objective is jointly represented in generated design-based test cases.

It provides a stricter objective-level perspective than independent element matching.

## Condition-Bound Transition Coverage

This metric focuses specifically on UML objectives involving conditions, guards, branches, and their corresponding transitions or outcomes.

## Expected-Result Presence Coverage

This metric measures whether generated test cases contain a meaningful expected-result specification.

It evaluates the presence of expected-result information and does not establish that the expected result constitutes a correct executable oracle.

---

# Additional Coverage Results

**Result archive:**

`Additional Semantic Coverage.zip`

The archive contains detailed additional-coverage results corresponding to the uploaded semantic-coverage configuration.

The repository provides these detailed results for the **Qwen2.5-7B-based test-case generation configuration**, consistent with the Qwen-based semantic coverage results already available under:

`Outputs/Semantic Coverage`

The notebook additionally computes threshold-based embedding results using:

- 0.65
- 0.70
- 0.75

These threshold-specific outputs are retained in the repository for transparency and reproducibility.

The primary manuscript reports the overall mean coverage scores used in the main analysis rather than separately reporting each threshold-specific result.

---

# Terminology Note for Expected-Result Presence Coverage

The original experimental implementation and the corresponding generated result files use the term:

`Oracle-Level Coverage`

In the revised manuscript, this metric is referred to as:

**Expected-Result Presence Coverage (ERPC)**

The terminology was revised because the implementation determines whether a meaningful expected-result field is present in a generated test case.

It does **not** determine whether the expected result is a correct executable oracle.

Therefore:

`Oracle-Level Coverage` in the original repository outputs  
=  
`Expected-Result Presence Coverage (ERPC)` in the revised manuscript.

This is a terminology clarification only.

The underlying implementation, calculations, and numerical results have not been changed. The original experimental files are intentionally retained in their generated form for reproducibility.

---

# Human Expert Validation

The repository includes the human expert validation used to provide an independent assessment of the automated coverage measures. The human evaluation involved a pool of eight software engineering experts, consisting of three senior faculty members, three postdoctoral researchers, and two PhD students. Each evaluated output was assessed independently by two experts, and the mean of the two ratings was used as the human reference score for comparison with the automated coverage measures.

## Validation and Analysis Notebook

**File:**

`HumanExpertValidation.ipynb`

This notebook supports two related parts of the human validation workflow.

First, it prepares the automated coverage information required for expert assessment by reading the existing semantic-coverage and additional objective-level coverage outputs without recomputing or modifying the original experimental metric values. The dataset-building stage covers the four design-based test-case generation LLM configurations, five UML-description VLM configurations, and 29 reports used in the final cross-artifact evaluation, corresponding to 580 possible LLM-VLM-report combinations. The completed expert-scored workbook described below contains the 29 report-level cases used for the final human validation analysis.

Second, it performs the statistical comparison between the automated coverage values and the completed human expert ratings. The human reference value for each evaluated measure is calculated as the mean of the two expert ratings assigned to that evaluated output.

The analysis covers:

- Element/Concept Coverage
- Flow Coverage
- Conditional/Validation Coverage
- Element-Level Semantic Coverage, reported as α in the revised manuscript
- Co-occurrence-Aware Test Objective Coverage, reported as γ in the revised manuscript
- Condition-Bound Transition Coverage, reported as δ in the revised manuscript
- Expected-Result Presence Coverage, reported as ε in the revised manuscript

For each measure, the notebook calculates the number of applicable observations, the mean automated score, the mean human score, Spearman's rank correlation coefficient, the corresponding p-value, and quadratically weighted Cohen's kappa for the paired expert ratings. Rows for which condition-bound behavior is not applicable are excluded from the corresponding CBTC analysis rather than being assigned a score of zero.

---

## Final Human Expert Validation Data

**File:**

`Human_Expert_Validation_Final.xlsx`

This workbook contains the completed report-level human validation data used for the final analysis.

The primary sheet, `Human_Expert_Validation_29`, contains the 29 evaluated report-level cases together with:

- report identifier
- associated LLM and VLM configuration
- UML-description and generated-test-case file references
- automated coverage values
- Human Expert 1 ratings
- Human Expert 2 ratings
- mean human ratings
- expected-result presence information
- expert reasoning and observed rating differences

The workbook also contains `Expert_Entry_Template_29`, which preserves the expert-entry structure used for the validation process.

The human ratings use a 0 to 1 ordinal scale represented through the values 0.00, 0.25, 0.50, 0.75, and 1.00. The two expert ratings are retained separately so that both the human reference mean and inter-rater agreement can be examined.

---

## Human Expert Validation Statistical Results

**File:**

`Human_Expert_Validation_Final_Statistics (1).xlsx`

This workbook contains the statistical outputs generated from the completed human validation data.

It contains three sheets:

### `Final_Results`

Provides the compact summary used to inspect the final human-validation results, including:

- number of applicable observations (`n`)
- mean automated score
- mean human score
- Spearman's ρ
- p-value
- quadratically weighted Cohen's kappa

### `Full_Precision`

Retains the same statistical results at full numerical precision for reproducibility and verification.

### `Validation_Data`

Contains the report-level values used in the statistical calculations, including the automated score, both expert ratings, and the resulting human mean for each evaluated metric.

The human expert validation is intended to assess how the automated UML-information-preservation measures correspond with expert judgments. It does not convert the generated test-case specifications into executable tests and should not be interpreted as execution-based validation, code coverage, mutation testing, or fault-detection evaluation.

---

# Redundancy Analysis

**File:**

`Redundancy Analysis.ipynb`

This notebook evaluates redundancy across the generated test-case specifications.

The analysis identifies normalized exact duplicates after removing non-substantive differences such as test-case identifiers, numbering, punctuation, and whitespace while preserving substantive test-case content.

Redundancy is evaluated independently for each generation phase.

---

## Phase 1: Design-Based Test Cases

Design-based test-case specifications generated from UML-derived descriptions are evaluated for duplicate content.

**Result archive:**

`PHASE1_DESIGN_REDUNDANCY_RESULT.zip`

---

## Phase 2: Requirement-Based Test Cases

Requirement-based test-case specifications generated from software requirement reports are independently evaluated for redundancy.

**Result archive:**

`PHASE2_REQUIREMENT_REDUNDANCY_RESULT.zip`

---

## Phase 3: Code-Derived Test Cases

Code-derived test-case specifications are evaluated using the same strict normalized-duplicate criterion.

**Result archive:**

`PHASE3_CODE_REDUNDANCY_STRICT_RESULT.zip`

The redundancy analysis characterizes repetition within the generated outputs and should not be interpreted as executable test validity or fault-detection capability.

---

# Direct UML-to-Test-Case Baseline

The direct UML baseline evaluates whether test-case specifications can be generated directly from UML diagram images without first generating the intermediate textual UML description used in the main Phase 1 pipeline.

## Baseline Generation Code

**File:**

`Direct_UML_to_Testcase_Baseline.ipynb`

The baseline follows:

`UML image → test-case generation`

whereas the main Phase 1 pipeline follows:

`UML image → structured UML description → test-case generation`

The direct baseline therefore provides a comparison under the same general visual-UML input setting while removing the intermediate UML-description stage.

---

## Direct UML Baseline Generation Results

**Result archive:**

`Direct_UML_to_Testcase_Baseline_Result.zip`

This archive contains the test-case specifications generated directly from UML diagram images using the evaluated VLM configurations.

These files represent the generated outputs from the direct UML-to-test-case baseline.

---

# Direct UML Baseline Comparison

**File:**

`Direct_UML_to_Testcase_Baseline_Comparison_Code.ipynb`

This notebook compares the direct UML-to-test-case baseline outputs with the existing Phase 1 pipeline.

The comparison evaluates UML-information preservation across dimensions including:

- UML page coverage
- Components and actors
- Relationships and messages
- Main behavioral flows
- Conditions and branches
- Overall structural coverage
- Redundancy

**Result archive:**

`Direct_UML_to_Testcase_Baseline_Comparison_Result.zip`

This archive contains the detailed comparison results produced by the baseline-comparison notebook.

The baseline is intended to evaluate the contribution of the intermediate UML-description stage within the visual UML setting used in this study.

It is not intended to establish superiority over traditional model-based UML test-generation approaches that operate on formal or machine-readable UML representations.

---

# Requirement-Based External Baseline

The repository contains an external requirement-to-test-case baseline used to provide an additional evaluation of the Phase 2 requirement-based generation pipeline.

## Baseline Implementation

**File:**

`Extenal_Requirement_Baseline.ipynb`

The notebook prepares independent requirement inputs, generates test-case specifications using the same family of LLM configurations evaluated in Phase 2, and compares the generated outputs with held-out reference test information.

Reference test cases are excluded from the generation input and are used only during the subsequent matching/evaluation stage.

---

## External Baseline Results

**Result archive:**

`external_baseline_modelwise_results 2.zip`

The archive contains the benchmark artifacts, model-wise generation/evaluation outputs, and combined analysis used for the external requirement baseline.

The archive contains three main components:

### `benchmark-inputs`

This directory contains the requirement artifacts used as input to the external requirement-based generation experiment.

For the paired requirement-test benchmark, two separated 30-item subsets were used, covering the first/top 30 and the last/bottom 30 requirement instances from the available benchmark data.

The corresponding reference test information was kept separate from the requirement input during generation.

The directory also contains the requirement PDF prepared for the **EBT** traceability benchmark.

For EBT, the requirement artifact provided to the LLM contains only the linked requirement information. The associated reference test cases were retained separately and were not provided during generation.

This organization ensures that the generation models receive requirement information only, while the reference test data remain held out for evaluation.

### `matching-results`

This directory contains the matching/evaluation outputs obtained by comparing the generated requirement-based test cases with the corresponding held-out benchmark references.

The matching results provide the model-wise evidence used to assess whether the generated specifications correspond to the available reference test intentions or traceability links.

### `combined-analysis`

This directory contains the combined model-wise analysis derived from the matching results.

The combined files summarize the external baseline results across the evaluated requirement-based generation models and provide the values used for the baseline comparison reported in the revised manuscript.

The external baseline assesses correspondence with available benchmark requirement-test relationships. It should not be interpreted as executable correctness, runtime effectiveness, code coverage, mutation performance, or fault-detection capability.

---

# Outputs Folder

The `Outputs` folder contains representative generated artifacts and evaluation outputs for **Report 2**.

The representative outputs make it possible to inspect the phase-wise processing workflow without publicly releasing the complete confidential project-report collection.

---

# Representative UML and Test-Case Outputs

## `Report_2_uml_pages (uml diagrams).pdf`

Contains UML pages extracted during Phase 1 Part 1.

## `Report_2_uml_pages (uml description).txt`

Contains structured UML descriptions produced during Phase 1 Part 2.

## `Report_2_uml_pages_design_testcases.csv`

Contains design-based test-case specifications produced during Phase 1 Part 3.

## `Report_2_Req_testcases.csv`

Contains requirement-based test-case specifications produced during Phase 2.

---

# Alignment Score Output

**Folder:**

`Outputs/Alignment_Score_Codellama`

The folder contains:

- `Set 1`
- `Set 2`
- `Set 3`
- `Set 4`

Each set corresponds to a different requirement-based test-case generation LLM configuration.

Within each set, results are organized according to UML-derived descriptions produced by the evaluated VLM configurations.

This folder is provided as a representative detailed CodeLlama alignment result.

The same alignment implementation was applied to the other evaluated code-model configurations, while their aggregated results are provided in `UMLFinalResults.xlsx`.

---

# Semantic Coverage Output

**Folder:**

`Outputs/Semantic Coverage`

This folder contains representative semantic coverage outputs.

The `qwen 7b` folder contains semantic coverage results obtained using UML-derived descriptions generated by different VLM configurations, including:

- Gemma-3-4B
- Gemma-3-12B
- Llama
- LLaVA
- Qwen

The `Additional Semantic Coverage.zip` archive complements these outputs with the additional objective-level coverage metrics.

---

# Final Experimental Results

**File:**

`UMLFinalResults.xlsx`

This workbook contains the aggregated experimental results used in the study.

The updated workbook contains **five sheets**.

---

## 1. Alignment Score

Contains aggregated semantic alignment results between:

- Design-based and code-derived test-case specifications
- Requirement-based and code-derived test-case specifications

The results cover the evaluated VLM, general-purpose LLM, and code-model combinations.

---

## 2. Semantic Coverage

Contains the primary element-level semantic coverage results across the evaluated VLM and LLM configurations.

---

## 3. Sustainability

Contains the original sustainability measurements collected during the Google Colab experiments, including:

- Energy consumption
- Carbon emissions
- SCI values
- Execution-related sustainability measurements

---

## 4. Additional Coverage

Contains the additional objective-level coverage results, including:

- Co-occurrence-Aware UML Test Objective Coverage
- Condition-Bound Transition Coverage
- Expected-Result Presence Coverage

Where the original experimental files use the name `Oracle-Level Coverage`, this corresponds to Expected-Result Presence Coverage in the revised manuscript, as described in the terminology note above.

---

## 5. Local GPU

Contains the complementary fixed-region local-GPU sustainability results.

These experiments were conducted using a common Alberta regional grid-intensity context across the evaluated configurations.

The local-GPU results provide an additional sustainability perspective alongside the original Google Colab experiments.

The Google Colab and local-GPU environments differ in hardware and execution environment. Consequently, observed differences between these settings should not be attributed solely to geographic region or grid carbon intensity.

---

# Prompt Templates

**File:**

`Prompts.pdf`

The updated prompt document contains the prompt templates used across the prompt-driven stages of the pipeline.

It includes prompts for:

- Phase 1 Part 2: UML description generation
- Phase 1 Part 3: Design-based test-case generation
- Phase 2: Requirement-based test-case generation
- Phase 3: Code-based test-case generation
- Direct UML-to-test-case baseline

Phase 1 Part 1 is not included because UML extraction is implemented programmatically using the fine-tuned classifier and does not use a natural-language prompt.

---

# Sustainability Evaluation

The repository contains sustainability evidence from two execution settings.

## Original Google Colab Evaluation

The original phase-wise experiments were executed using Google Colab GPU environments.

The corresponding aggregated results include operational estimates of:

- Energy consumption
- Carbon emissions
- SCI

recorded during the evaluated inference workloads.

The aggregated results are available in the:

`Sustainability`

sheet of:

`UMLFinalResults.xlsx`

---

## Fixed-Region Local-GPU Evaluation

A complementary local-GPU evaluation was conducted using a common Alberta regional grid context.

The aggregated results are available in the:

`Local GPU`

sheet of:

`UMLFinalResults.xlsx`

The purpose of this additional evaluation is to provide a common regional context while retaining the original multi-region Google Colab results.

Because the two settings differ in hardware, runtime environment, and other execution characteristics, the repository does not treat cross-setting differences as effects attributable to a single factor.

---

# Representative Raw CodeCarbon Outputs

Two original CodeCarbon-generated CSV files are included to provide examples of the execution-level sustainability records produced under the two experimental environments.

## Google Colab Example

**File:**

`phase1_part3_run_1_inference_emissions_Colab.csv`

This file contains the CodeCarbon output from a representative **Phase 1 Part 3, Run 1** inference execution conducted in the Google Colab environment.

---

## Local-GPU Example

**File:**

`phase1_part3_run_1_inference_emissions_local_gpu.csv`

This file contains the corresponding CodeCarbon output from a representative **Phase 1 Part 3, Run 1** inference execution conducted in the local-GPU environment.

These CSV files are provided as representative raw measurement outputs to illustrate the form of the execution-level CodeCarbon records from each experimental setting.

They are **not intended to represent the complete set of raw CodeCarbon files for all phases, models, and runs**.

The complete aggregated sustainability values used in the manuscript are reported in:

`UMLFinalResults.xlsx`

through the:

- `Sustainability` sheet for the original Google Colab experiments
- `Local GPU` sheet for the complementary local-GPU experiments

The representative raw CSV files therefore provide additional traceability between the CodeCarbon measurement process and the aggregated sustainability results reported in the study.

---

# Evidence and Traceability for Reported Findings

The repository is organized so that the empirical findings reported in the manuscript can be traced to the corresponding implementation and result artifacts.

The manuscript remains the primary source for interpretation of the findings, while this repository provides supporting implementation, representative artifacts, baseline evidence, and experimental results.

---

## UML Extraction Evidence

Evidence for UML identification and extraction is provided through:

- `Fine_tuning_UML.ipynb`
- `Phase_1_Part_1.ipynb`
- the fine-tuned classifier checkpoint `uml_clip_lora_classifier_N12.pt`
- the representative extracted UML PDF for Report 2

These artifacts show the classification and extraction workflow used before VLM-based UML interpretation.

---

## UML Description and Test-Case Generation Evidence

The generation workflow can be traced through:

- `Phase_1_Part_2.ipynb`
- `Phase_1_Part_3.ipynb`
- `Phase_2.ipynb`
- `Phase_3.ipynb`
- `Prompts.pdf`
- representative generated outputs under `Outputs`

Together, these files provide the implementation and representative intermediate artifacts from UML extraction through test-case specification generation.

---

## Alignment Evidence

The alignment methodology is implemented in:

`AlignmentScore&SemanticCoverage.ipynb`

The:

`Outputs/Alignment_Score_Codellama`

directory provides a representative detailed example of the alignment calculations and result organization.

The same alignment procedure was applied to the corresponding outputs associated with the other evaluated code-oriented models, including DeepSeek-Coder and Qwen-Coder.

The aggregated model-wise alignment results are reported in:

`UMLFinalResults.xlsx`

under the:

`Alignment Score`

sheet.

The representative CodeLlama folder therefore illustrates the detailed result structure, while the workbook provides the broader model-wise results used for the manuscript analysis.

---

## Semantic Coverage Evidence

The primary element-level semantic coverage implementation is available in:

`AlignmentScore&SemanticCoverage.ipynb`

Representative detailed semantic coverage outputs are available under:

`Outputs/Semantic Coverage`

The additional objective-level evaluation is provided through:

- `Additional_Semantic_Coverage.ipynb`
- `Additional Semantic Coverage.zip`
- the `Additional Coverage` sheet of `UMLFinalResults.xlsx`

These artifacts provide the supporting calculations and results for the semantic and objective-level coverage observations reported in the manuscript.

---

## Human Expert Validation Evidence

The human expert validation can be inspected through:

- `HumanExpertValidation.ipynb`
- `Human_Expert_Validation_Final.xlsx`
- `Human_Expert_Validation_Final_Statistics (1).xlsx`

The notebook provides the data-preparation and statistical-analysis workflow. The final validation workbook retains the automated measures together with the two expert ratings and report-level reasoning, while the statistics workbook provides the compact summary, full-precision results, and report-level values used in the final calculations.

These files provide traceability from the automated coverage values and expert assessments to the statistical human-validation results reported in the revised study.

---

## Redundancy Evidence

The redundancy methodology is implemented in:

`Redundancy Analysis.ipynb`

The corresponding generated results are provided independently for all three phases through:

- `PHASE1_DESIGN_REDUNDANCY_RESULT.zip`
- `PHASE2_REQUIREMENT_REDUNDANCY_RESULT.zip`
- `PHASE3_CODE_REDUNDANCY_STRICT_RESULT.zip`

These archives contain the result artifacts used to characterize duplicate patterns across design-based, requirement-based, and code-derived test-case specifications.

---

## Direct UML Baseline Evidence

The direct UML baseline can be reproduced and inspected through:

- `Direct_UML_to_Testcase_Baseline.ipynb`
- `Direct_UML_to_Testcase_Baseline_Result.zip`

The comparison with the main Phase 1 pipeline is provided through:

- `Direct_UML_to_Testcase_Baseline_Comparison_Code.ipynb`
- `Direct_UML_to_Testcase_Baseline_Comparison_Result.zip`

Together, these files provide both the generated direct-UML baseline outputs and the corresponding comparison evidence used in the revised manuscript.

---

## External Requirement Baseline Evidence

The external requirement-based baseline is supported by:

- `Extenal_Requirement_Baseline.ipynb`
- `external_baseline_modelwise_results 2.zip`

Within the result archive:

- `benchmark-inputs` contains the requirement artifacts used as model inputs and the separated benchmark reference material
- `matching-results` contains the model-wise matching/evaluation outputs
- `combined-analysis` contains the combined result analysis across the evaluated models

The benchmark construction keeps requirement input and held-out reference test information separated during generation.

For the paired requirement-test benchmark, the evaluation includes the first/top 30 and last/bottom 30 requirement instances used in the external validation.

The EBT requirement PDF is also included in the benchmark inputs, while the associated reference test information remains held out from the LLM during generation.

These files provide the trace from external benchmark input preparation through generated test cases, matching, and final model-wise analysis.

---

## Sustainability Evidence

The sustainability observations can be traced through three levels of repository evidence.

### Representative Raw Measurement Outputs

- `phase1_part3_run_1_inference_emissions_Colab.csv`
- `phase1_part3_run_1_inference_emissions_local_gpu.csv`

These files provide representative CodeCarbon-generated execution records from the Google Colab and local-GPU environments.

### Aggregated Google Colab Results

The:

`Sustainability`

sheet of:

`UMLFinalResults.xlsx`

contains the aggregated sustainability values from the original Google Colab experiments.

### Aggregated Local-GPU Results

The:

`Local GPU`

sheet of:

`UMLFinalResults.xlsx`

contains the aggregated values from the complementary fixed-region local-GPU evaluation.

The representative CSV files illustrate the underlying CodeCarbon output structure, while the workbook contains the broader values used for the manuscript analysis.

---

## Interpretation of Repository Evidence

The repository provides supporting evidence for the empirical observations reported in the manuscript by making the following available where possible:

- implementation notebooks
- generation prompts
- representative intermediate outputs
- detailed alignment outputs
- semantic coverage outputs
- additional coverage results
- human expert validation data and statistical results
- redundancy result archives
- direct UML baseline outputs
- direct UML baseline comparison results
- external requirement benchmark inputs and evaluation results
- representative raw CodeCarbon outputs
- aggregated sustainability results
- final experimental result workbook

Some original software project reports and source-code artifacts cannot be publicly released because of confidentiality restrictions.

For these artifacts, representative examples, benchmark inputs where publicly distributable, and derived experimental results are provided instead.

The repository therefore supports traceability between the reported empirical observations and the corresponding experimental artifacts without making claims beyond the scope of the available evaluation.

---

# Execution Environment

The phase-wise generation notebooks were designed primarily for Google Colab.

## A100 GPU

An A100 GPU was used for computationally intensive generation stages such as:

- UML classification and extraction
- VLM-based UML description generation
- Design-based test-case generation
- Requirement-based test-case generation
- Code-based test-case generation

## T4 GPU

A T4 GPU was used for evaluation stages including:

- Alignment analysis
- Element-level semantic coverage
- Additional objective-level coverage analysis

The human expert validation notebook uses the previously generated coverage results together with the expert-provided ratings. Its statistical analysis is based on the completed validation workbook and does not require model inference.

The complementary local-GPU sustainability experiments were executed separately, and their aggregated results are provided in `UMLFinalResults.xlsx`.

---

# How to Run

## Step 1: Prepare the Classifier Checkpoint

Ensure that the fine-tuned classifier checkpoint:

`uml_clip_lora_classifier_N12.pt`

is available in the working environment at the path configured in `Phase_1_Part_1.ipynb`.

The current fine-tuning implementation used to produce this checkpoint is provided in:

`Fine_tuning_UML.ipynb`

---

## Step 2: Open the Required Notebook

Open the required `.ipynb` notebook in Google Colab.

---

## Step 3: Select the Appropriate Runtime

Select the GPU runtime appropriate for the experiment being reproduced.

---

## Step 4: Update Configuration Fields

Where required, update:

- Input paths
- Output paths
- Model name
- Hugging Face authentication
- human expert validation workbook paths when running `HumanExpertValidation.ipynb`

Do not commit personal Hugging Face access tokens to the repository.

---

## Step 5: Run the Notebook

Run the required notebook cells sequentially.

---

## Step 6: Inspect the Results

Generated outputs are saved to the configured output directories.

The ZIP archives included in the repository provide corresponding detailed results for the additional evaluations and baseline experiments described above.

---

# Reproducibility and Data Availability

This repository provides:

- phase-wise implementation
- prompt templates
- representative generated artifacts
- alignment and semantic coverage implementation
- additional coverage implementation and results
- human expert validation notebook, completed validation data, and statistical results
- redundancy implementation and result archives
- direct UML baseline implementation and outputs
- direct UML baseline comparison implementation and outputs
- external requirement baseline implementation
- external requirement benchmark inputs and evaluation results
- representative raw Google Colab CodeCarbon output
- representative raw local-GPU CodeCarbon output
- aggregated Google Colab sustainability results
- aggregated fixed-region local-GPU sustainability results
- final experimental result workbook

The complete original software project reports and source-code repositories are not publicly released because of confidentiality restrictions.

Representative artifacts are provided to illustrate the processing workflow, while derived, baseline, and aggregated results are included to support inspection of the reported empirical findings.

---

# Important Interpretation Note

The generated outputs in this repository are **test-case specifications** derived from design artifacts, requirement information, and source-code context.

The reported alignment and semantic coverage metrics evaluate semantic correspondence and preservation of software information across artifacts.

They do not establish:

- compilation success
- runtime pass/fail behavior
- executable code coverage
- mutation score
- fault-detection effectiveness
- executable oracle correctness

Expected-Result Presence Coverage evaluates whether meaningful expected-result information is present and should not be interpreted as validation of the correctness of that expected result.

The external requirement baseline evaluates correspondence with held-out benchmark requirement-test relationships rather than executable test effectiveness.

The human expert validation provides an independent comparison between the automated coverage measures and expert judgments of UML-information preservation. It does not constitute execution-based validation of the generated test-case specifications.

Similarly, the sustainability results represent operational estimates recorded for the evaluated inference workloads and execution environments.

The two uploaded CodeCarbon CSV files are representative execution-level examples and should not be interpreted as the complete raw measurement collection for all experimental phases.

The conclusions in the manuscript are therefore interpreted within the scope of these empirical measures and the evaluated experimental settings.

---

# License

This repository is distributed under the MIT License. See:

`LICENSE`

for details.
