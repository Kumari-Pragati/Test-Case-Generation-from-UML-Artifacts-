# Test-Case Generation from UML Artifacts and Environmental Impact Analysis

This repository contains the implementation, generated outputs, prompt templates, evaluation scripts, baseline experiments, and final aggregated results for a pipeline that generates test-case specifications from UML-based design artifacts, software requirement reports, and source code.

The repository also contains the evaluation used in the study, including alignment analysis, semantic coverage, additional objective-level coverage metrics, redundancy analysis, baseline comparisons, and sustainability analysis under Google Colab and fixed-region local-GPU execution settings.

The repository demonstrates the phase-wise workflow using one representative example, **Report 2**. The complete original software project reports and source-code artifacts are not publicly included because of confidentiality restrictions. Aggregated and derived experimental results required to support the reported analysis are included where applicable.

---

# Repository Overview

The repository contains:

- Fine-tuning and UML classification implementation
- Phase-wise generation notebooks
- Generated UML descriptions and test-case specifications
- Alignment score and semantic coverage evaluation
- Additional coverage metric evaluation
- Redundancy analysis for all three test-case generation phases
- Direct UML-to-test-case baseline and comparison results
- Requirement-based baseline evaluation
- Sustainability evaluation results
- Fixed-region local-GPU sustainability results
- Prompt templates used in the generation stages
- Final aggregated experimental results

---

# Fine-Tuning and UML Classification

## `Fine_tuning_UML_12Layers.ipynb`

This notebook contains the fine-tuning implementation for the UML/non-UML classifier.

The classifier is based on CLIP with LoRA applied across 12 transformer layers and is used to distinguish UML-containing pages from non-UML pages in heterogeneous software reports.

The dataset includes UML diagrams together with non-UML report content such as text-heavy pages, screenshots, tables, layouts, and other document elements.

---

## Fine-Tuned UML Classifier Checkpoint

The fine-tuned classifier checkpoint is not stored directly in this GitHub repository because of its file size.

**Checkpoint file:**

`uml_clip_lora_classifier_N12.pt`

**Google Drive:**

[Download the fine-tuned UML classifier checkpoint](PASTE_GOOGLE_DRIVE_LINK_HERE)

The checkpoint is used by:

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

This file contains only the UML pages extracted from the representative software design report.

**Note:** Phase 1 Part 1 is implemented programmatically and does not use a natural-language prompt.

---

# Phase 1 Part 2: UML Description Generation

**File:**

`Phase_1_Part_2.ipynb`

This notebook processes the UML diagram pages extracted in Phase 1 Part 1 and generates structured textual UML descriptions using different Vision-Language Models (VLMs).

The generated descriptions capture information such as UML type, components, actors or lifelines, relationships, main flows, conditions, branches, loops, and visible annotations.

### Representative Output

`Report_2_uml_pages (uml description).txt`

This file contains the UML-derived textual descriptions generated from the extracted UML diagrams.

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

The generation is restricted to information explicitly available in the requirement documents. Document-layout information such as cover pages, tables of contents, references, and formatting is excluded from test-case generation.

### Representative Output

`Report_2_Req_testcases.csv`

---

# Phase 3: Code-Based Test-Case Generation

**File:**

`Phase_3.ipynb`

This notebook generates code-derived test-case specifications from available source-code context using code-oriented language models.

The implementation identifies methods and relevant user-interface or interaction signals and generates test cases grounded in the available code structure, branches, methods, and backend behavior.

### Representative Output

`Code File 2 testcases using qwen coder.csv`

The complete original source-code repositories are not publicly provided because of confidentiality restrictions.

---

# Alignment Score and Semantic Coverage

**File:**

`AlignmentScore&SemanticCoverage.ipynb`

This notebook performs the primary cross-artifact semantic evaluation.

It computes:

- Design-to-code alignment
- Requirement-to-code alignment
- Element-level semantic coverage between UML-derived information and design-based test cases

