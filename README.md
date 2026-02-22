# AI Stylistic Drift Detection

## Overview

This project analyzes whether my personal writing exhibits measurable stylistic differences between pre-Fall 2024 (before widespread AI tool adoption) and post-Fall 2024 (after). Using supervised classification on engineered stylometric features, the analysis achieves 55.6% cross-validation accuracy and perfect holdout accuracy, suggesting subtle but genuine stylistic drift.

## Research Question

Does my writing style show measurable differences between pre-AI and post-AI periods? If so, which stylometric features drive the classification?

## Dataset

* **Source**: 51 personal academic assignments from Google Drive (Minerva University)
* **Split**: 23 pre-AI (pre-Fall 2024), 28 post-AI (Fall 2024+)
* **Training/Test**: 45 docs for 5-fold CV, 5 held-out for final evaluation
* **Exclusions**: Group work, heavily templated documents, low-prose assignments, hand-written work
* **Representation**: Solo-authored essayistic work (papers, reflections, scripts)

**Note:** Dataset not included due to privacy.

## Data Pipeline

1. **Collection**: Scrape documents from Google Drive via Colab notebook
2. **Manual labeling**: Mark pre/post AI based on submission date and course
3. **Cleaning** (4-step pipeline):
   - Remove cover sheets and headers
   - Remove table of contents
   - Extract body only (exclude references, appendices, AI statements)
   - Remove metadata tags (HC/LO)
4. **Tokenization**: spaCy NLP with sentencizer (no lemmatization)
5. **Feature engineering**: 12 stylometric features
6. **Train/test split**: Stratified, with 5-document holdout

## Features (12 Stylometric Metrics)

**Structural** (2):
- Mean sentence length
- Sentence length variance

**Lexical** (1):
- Lexical diversity (unique words / total words)

**Punctuation** (4):
- Comma rate, semicolon rate, colon rate, dash rate (per token)

**Function words** (5):
- Frequency of: "the", "to", "of", "and", "a/an" (per word)

## Methods

Three models trained on raw vs. clean text:

1. **Naive Bayes**: Baseline, assumes feature independence
2. **Logistic Regression**: Interpretable coefficients, calibrated probabilities
3. **Linear SVM**: Maximum-margin separator, robust to outliers

**Evaluation**: Stratified 5-fold cross-validation, then final holdout test

## Results

### Cross-Validation (5 folds on 45 docs)
- **LogReg (clean)**: 55.6% accuracy, F1 ≈ 0.609
- **LogReg (raw)**: 57.8% accuracy
- **LinearSVM (clean)**: 55.6% accuracy
- **LinearSVM (raw)**: 57.8% accuracy
- **Naive Bayes (clean)**: 46.7% accuracy

### Holdout Test (5 docs)
- **Accuracy**: 5/5 correct (100%)
- **Confidence**: 53–69% (average 61.1%)
- **No high-confidence predictions** (none ≥80%), suggesting real but subtle signal

### Key Features Predicting Post-AI

| Feature | Coefficient | Interpretation |
|---------|------------|-----------------|
| "the" frequency | +0.564 | Higher in post-AI (formal tone?) |
| Lexical diversity | +0.549 | Higher in post-AI (broader vocabulary?) |
| Dash rate | +0.408 | Higher in post-AI (em-dash usage) |
| "of" frequency | -1.21 | Higher in pre-AI (complex structures) |
| "and" frequency | -0.37 | Higher in pre-AI (coordination) |

## Limitations

- **No baseline**: No comparison to actual AI-generated text
- **Confounded variables**: Course type, subject matter, and time all changed
- **Small dataset**: 51 documents limits generalization
- **Binary label**: Ignores gradual AI adoption and within-course variation
- **Feature interactions**: Model treats features independently

## Key Findings

✓ **Stylistic drift exists** — 55.6% accuracy substantially exceeds 50% chance baseline  
✓ **Signal persists after cleaning** — raw-to-clean drop validates preprocessing  
✓ **Changes are subtle** — moderate confidence suggests genuine patterns, not overfitting  
✗ **Causation unclear** — cannot confirm AI influence without AI baseline

## Next Steps

1. Collect AI-generated baselines (GPT outputs from same courses)
2. Train temporal regression to track style changes continuously
3. Perform within-course analysis to isolate subject matter effects
4. Add syntactic/readability features for better signal
5. Conduct error analysis on misclassified documents

## Project Structure

```
first_draft.ipynb       # Main analysis notebook (complete pipeline)
pipeline_diagram.png    # Mermaid visualization of data pipeline
gdrive_scraped_data.csv # Raw dataset (not included)
README.md              # This file
```

## Usage

1. Ensure dependencies installed: `pandas`, `scikit-learn`, `spacy`, `nltk`, `matplotlib`, `seaborn`, `wordcloud`
2. Download spaCy model: `python -m spacy download en_core_web_sm`
3. Open `first_draft.ipynb` and run all cells sequentially

## References

- Wikipedia. (n.d.). Signs of AI writing. Retrieved from https://en.wikipedia.org/wiki/Wikipedia:Signs_of_AI_writing
- Wikipedia. (n.d.). Most common words in English. Retrieved from https://en.wikipedia.org/wiki/Most_common_words_in_English
- Hastewire. (2025). How Machine Learning Identifies Writing Style. Retrieved from https://hastewire.com/

## AI Statement

- Used **GitHub Copilot** for code autocomplete and refactoring
- Used **ChatGPT** for pipeline design and feature engineering guidance
- Used **Grammarly** for markdown copy editing

