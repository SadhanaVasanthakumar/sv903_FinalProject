# What the Headlines Say

**Measuring Cognitive Bias and Agency Framing in AI News Discourse**

CS 210: Data Management for Data Science — Rutgers University — Spring 2026  
Sadhana Vasanthakumar

---

## Overview

This project analyzes 179,372 AI news headlines scraped from 13,019 publishers across 20 monthly windows from August 2024 to March 2026. Three original analytical frameworks were built to measure how these headlines frame AI: the Power Shift Index (PSI), which tracks whether AI or humans appear as the grammatical subject of action verbs; the AI Anxiety Index (AAI), a weighted vocabulary score measuring fear and risk language; and the Invisible Human Framework, which measures how often AI coverage engages with human-experience vocabulary like grief, dignity, love, and community.

Key results: PSI declined at −10.6 points/month while remaining consistently above 700 (far above the balanced-framing threshold of 100); AAI rose at +0.00048 units/month with Safety and Risk headlines carrying five times the anxiety of product coverage; and 92.4% of all headlines contain zero vocabulary from any human-experience domain.

**[Interactive Website](https://sadhanavasanthakumar.github.io/sv903_FinalProject/)** — live headline scorer, full figure gallery, cluster explorer, and corpus browser.

---

## Project Structure

```
FinalProject/
├── 00_scraping.ipynb           # Data collection: 192 queries × 20 monthly windows
├── 01_database.ipynb           # SQLite schema, normalization, SQL analysis
├── 02_nlp_pipeline.ipynb       # VADER, embeddings, PSI, AAI, Invisible Human
├── 03_modeling.ipynb           # Ridge regression, decision tree, K-means clustering
├── 04_validation.ipynb         # Gold-standard hand labeling, control corpus
├── 05_ta_demo.ipynb            # TA demo: full pipeline on 500-headline sample (~10 min)
│
├── index.html                  # Self-contained interactive website
├── requirements.txt            # All Python dependencies with versions
│
├── data/
│   ├── ai_headlines_raw.csv         # 179,372 headlines, minimal columns [LFS]
│   ├── ai_headlines_full.csv        # 179,372 headlines, full metadata [LFS]
│   ├── ai_headlines_website.csv     # 3,000-headline stratified sample
│   ├── bias_observatory.db          # SQLite database (103MB) [LFS]
│   ├── allsides.csv                 # AllSides political lean ratings
│   ├── figshare_headlines.csv       # Figshare baseline corpus [LFS]
│   └── figshare_with_features.csv   # Figshare corpus with NLP features [LFS]
│
├── outputs/
│   ├── figures/                     # All 19 generated figures (PNG)
│   ├── demo/                        # Output from 05_ta_demo.ipynb
│   │   ├── features_demo.csv            # NLP feature table for demo sample
│   │   ├── monthly_trend_demo.csv       # Monthly aggregates for demo sample
│   │   └── demo_summary.txt             # One-page text summary of all results
│   ├── website_data.json            # Pre-computed data for the website
│   ├── validation_summary.json      # PSI kappa, IH control comparison results
│   ├── findings_interpretation.txt  # Domain-specific findings writeup
│   ├── monthly_trend.csv            # Monthly PSI and AAI values
│   ├── hypothesis_test_results.csv  # All five hypothesis test outputs
│   ├── cluster_stability_report.csv # ARI scores across seeds and subsamples
│   ├── lexicon_sensitivity.csv      # Keyword drop sensitivity analysis
│   ├── summary_ci.csv               # Bootstrap confidence intervals
│   └── gold_standard_labeled.csv   # 200 hand-labeled headlines
│
├── checkpoints/                     # Monthly scraping checkpoints (raw)
│   └── checkpoint_*.csv                 # One file per monthly window
│
└── scraping_results_summary.json    # Full scraping session metadata
```

---

## How to Run

**Prerequisites:** Python 3.10 or higher, Git LFS installed (see below).

```bash
# 1. Clone the repository
git clone https://github.com/sv903/FinalProject.git
cd FinalProject

# 2. Pull LFS files (large CSVs and the database)
git lfs pull

# 3. Install dependencies
pip install -r requirements.txt

# 4. Download NLTK data (required for VADER)
python -m nltk.downloader vader_lexicon
```

Then open JupyterLab or Jupyter Notebook and run the notebooks in order:

| Notebook | What it does | Runtime |
|---|---|---|
| `00_scraping.ipynb` | Scrapes 179,372 headlines from Google News | ~11 hours |
| `01_database.ipynb` | Builds SQLite database, runs SQL analysis | ~2 minutes |
| `02_nlp_pipeline.ipynb` | Computes all NLP scores | ~12 minutes |
| `03_modeling.ipynb` | Trains regression, classification, clustering | ~8 minutes |
| `04_validation.ipynb` | Runs validation and control corpus comparison | ~4 minutes |

> **Note on Notebook 00:** The scraping run takes roughly 11 hours and makes 3,840 API calls. If you're reproducing the analysis from the provided data files rather than re-scraping from scratch, you can skip Notebook 00 entirely. All downstream notebooks read from `data/ai_headlines_raw.csv` and `data/ai_headlines_full.csv`, which are already included in this repository.

---

## TA Demo (Notebook 05)

`05_ta_demo.ipynb` is a self-contained notebook designed for grading and quick reproducibility verification. It runs the complete pipeline — database construction, NLP scoring, modeling, and validation — on a 500-headline stratified sample drawn from the full corpus with a fixed random seed (`random_state=42`), so results are identical on every run.

**Expected runtime: 8–12 minutes on CPU.** The sentence-transformer encoding step is the bottleneck at roughly 60–90 seconds for 500 headlines.

**To run the demo:**

```bash
# From the repo root, after completing the setup steps above:
jupyter notebook 05_ta_demo.ipynb
# Then: Kernel → Restart & Run All
```

**What the demo covers:**

| Section | What it does |
|---|---|
| 0 — Sample generation | Draws 500 headlines from `ai_headlines_full.csv`, stratified by monthly window; saves to `data/demo_headlines.csv` |
| 1 — Database | Builds `data/demo_observatory.db` with the same four-table SQLite schema as notebook 01 |
| 2 — NLP pipeline | Runs VADER sentiment, PSI classification, AAI scoring, bias lexicon scoring, and Invisible Human detection on all 500 headlines |
| 3 — Modeling | Fits Ridge regression, decision tree, and K-means clustering; reports R², CV scores, and optimal k |
| 4 — Validation | PSI qualitative spot-check (5 headlines per class) and Invisible Human control corpus comparison |

**What the demo skips:**
- Re-scraping (notebook 00) — reads from the already-provided CSV
- matplotlib figures — all output is printed to stdout; no plots are generated
- Gold-standard manual annotation loop — replaced with a printed spot-check
- Live gnews control corpus scrape — replaced with a 30-headline hardcoded climate change control set

All outputs are written to `outputs/demo/`, including a `demo_summary.txt` with a one-page text summary of every result.

---

## Git LFS

This repository uses [Git LFS](https://git-lfs.com) for files over 50MB. The following files are tracked with LFS:

- `data/ai_headlines_full.csv` (~84MB)
- `data/ai_headlines_raw.csv` (~19MB)
- `data/bias_observatory.db` (~103MB)
- `data/figshare_headlines.csv` (~12MB)
- `data/figshare_with_features.csv` (~28MB)

**Installing Git LFS:**

```bash
# macOS
brew install git-lfs

# Ubuntu / Debian / WSL
sudo apt install git-lfs

# After installing the binary (all platforms)
git lfs install
```

If you clone the repository without Git LFS installed, the large files will appear as small pointer files rather than the actual data. Run `git lfs pull` after installing to download the real files.

---

## Data Sources

**Primary corpus:** 179,372 AI news headlines scraped using [gnews](https://pypi.org/project/gnews/) from Google News RSS feeds, August 2024 to March 2026. The scraping architecture is documented fully in `scraping_results_summary.json`.

**Figshare baseline:** [AI News Headlines Dataset (Aug 2024 to Jul 2025)](https://figshare.com/articles/dataset/Dataset_of_AI-related_News_Headlines_Aug_2024_Jul_2025_/30189736), used as a methodological baseline and for cross-validation.

**AllSides political lean data:** Publisher bias ratings from [AllSides](https://www.allsides.com/media-bias/media-bias-ratings), matched against 118 of 13,019 publishers in the corpus (10.3% of headlines by volume).

---

## Interactive Website
 
The project website is a single self-contained HTML file (`index.html`) with no server dependencies. It includes a live headline scanner using the same keyword lexicons from the notebooks, all 19 figures with descriptions, cluster explorer, monthly trend charts, and the full corpus breakdown.
 
#### The website is hosted at **[sadhanavasanthakumar.github.io/sv903_FinalProject](https://sadhanavasanthakumar.github.io/sv903_FinalProject/)** and can also be opened locally by simply opening `index.html` in any browser.

---

## Report and Submission

The final report PDF is included in the repository root as `sv903_FinalReport.pdf`.

All project materials including the demo video, report, code, and data are also submitted via Codebench under the "Final Project" assignment.

---

## Dependencies

See `requirements.txt` for pinned versions. The main packages are:

- `gnews` — Google News scraping
- `pandas`, `numpy` — data manipulation
- `nltk` — VADER sentiment analysis
- `sentence-transformers` — all-MiniLM-L6-v2 headline embeddings
- `scikit-learn` — regression, classification, clustering
- `scipy` — statistical tests
- `matplotlib`, `seaborn`, `networkx` — visualization

The sentence-transformers model (~80MB) is downloaded from HuggingFace Hub on first run and cached in `~/.cache/huggingface/`. An internet connection is required the first time only.

---

## Contact

Sadhana Vasanthakumar — Rutgers University, Department of Computer Science