The alignment measures semantic correspondence between generated test-case specifications derived from different software artifacts. These measures should not be interpreted as executable test correctness, runtime validation, code coverage, or fault-detection capability.

The evaluation uses the `BAAI/bge-large-en-v1.5` embedding model.

A T4 GPU in Google Colab was used for this evaluation.

---

# Additional Coverage Metrics

**File:**

`Additional_Semantic_Coverage.ipynb`

This notebook provides additional objective-level coverage analysis beyond the element-level semantic coverage reported by the primary semantic coverage implementation.

The additional evaluation includes:

### Co-occurrence-Aware UML Test Objective Coverage

This metric evaluates whether semantically related UML information that forms a test objective is jointly represented in generated design-based test cases.

### Condition-Bound Transition Coverage

This metric focuses on UML objectives involving conditions, branches, guards, and corresponding transitions or outcomes.

### Expected-Result Presence Coverage

This metric measures whether generated test cases contain a meaningful expected-result specification.

---

## Additional Coverage Results

**File:**

`Additional Semantic Coverage.zip`

The ZIP archive contains the generated additional coverage results for the uploaded semantic-coverage configuration.

The repository currently provides the corresponding detailed results for the **Qwen2.5-7B-based test-case generation configuration**, consistent with the Qwen-based semantic coverage output already provided in the `Outputs/Semantic Coverage` folder.

The notebook also computes threshold-specific embedding results at:

- 0.65
- 0.70
- 0.75

These threshold-specific outputs are retained in the repository for transparency and reproducibility.

The primary manuscript reports the overall mean coverage scores used in the main analysis rather than separately reporting every threshold-specific result.

---

# Terminology Note for Expected-Result Presence Coverage

The original experimental implementation and generated result files use the term:

`Oracle-Level Coverage`

In the revised manuscript, this metric is referred to as:

**Expected-Result Presence Coverage (ERPC)**

The terminology was revised because the metric determines whether a meaningful expected-result field is present in a generated test case. It does **not** establish that the expected result is a correct executable oracle.

Therefore:

`Oracle-Level Coverage` in the repository = `Expected-Result Presence Coverage (ERPC)` in the revised manuscript.

This terminology revision does not change the implementation, calculation, or any numerical result. The original experimental files are intentionally preserved in the repository for reproducibility.

---

# Redundancy Analysis

**File:**

`Redundancy Analysis.ipynb`

This notebook evaluates redundancy across the generated test-case specifications.

The analysis identifies normalized exact duplicate test cases after removing non-substantive differences such as test-case identifiers, numbering, punctuation, and whitespace while preserving substantive test-case content.

Redundancy is evaluated independently for:

### Phase 1

Design-based test-case specifications generated from UML-derived descriptions.

**Result archive:**

`PHASE1_DESIGN_REDUNDANCY_RESULT.zip`

### Phase 2

Requirement-based test-case specifications generated from software requirement reports.

**Result archive:**

`PHASE2_REQUIREMENT_REDUNDANCY_RESULT.zip`

### Phase 3

Code-derived test-case specifications generated from source code.

**Result archive:**

`PHASE3_CODE_REDUNDANCY_STRICT_RESULT.zip`

The redundancy analysis is intended to characterize repetition within the generated outputs and should not be interpreted as executable test validity.

---

# Direct UML-to-Test-Case Baseline

**File:**

`Direct_UML_to_Testcase_Baseline.ipynb`

This notebook implements an additional baseline in which the UML diagram image is provided directly for test-case generation.

Unlike the main Phase 1 pipeline:

`UML image → UML description → test-case generation`

the baseline follows:

`UML image → test-case generation`

This experiment is used to examine the contribution of the intermediate UML-description stage while maintaining the same type of visual UML input.

---

## Direct UML Baseline Generation Results

One ZIP archive contains the test-case specifications generated directly from UML diagram images using the evaluated VLM configurations.

