# Stylometric Drift Detection in Personal Writing

## Overview

This project explores whether measurable stylistic differences can be detected in my personal writing across two time periods: pre-Fall 2024 and post-Fall 2024. Using supervised classification with engineered stylometric features, the analysis evaluates whether a weak but consistent signal of stylistic drift can be identified under controlled evaluation settings.

The best-performing models achieve ~55–58% cross-validation accuracy, with consistent performance across Logistic Regression and SVM, suggesting a weak but detectable distribution shift in writing style under limited data conditions.

## Research Question

Can stylometric features be used to distinguish between earlier and later personal writing samples, and if so, which features contribute most to classification performance?

## Dataset

- **Source**: 50 personal academic assignments (Google Drive, Minerva University)
- **Split**: 22 pre-Fall 2024, 28 post-Fall 2024
- **Training/Test**: 45 documents used for 5-fold cross-validation, 5 held-out for final evaluation
- **Exclusions**: Group work, templated assignments, non-essay formats, handwritten work
- **Text type**: Solo-authored academic and reflective writing

**Note:** Dataset is not included due to privacy constraints.

## Data Pipeline

1. **Collection**: Documents exported from Google Drive via Colab notebook  
2. **Labeling**: Manual labeling based on timestamp and course context  
3. **Cleaning pipeline**:
   - Removal of headers and cover pages  
   - Removal of table of contents  
   - Extraction of main body text only  
   - Removal of references, appendices, and metadata artifacts  
4. **Tokenization**: spaCy sentencizer (no lemmatization applied)  
5. **Feature engineering**: Extraction of 12 stylometric features  
6. **Evaluation split**: Stratified 5-fold cross-validation with a held-out test set  

## Features (12 Stylometric Metrics)

### Structural (2)
- Mean sentence length  
- Sentence length variance  

### Lexical (1)
- Lexical diversity (unique tokens / total tokens)  

### Punctuation (4)
- Comma rate  
- Semicolon rate  
- Colon rate  
- Dash rate  

### Function Words (5)
- Frequency of: “the”, “to”, “of”, “and”, “a/an”  

## Methods

Three classical classifiers were evaluated:

- **Naive Bayes**: Baseline probabilistic model assuming feature independence  
- **Logistic Regression**: Interpretable linear classifier with calibrated probabilities  
- **Linear SVM**: Margin-based classifier robust to high-dimensional feature spaces  

Evaluation was performed using stratified 5-fold cross-validation, followed by final testing on a held-out subset.

## Results

### Cross-Validation (45 documents, 5 folds)

- Logistic Regression (clean): 55.6% accuracy, F1 ≈ 0.609  
- Logistic Regression (raw): 57.8% accuracy  
- Linear SVM (clean): 55.6% accuracy  
- Linear SVM (raw): 57.8% accuracy  
- Naive Bayes (clean): 46.7% accuracy  

### Held-Out Test Set (5 documents)

- Accuracy: 5/5 correct (100%)  
- Mean confidence: 61.1% (range: 53–69%)  
- No high-confidence predictions (>80%), suggesting weak but consistent signal rather than strong separability  

## Most Informative Features

| Feature | Effect | Interpretation |
|--------|--------|----------------|
| "the" frequency | +0.564 | Increased usage in post-period writing |
| Lexical diversity | +0.549 | Slight increase in vocabulary variation |
| Dash usage | +0.408 | Increased use of em-dash style punctuation |
| "of" frequency | -1.21 | Higher usage in earlier writing samples |
| "and" frequency | -0.37 | Higher coordination in earlier writing |

## Limitations

- **No external baseline** (e.g., AI-generated text comparison)  
- **Strong confounding variables** (topic, course type, time)  
- **Small sample size** limits generalization  
- **Binary labeling simplifies a continuous process**  
- **Feature independence assumption limits interpretability of interactions**  

## Key Findings

- A weak but consistent classification signal exists above chance baseline  
- Signal remains stable across model types, suggesting robustness to classifier choice  
- Performance is modest, indicating subtle stylistic variation rather than strong separability  
- Results do not establish causation or attribute changes to any specific factor  

## Next Steps

1. Introduce external baseline comparisons (e.g., model-generated text)  
2. Expand dataset with additional writing samples over time  
3. Perform within-course or within-topic stratification  
4. Incorporate syntactic and readability features  
5. Conduct error analysis on misclassified samples  

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
