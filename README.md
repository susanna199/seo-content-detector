# SEO Content Quality & Duplicate Detector

An end-to-end machine learning pipeline that extracts content from HTML documents, analyzes SEO quality metrics, detects duplicate content using TF-IDF and cosine similarity, and predicts content quality using classification models. The system processes a dataset with 81 websites, identifies quality issues, and flags near-duplicate pages for SEO optimization.

---

## Table of Contents

- [Project Overview](#project-overview)
- [Setup Instructions](#setup-instructions)
- [Quick Start](#quick-start)
- [Key Decisions](#key-decisions)
- [Results Summary](#results-summary)
- [Limitations](#limitations)
- [Project Structure](#project-structure)
- [Technologies Used](#technologies-used)

---

## Project Overview

This project implements a complete data science pipeline for SEO content analysis with five phases:

1. **Phase 1**: HTML parsing and text extraction (77.8% success rate)
2. **Phase 2**: NLP feature engineering (word count, readability, keywords)
3. **Phase 3**: Duplicate detection using cosine similarity (threshold: 0.80)
4. **Phase 4**: Quality classification with multiple ML models
5. **Phase 5**: Real-time analysis on new URLs

---

## Setup Instructions

### Prerequisites

- Python 3.8 or higher  
- pip package manager  
- Git  

### Installation

**Clone repository**

git clone https://github.com/yourusername/seo-content-detector
cd seo-content-detector

**Install dependencies**

pip install -r requirements.txt

**Run Jupyter notebook**

jupyter notebook notebooks/seo_pipeline.ipynb

---

## Quick Start

Execute the pipeline phases sequentially:
python phase_1_extract.py # Extract text from HTML (63/81 success)
python phase_2_features.py # Calculate NLP features
python phase_3_duplicates.py # Detect duplicates (0.80 threshold)
python phase_4_classification.py # Train quality models
python phase_5_analyze.py # Real-time URL analysis

Output files: `data/extracted_content.csv`, `data/features.csv`, `data/duplicates.csv`

---

## Key Decisions

## Key Decisions

**1. Choice of Libraries & Why**

  - **BeautifulSoup4**: BeautifulSoup's flexible selectors allowed to try different tag strategies (main → article → content divs) without crashing. 
  - **Scikit-learn**: Industry-standard ML library with well-tested implementations of Logistic Regression, Random Forest, and SVM; integrated preprocessing pipelines reduce manual tuning and errors.
  - **Textstat**: Standardized readability calculations (Flesch Reading Ease) independent of domain; easier than building custom linguistic analysis.

**2. HTML Parsing Approach: Multi-Strategy Selector Priority**

Implemented fallback hierarchy: `main` → `article` → `content-divs` → `all paragraphs`. This ensures robustness across diverse website architectures (news sites, blogs, documentation) where content location varies. Single-strategy parsing fails on 22% of websites; multi-strategy achieves 77.8% success rate. Strategy avoids machine learning overhead while maintaining interpretability.

**3. Similarity Threshold Rationale: 0.80 Cosine Similarity**

Threshold balances precision vs. recall in duplicate detection. Scores range 0-1; at 0.80, captures genuine near-duplicates (paraphrased or slightly modified content) while avoiding false positives from similar-topic articles. Lower thresholds (0.5-0.7) flag too many non-duplicates; higher thresholds (0.9+) miss legitimate duplicates. 0.80 validated against domain expertise for SEO use-cases.

**4. Model Selection Reasoning: Baseline Word Count Over Complex Models**

Counter-intuitive finding: simple baseline (word count only) achieved 94.4% accuracy vs. 88.9% for Random Forest using all features. Root cause: severe class imbalance (High: 6.7%, Low: 61.7%) in small dataset (60 samples). Complex models overfit to imbalanced minority class; word count as single feature avoids overfitting while capturing primary quality signal. Stratified train-test split (70/30) ensures results reliability despite imbalance.

**5. Stratified Train-Test Split Over Random Split**

Stratification maintains label distribution (6.7% High, 31.7% Medium, 61.7% Low) across train/test sets despite class imbalance. Random split risks zero High-quality samples in test set, invalidating minority-class evaluation. Stratification ensures at least 1 sample per class in test set, enabling meaningful per-class metrics and preventing skewed performance estimates.

---

## Results Summary

**Model Performance**:
- Baseline (word_count): **94.4% accuracy, 95.1% F1-score**
- Random Forest: 88.9% accuracy, 88.8% F1-score
- SVM: 83.3% accuracy, 85.6% F1-score
- Logistic Regression: 72.2% accuracy, 73.1% F1-score

**Quality Analysis**:
- Total documents: 81 analyzed
- Successfully extracted: 63 (77.8%)
- Average content length: 1,507 words (5–9,974 range)
- Readability average: 52.3 (moderate level)

**Duplicate Detection**: 18 thin content pages identified (<500 words)

---

## Limitations

- **Class Imbalance**: Only 4 high-quality documents; single test sample limits confidence in rare-class predictions
- **Small Dataset**: 60 samples insufficient for complex model generalization; results may not transfer to different domains
- **HTML Structure**: Multi-selector strategy handles 77.8% of websites; ineffective for JavaScript-rendered or non-standard layouts

---

## Technologies

Pandas, NumPy, BeautifulSoup4, Textstat, Scikit-learn, Jupyter, Matplotlib, Seaborn


