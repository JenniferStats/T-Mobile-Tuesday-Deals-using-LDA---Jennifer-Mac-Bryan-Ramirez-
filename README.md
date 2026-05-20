# 🏷️ T-Mobile Tuesdays Discount Categorization Using Topic Modeling 

**Authors:** Jennifer Mac & Bryan Ramirez

## Overview

This project analyzes data from T-Mobile Tuesdays, a weekly promotion offered by T-Mobile, an American wireless network operator. Every Tuesday, T-Mobile members receive discounts on 3 to 7 randomly selected items, ranging from fast food deals to high-end brands like Adidas.

Given that T-Mobile operates at a higher price point than competing carriers, this project aims to evaluate the practical value of its membership perks by applying topic modeling to categorize and analyze the distribution of weekly discount offers. Identified categories include food, gas, clothing, and cosmetics, among others. By determining what proportion of offers fall into each category, we can estimate the percentage of discounts a typical subscriber would realistically utilize. For instance, if food-related offers account for 60% of all discounts and clothing accounts for 5%, a subscriber who redeems only food and clothing discounts could still take advantage of up to 65% of all available offers.

Data was collected from r/TMobileTuesdays, a Reddit community in which members share and request unused discount codes on a weekly basis, with archived posts dating back to 2016. Data collection was performed using Arctic Shift (arctic-shift.photon-reddit.com), a third-party tool designed for deep archiving and research of Reddit data.

---

## Discovered Topics

Using BERTopic, the weekly discounts were grouped into 10 distinct categories:

| Topic | Label |
|-------|-------|
| 0 | Pizza Chains |
| 1 | Gas Stations |
| 2 | Photo & Gifts |
| 3 | Clothing |
| 4 | Movie Ticketing |
| 5 | Sports Streaming |
| 6 | Digital Subscriptions |
| 7 | Smoothie & Food Chains |
| 8 | Warehouse Membership |
| 9 | Headwear |

---

## Pipeline Overview

### Step 0 – Setup
Install dependencies and import all required libraries.

### Step 1 – Data Collection
Fetch posts and top-level comments from r/TMobileTuesdays using the Arctic Shift API, then save the raw data to disk.

### Step 2 – Dataset Construction
- Parse and clean raw Reddit comments
- Define known brands and alias mappings
- Extract brand mentions from comments using regex, fuzzy matching, and PMI phrase detection
- Clean and filter comments per brand
- Write hand-crafted descriptions for each brand's typical T-Mobile Tuesday offer

### Step 3 – Topic Modeling
Train and compare three topic models on the brand descriptions:
- **LDA** (Latent Dirichlet Allocation) via Gensim
- **BERTopic** using sentence embeddings + UMAP + HDBSCAN
- **Top2Vec** using joint document/word embeddings

Models are evaluated using the **C_V coherence score**. BERTopic achieved the highest coherence (0.76), significantly outperforming LDA (0.41) and Top2Vec (0.50).

---

## Coherence Score Comparison

| Model | Coherence (C_V) |
|-------|----------------|
| LDA | 0.4068 |
| Top2Vec | 0.5000 |
| BERTopic | 0.7633 |

---

## Installation

```bash
pip install pandas gensim spacy nltk matplotlib bertopic top2vec thefuzz python-Levenshtein sentence-transformers rapidfuzz
python -m spacy download en_core_web_sm
```

---

## Project Structure

```
├── T_mobile_Tuesdays_DataScrapping_LDA_BERT_2Vec.ipynb   # Main notebook
├── README.md
```

---

## Data Source

Reddit community: [r/TMobileTuesdays](https://www.reddit.com/r/TMobileTuesdays/)  
Scraping tool: [Arctic Shift](https://arctic-shift.photon-reddit.com)
