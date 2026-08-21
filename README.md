# Test-Case Generation from UML Artifacts and Environmental Impact Analysis

This repository contains the implementation, generated outputs, prompt templates, evaluation scripts, baseline experiments, and aggregated results for a pipeline that generates test-case specifications from UML-based design artifacts, software requirement reports, and source code.

The repository also contains the evaluation used in the study, including alignment analysis, element-level semantic coverage, additional objective-level coverage metrics, redundancy analysis, direct UML and requirement-based baseline evaluations, and sustainability analysis under Google Colab and fixed-region local-GPU execution settings.

The repository demonstrates the phase-wise workflow using one representative example, **Report 2**. The complete original software project reports and source-code artifacts are not publicly included because of confidentiality restrictions. Representative artifacts, derived outputs, evaluation results, baseline results, and aggregated experimental results are provided to support inspection and reproducibility of the reported findings.

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
- Redundancy analysis for Phase 1, Phase 2, and Phase 3
- Direct UML-to-test-case baseline generation
- Direct UML baseline comparison with the main Phase 1 pipeline
- External requirement-based baseline evaluation
- Original Google Colab sustainability results
- Fixed-region local-GPU sustainability results
- Prompt templates
- Aggregated experimental results

---

# Fine-Tuning and UML Classification

## `Fine_tuning_UML_12Layers.ipynb`

This notebook contains the fine-tuning implementation for the UML/non-UML classifier.

The classifier is based on CLIP with LoRA applied across 12 transformer layers and is used to distinguish UML-containing pages from non-UML pages in heterogeneous software reports.

The classification dataset contains UML diagrams together with non-UML document content such as text-heavy pages, screenshots, tables, layouts, and other report elements.

---

## Fine-Tuned UML Classifier Checkpoint

The fine-tuned classifier checkpoint is not stored directly in this GitHub repository because of its file size.

**Checkpoint file:**

`uml_clip_lora_classifier_N12.pt`

**Google Drive:**

https://drive.google.com/file/d/164RBV0_-bfEueQgSElIWAprOEI4FvzY0/view?usp=sharing

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

This allows the implementation, representative detailed outputs, and final aggregated results to be inspected together.

---

# Additional Coverage Metrics

**File:**

`Additional_Semantic_Coverage.ipynb`

This notebook extends the primary element-level semantic coverage analysis with additional objective-level measures.

The additional evaluation includes the following metrics.

## Co-occurrence-Aware UML Test Objective Coverage

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

These files represent the raw generated outputs from the direct UML-to-test-case baseline.

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

The repository also contains the implementation and generated result archive for the external requirement-to-test-case baseline evaluation.

This baseline evaluates the Phase 2 requirement-based generation approach using external requirement-test relationships.

The same requirement-based generation models used in Phase 2 are evaluated under the baseline setting.

The corresponding baseline notebook contains the implementation, while its ZIP result archive contains the generated outputs and associated evaluation results.

This external evaluation provides an additional reference for requirement-based test-case generation.

The baseline evaluates correspondence with available requirement-test relationships and should not be interpreted as executable correctness, runtime test effectiveness, code coverage, or fault-detection performance.

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

The repository contains sustainability results from two execution settings.

## Original Google Colab Evaluation

The original phase-wise experiments were executed using Google Colab GPU environments.

The corresponding results include operational estimates of:

- Energy consumption
- Carbon emissions
- SCI

recorded during the evaluated inference workloads.

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

# Evidence and Traceability for Reported Findings

The repository is organized so that the empirical findings reported in the manuscript can be traced to the corresponding implementation and result artifacts.

The manuscript remains the primary source for interpretation of the findings, while this repository provides the supporting implementation and experimental evidence.

## UML Extraction

Evidence for UML identification and extraction is provided through:

- `Fine_tuning_UML_12Layers.ipynb`
- `Phase_1_Part_1.ipynb`
- the externally hosted fine-tuned classifier checkpoint
- the representative extracted UML PDF for Report 2

These artifacts show the classification and extraction workflow used before VLM-based UML interpretation.

---

## UML Description and Test-Case Generation

The generation workflow can be traced through:

- `Phase_1_Part_2.ipynb`
- `Phase_1_Part_3.ipynb`
- `Phase_2.ipynb`
- `Phase_3.ipynb`
- `Prompts.pdf`
- representative generated outputs under `Outputs`

Together, these files provide the implementation and representative intermediate artifacts from UML extraction through test-case specification generation.

---

## Alignment Findings

The alignment methodology is implemented in:

`AlignmentScore&SemanticCoverage.ipynb`

The `Outputs/Alignment_Score_Codellama` directory provides a representative detailed example of the resulting alignment calculations.

