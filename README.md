# seo-content-detector

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
```bash
git clone https://github.com/yourusername/seo-content-detector
cd seo-content-detector

**Install dependencies**
```bash
pip install -r requirements.txt

**Run Jupyter notebook**
```bash
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

- **BeautifulSoup + Multi-Strategy Parsing**: Prioritizes main → article → content-divs → all paragraphs selectors; handles 77.8% of diverse website structures without requiring ML-based extraction
- **Textstat for Readability**: Industry-standard Flesch Reading Ease (0-100 scale) provides normalized quality assessment independent of domain
- **Cosine Similarity (0.80 threshold)**: Captures semantic overlap while maintaining 20% tolerance for minor variations; faster and more interpretable than deep learning alternatives
- **Baseline Model Selection**: Word count alone achieved 94.4% accuracy, outperforming multi-feature models due to class imbalance (High: 6.7%, Low: 61.7%); prevents overfitting in small dataset
- **Stratified Train-Test Split**: Maintains label distribution across 70/30 split despite imbalance; ensures minority classes in both sets

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

### Installation