These outputs represent the direct UML-to-test-case baseline.

---

## Direct UML Baseline Comparison

**File:**

`Direct_UML_to_Testcase_Baseline_Comparison.ipynb`

This notebook compares the direct UML-to-test-case baseline with the existing Phase 1 pipeline.

The comparison considers UML-information preservation across dimensions such as:

- UML page coverage
- Components and actors
- Relationships and messages
- Main behavioral flows
- Conditions and branches
- Overall structural coverage
- Redundancy

A separate ZIP archive contains the resulting baseline comparison files.

The purpose of this experiment is not to claim superiority over traditional model-based UML test-generation techniques. It provides a directly compatible baseline under the visual UML input setting used in this study.

---

# Requirement-Based External Baseline

The repository also contains the implementation and result files for an external requirement-to-test-case baseline evaluation.

The baseline uses requirement-test datasets that provide requirement information together with associated reference test information.

The same requirement-based generation models used in Phase 2 are evaluated in this baseline so that the generation behavior can be assessed on external requirement-test relationships.

The corresponding notebook contains the baseline implementation, while the accompanying ZIP archive contains the generated outputs and evaluation results.

This evaluation provides an additional external reference for requirement-based test-case generation. It does not represent executable correctness or runtime test effectiveness.

---

# Outputs Folder

The `Outputs` folder contains representative generated artifacts and evaluation outputs for **Report 2**.

The representative files allow the phase-wise workflow to be inspected without publishing the complete confidential project-report collection.

---

# Alignment Score Output

**Folder:**

`Outputs/Alignment_Score_Codellama`

This folder contains four sets:

- `Set 1`
- `Set 2`
- `Set 3`
- `Set 4`

Each set corresponds to a different requirement-based test-case generation LLM configuration.

Within each set, alignment results are organized according to UML descriptions generated by the different VLM configurations.

---

# Semantic Coverage Output

**Folder:**

`Outputs/Semantic Coverage`

This folder contains the semantic coverage outputs.

The `qwen 7b` folder contains semantic coverage results evaluated using UML descriptions generated by VLM configurations including:

- Gemma-3-4B
- Gemma-3-12B
- Llama
- LLaVA
- Qwen

The additional coverage ZIP and notebook complement these semantic coverage outputs with the objective-level metrics described above.

---

# Representative UML and Test-Case Outputs

The `Outputs` folder also contains representative files from the phase-wise pipeline.

## `Report_2_uml_pages (uml diagrams).pdf`

Extracted UML pages identified during Phase 1 Part 1.

## `Report_2_uml_pages (uml description).txt`

Structured UML-derived descriptions produced during Phase 1 Part 2.

## `Report_2_uml_pages_design_testcases.csv`

Design-based test-case specifications produced during Phase 1 Part 3.

## `Report_2_Req_testcases.csv`

Requirement-based test-case specifications produced during Phase 2.

---

# Final Experimental Results

**File:**

`UMLFinalResults.xlsx`

This workbook contains the aggregated experimental results used in the study.

The updated workbook contains **five sheets**.

## 1. Alignment Score

Contains semantic alignment results between:

- Design-based and code-derived test cases
- Requirement-based and code-derived test cases

## 2. Semantic Coverage

Contains the primary element-level semantic coverage results across the evaluated VLM and LLM configurations.

## 3. Sustainability

Contains the original sustainability measurements collected during the Google Colab experiments, including:

- Energy consumption
- Carbon emissions
- SCI values
- Execution-related sustainability measurements

## 4. Additional Coverage

Contains the additional objective-level coverage results, including:

- Co-occurrence-Aware UML Test Objective Coverage
- Condition-Bound Transition Coverage
- Expected-Result Presence Coverage

Where the original experimental result files use `Oracle-Level Coverage`, this corresponds to Expected-Result Presence Coverage in the revised manuscript, as explained in the terminology note above.