The same alignment procedure was applied to the outputs associated with the other evaluated code-oriented models, including DeepSeek-Coder and Qwen-Coder.

The corresponding aggregated alignment results are reported in:

`UMLFinalResults.xlsx`

under the **Alignment Score** sheet.

Therefore, the representative CodeLlama folder illustrates the detailed result structure, while the workbook records the broader model-wise results used for the manuscript analysis.

---

## Semantic Coverage Findings

The primary semantic coverage implementation is available in:

`AlignmentScore&SemanticCoverage.ipynb`

Representative detailed semantic coverage outputs are available under:

`Outputs/Semantic Coverage`

The additional objective-level evaluation is provided through:

- `Additional_Semantic_Coverage.ipynb`
- `Additional Semantic Coverage.zip`
- the `Additional Coverage` sheet of `UMLFinalResults.xlsx`

These artifacts provide the supporting calculations and results for the semantic and objective-level coverage findings reported in the manuscript.

---

## Redundancy Findings

The redundancy methodology is implemented in:

`Redundancy Analysis.ipynb`

The corresponding generated evidence is provided independently for all three phases through:

- `PHASE1_DESIGN_REDUNDANCY_RESULT.zip`
- `PHASE2_REQUIREMENT_REDUNDANCY_RESULT.zip`
- `PHASE3_CODE_REDUNDANCY_STRICT_RESULT.zip`

These archives contain the result artifacts used to characterize duplicate patterns across design-based, requirement-based, and code-derived test-case specifications.

---

## Direct UML Baseline Findings

The direct UML baseline can be reproduced and inspected through:

- `Direct_UML_to_Testcase_Baseline.ipynb`
- `Direct_UML_to_Testcase_Baseline_Result.zip`

The comparison with the main Phase 1 pipeline is provided through:

- `Direct_UML_to_Testcase_Baseline_Comparison_Code.ipynb`
- `Direct_UML_to_Testcase_Baseline_Comparison_Result.zip`

Together, these files provide both the generated baseline test cases and the corresponding comparison evidence used in the manuscript.

---

## Requirement Baseline Findings

The requirement-based external baseline is supported by its corresponding baseline notebook and result ZIP archive.

These artifacts provide the generated baseline outputs and evaluation results used to support the external requirement-based comparison reported in the revised study.

---

## Sustainability Findings

The sustainability results used in the manuscript are aggregated in:

`UMLFinalResults.xlsx`

The:

`Sustainability`

sheet contains the original Google Colab results.

The:

`Local GPU`

sheet contains the complementary fixed-region local-GPU results.

These results provide the numerical evidence underlying the sustainability observations discussed in the manuscript.

---

## Interpretation of Repository Evidence

The repository provides evidence for the empirical observations reported in the manuscript by making the following available where possible:

- implementation notebooks
- generation prompts
- representative intermediate outputs
- detailed evaluation outputs
- baseline outputs
- redundancy result archives
- additional coverage outputs
- sustainability results
- aggregated experimental results

Some original software project reports and source-code artifacts cannot be publicly released because of confidentiality restrictions. For these artifacts, representative examples and derived experimental results are provided instead.

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

The complementary local-GPU sustainability experiments were executed separately, and their aggregated results are provided in `UMLFinalResults.xlsx`.

---

# How to Run

## Step 1: Obtain the Classifier Checkpoint

Download:

`uml_clip_lora_classifier_N12.pt`

from the Google Drive link provided above.

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

Do not commit personal Hugging Face access tokens to the repository.

---

## Step 5: Run the Notebook

Run the required notebook cells sequentially.

---

## Step 6: Inspect the Results

Generated outputs are saved to the configured output directories.

The ZIP archives included in the repository provide the corresponding results for the additional evaluations and baseline experiments described above.

---

# Reproducibility and Data Availability

This repository provides:

- phase-wise implementation
- prompt templates
- representative generated artifacts
- alignment and semantic coverage implementation
- additional coverage implementation and results
- redundancy implementation and result archives
- direct UML baseline implementation and outputs
- direct UML baseline comparison implementation and outputs
- requirement-based baseline evidence
- Google Colab sustainability results
- fixed-region local-GPU sustainability results
- aggregated experimental results

The complete original software project reports and source-code repositories are not publicly released because of confidentiality restrictions.

Representative artifacts are provided to illustrate the processing workflow, while derived and aggregated results are included to support inspection of the reported empirical findings.

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

Similarly, the sustainability results represent operational estimates recorded for the evaluated inference workloads and execution environments.

The conclusions in the manuscript are therefore interpreted within the scope of these empirical measures and the evaluated experimental settings.

---

# License

This repository is distributed under the MIT License. See:

`LICENSE`

for details.
