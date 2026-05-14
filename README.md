# african-media-intelligence-ai
An end-to-end NLP pipeline for automated sentiment analysis, topic detection, named entity recognition, and reputation scoring across African media sources.


## 1. Project Overview

African media outlets produce thousands of articles daily across French and English, yet no automated tool exists to monitor public opinion, detect reputational risks, or track emerging narratives at scale. This project builds a complete NLP pipeline — from RSS ingestion to a visual analytics dashboard — to transform raw African media content into structured, decision-ready intelligence. The final output serves media monitoring agencies, fintechs, NGOs, and communications teams operating across the African continent.

## 2. Dataset

| Attribute | Detail |
|---|---|
| Sources | 8 African RSS feeds: RFI Afrique, BBC Afrique, Jeune Afrique, AllAfrica, Abidjan.net, Fratmat.info, Le Monde Afrique, The Africa Report |
| Volume | 121 articles per collection cycle |
| Languages | French (FR) + English (EN) |
| Coverage | Pan-African + Côte d'Ivoire focus |
| Entities extracted | 532 mentions across 58 unique entities |
| Topics | 8 thematic clusters |
| Time range | Rolling collection window (23 days in demo cycle) |
| Variables | Title, summary, source, language, publication date, sentiment score (×3), topic ID, entity name, entity type, RepScore |

## 3. Tools & Technologies

| Tool | Role in the pipeline |
|---|---|
| Python 3.13 | Core language for all data processing and NLP |
| `feedparser` + `BeautifulSoup4` | RSS ingestion and HTML cleaning (Phase 1) |
| `transformers` — XLM-RoBERTa | Multilingual sentiment analysis — FR + EN (Phase 2) |
| `scikit-learn` — TF-IDF + LDA | Topic modeling and keyword extraction (Phase 3) |
| `spaCy` — fr_core_news_md / en_core_web_sm | Named entity recognition — ORG, GPE, PER, LOC (Phase 4) |
| `pandas` + `numpy` | Data manipulation and RepScore computation (Phase 5) |
| `matplotlib` + `seaborn` + `wordcloud` | Python analytics dashboard — 6 production charts (Phase 6) |
| Gamma | Project presentation — 10-slide deck |

## 4. Project Steps

1. Data Collection (Python) — RSS feeds from 8 African sources are parsed, HTML-cleaned, deduplicated, and stored as structured CSV. A unified `texte_nlp` field concatenates title and summary for downstream NLP processing.

2. Sentiment Analysis (Python) — The multilingual XLM-RoBERTa model is applied to each article, returning three probability scores (Positive, Neutral, Negative) and a dominant label. Batch processing handles the full dataset efficiently.

3. Topic Modeling (Python) — Article texts are vectorized with TF-IDF (2,000 features, bigrams) and fed into a Latent Dirichlet Allocation model. Eight thematic clusters are extracted, each described by its top 10 keywords. Every article receives a dominant topic label and a relevance score.

4. Named Entity Recognition (Python) — spaCy models for French and English extract organizations, countries, cities, and people from each article. A custom African entity dictionary (50+ entries: MTN, CEDEAO, Flutterwave, Alassane Ouattara…) boosts detection of entities that generic models often miss.

5. Reputation Scoring (Python) — For each unique entity, a composite RepScore is computed by combining sentiment distribution, frequency of mentions, and recency. The score ranges from −100 to +100 and is classified as Positive (≥ 30), Neutral, or Negative (< −10). Results are exported to formatted Excel sheets.

6. Visualization Dashboard (Python) — Six production-grade charts are generated: a main dashboard canvas with KPI cards, a sentiment evolution time series with 7-day rolling average, an entity RepScore bar chart, a keyword word cloud, a NER breakdown by entity type, and a topic × sentiment heatmap.

7. Presentation (Gamma) — A 10-slide professional deck covers the business problem, solution architecture, pipeline, RepScore formula, key results, and business use cases.

## 5. Dashboard Preview

The Python dashboard includes:
- KPI cards: total articles, unique entities, topics detected, % positive and negative sentiment
- Stacked bar chart: sentiment breakdown per media source, revealing which outlets publish more negatively
- Sentiment time series: 7-day rolling average across all three sentiment classes, with a positive/negative ratio panel below

## 6. Key Results & Insights

- Neutral sentiment dominates (≈ 40 % of articles): most African media coverage of institutions and companies is factual and non-evaluative — sentiment monitoring alone is insufficient; topic context is required for meaningful interpretation.
- Economy & Finance and Politics & Governance account for the majority of articles: coverage is concentrated on macroeconomic and political events, leaving health, education, and environment underrepresented in the media landscape.
- Fintech and telecom organizations (MTN, Flutterwave, Orange CI) generate the highest entity mention volumes: these sectors attract sustained media attention and are therefore the most exposed to rapid reputational shifts.
- RepScore diverges significantly by source: the same organization can hold a positive reputation score on one outlet and a negative one on another — validating the need for source-level monitoring rather than aggregate scoring only.
- The pipeline is fully bilingual (FR + EN) and processes both languages within a single workflow, making it directly applicable to any pan-African monitoring use case without language-specific retraining.

## 7. How to Run

Prerequisites
- Python 3.10+
- Anaconda or virtualenv
- 4 GB RAM minimum (for XLM-RoBERTa inference)

```bash
# 1. Clone the repository
git clone https://github.com/esmel-amari/african-media-intelligence-ai.git
cd african-media-intelligence-ai

# 2. Install dependencies
pip install -r requirements.txt

# 3. Download spaCy language models
python -m spacy download fr_core_news_md
python -m spacy download en_core_web_sm

# 4. Run notebooks in order
# Open Jupyter Notebook and execute: 01 → 02 → 03 → 04 → 05 → 06

# 5. Generate the Power BI Excel file
python exports/powerbi_export.py
```

> Data is collected live from public RSS feeds. No external dataset download is required.  
> Demo CSV files are available in `data/processed/` for immediate testing without running the full pipeline.

---

Author — Esmel Amari  · Junior Data Analyst · Abidjan, Côte d'Ivoire · [esmel.netlify.app](https://esmel.netlify.app)