## 5. Local GPU

Contains the complementary fixed-region local-GPU sustainability results.

These experiments were conducted under a common Alberta regional grid-intensity context and provide an additional sustainability evaluation alongside the original Google Colab experiments.

Because the Google Colab and local-GPU experiments use different hardware and execution environments, differences between these settings should not be attributed solely to geographic region or grid carbon intensity.

---

# Prompt Templates

**File:**

`Prompts.pdf`

The updated prompt document contains the prompt templates used in the prompt-driven stages of the study.

It includes prompts for:

- Phase 1 Part 2: UML description generation
- Phase 1 Part 3: Design-based test-case generation
- Phase 2: Requirement-based test-case generation
- Phase 3: Code-based test-case generation
- Direct UML-to-test-case baseline

Phase 1 Part 1 is not included in the prompt document because UML extraction is implemented using the fine-tuned classifier and does not use a natural-language prompt.

---

# Sustainability Evaluation

The repository contains sustainability results from two execution settings.

## Original Google Colab Evaluation

The original phase-wise experiments were executed using Google Colab GPU environments.

The corresponding sustainability results include energy consumption, carbon emissions, and SCI values recorded during model inference.

## Fixed-Region Local-GPU Evaluation

A complementary local-GPU evaluation was also conducted using a common Alberta regional grid context.

These results are available in the `Local GPU` sheet of:

`UMLFinalResults.xlsx`

The fixed-region evaluation provides an additional environmental perspective while preserving the original multi-region Google Colab measurements.

The two execution settings differ in hardware and runtime environment. Therefore, cross-setting differences are treated as empirical observations rather than effects attributable to a single environmental factor.

---

# Execution Environment

The phase-wise generation notebooks were designed primarily for Google Colab.

## A100 GPU

An A100 GPU was used for the computationally intensive generation stages, including:

- UML classification and extraction
- VLM-based UML description generation
- Design-based test-case generation
- Requirement-based test-case generation
- Code-based test-case generation

## T4 GPU

A T4 GPU was used for evaluation stages such as:

- Alignment analysis
- Semantic coverage
- Additional semantic/objective-level coverage analysis

The local-GPU sustainability experiments were executed separately and their results are provided in the final results workbook.

---

# How to Run

## Step 1: Obtain the Classifier Checkpoint

Download:

`uml_clip_lora_classifier_N12.pt`

from the Google Drive link provided above.

## Step 2: Open the Required Notebook

Open the required `.ipynb` file in Google Colab.

## Step 3: Select the GPU Runtime

Use the appropriate GPU according to the experiment being reproduced.

## Step 4: Update Configuration Fields

Where required, update:

- Input paths
- Output paths
- Model name
- Hugging Face authentication

Do not commit personal Hugging Face access tokens to the repository.

## Step 5: Run the Notebook

Run the notebook cells sequentially.

## Step 6: Inspect Generated Results

Generated files are saved to the configured output directory.

The ZIP archives included in this repository provide the corresponding derived results for the additional evaluations described above.

---

# Reproducibility and Data Availability

This repository provides the implementation, prompt templates, representative generated artifacts, evaluation notebooks, derived result archives, baseline experiments, and aggregated result workbook used in the study.

The complete original software project reports and source-code repositories are not publicly released because of confidentiality restrictions.

A representative report is provided to illustrate the complete processing workflow, while derived and aggregated results are included to support inspection and reproducibility of the reported evaluation.

---

# Important Interpretation Note

The generated outputs in this repository are **test-case specifications** derived from design artifacts, requirements, and source code.

The reported alignment and semantic coverage metrics evaluate semantic correspondence and preservation of software information across artifacts. They do not establish compilation success, runtime pass/fail behavior, executable code coverage, mutation score, fault-detection effectiveness, or oracle correctness.

Similarly, the sustainability results are operational estimates recorded for the evaluated inference workloads and execution environments.

---
