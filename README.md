# AI Stylistic Drift Detection

## Overview

This project explores whether my writing style has changed over time alongside increased exposure to AI-generated text. The goal is not to build a highly accurate model, but to examine the machine learning pipeline and identify potential sources of bias, leakage, and confounding.

## Research Question

Does my writing exhibit measurable stylistic differences between pre-AI and post-AI periods?

## Data

* Source: Personal academic assignments (Google Docs, PDFs, limited notebooks)
* Only **final, solo-authored submissions** are included
* Excluded:

  * group assignments
  * heavily templated work
  * math-heavy / low-prose documents
  * very short texts

Labels are assigned based on submission year (pre vs post AI exposure).

**Note:** Dataset is not included due to privacy and size.

## Data Pipeline

1. Collect files from structured storage (Google Drive)
2. Filter using filename tag `[final]`
3. Extract text from `.docx` and `.pdf`
4. Apply light cleaning (e.g., remove headers, AI statements)
5. Save to `dataset.csv`

## Usage

1. Generate dataset:

   * Run `src/ingest.py` (see script for details)

2. Place dataset:

   * Save `dataset.csv` in `data/` directory

3. Run analysis:

   * Open `notebooks/analysis.ipynb`

## Methods

* Tokenization + basic preprocessing
* Naive Bayes classifier (EDA-focused)
* Inspection of feature importance to identify learned signals

## Limitations

* Potential confounding from course content and assignment type
* Residual template and formatting effects
* Small dataset size
* Labeling based on time, not verified AI usage

## Goal

The emphasis is on understanding:

* how modeling decisions affect results
* how leakage and bias emerge in practice
* what the model is actually learning

## Structure

```
src/          # data ingestion and preprocessing
notebooks/    # analysis and modeling
data/         # local dataset (ignored by git)
```
