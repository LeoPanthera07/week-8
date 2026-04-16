# Week 08 — Wednesday Assignment
## CNNs + Embeddings · PG Diploma AI-ML & Agentic AI Engineering · IIT Gandhinagar

### Overview
This notebook covers two parallel tracks:
- **CNN track** — MNIST digit classification with filter visualisation
- **NLP/Embedding track** — Hate speech detection + semantic search on Twitter/Reddit data

### Sub-steps completed
| # | Level | Topic | Status |
|---|-------|-------|--------|
| 1 | 🟢 Easy | Social media EDA & class imbalance analysis | ✅ |
| 2 | 🟢 Easy | MNIST EDA & preprocessing | ✅ |
| 3 | 🟡 Medium | CNN architecture + filter visualisation | ✅ |
| 4 | 🟡 Medium | Hate speech classifier + semantic search | ✅ |
| 5 | 🟡 Medium | Two-stage moderation pipeline + evaluation | ✅ |
| 6 | 🔴 Hard | TF-IDF vs Sentence Embeddings (empirical) | ✅ |
| 7 | 🔴 Hard | Transfer learning experiment (MNIST → Text) | ✅ |

### How to Run

**Python version:** 3.10+

**Install dependencies:**
```bash
pip install torch torchvision numpy pandas matplotlib seaborn scikit-learn kagglehub sentence-transformers pillow
```

**Run notebook:**
```bash
jupyter notebook W8_Wednesday_Assignment.ipynb
```
Or execute all cells in sequence from top to bottom.

**Datasets are auto-downloaded** via `kagglehub` — no manual download needed.  
Requires a Kaggle API token (`~/.kaggle/kaggle.json`).

### Output Files
All generated artefacts land in `output/`:

| File | Description |
|------|-------------|
| `substep1_distributions.png` | Sentiment / hate-speech / spam class distributions |
| `substep2_mnist_eda.png` | MNIST digit samples + class balance bar chart |
| `substep3_training.png` | CNN loss and accuracy curves |
| `substep3_filters.png` | 32 learned Conv1 filters visualised |
| `substep4_confusion.png` | Hate speech classifier confusion matrix |
| `substep5_pipeline.png` | Stage 1 vs Stage 2 recall/precision/F2 comparison |
| `substep6_comparison.png` | TF-IDF vs Embedding Jaccard overlap chart |
| `substep6_comparison.csv` | Full comparison data table |
| `substep7_transfer.png` | Transfer learning experiment results |

### Project Structure
```
week-08/
└── wednesday/
    ├── W8_Wednesday_Assignment.ipynb   ← main notebook
    ├── README.md                       ← this file
    ├── prompts.md                      ← AI usage log (policy requirement)
    └── output/                         ← all generated artefacts
```

### Engineering Quality Checklist (Band 4)
- [x] All functions named descriptively (`build_tfidf_hate_classifier`, `semantic_search_top_k`, …)
- [x] ≥ 2 functions per sub-step
- [x] No magic numbers — all constants named (`RANDOM_STATE`, `TOP_K_SIMILAR`, `DAILY_POST_VOLUME`, …)
- [x] `try/except` defensive handling around all I/O and model calls
- [x] No `.env` files, API keys, or `__pycache__` committed

### Recommended Git Commit Sequence
```
git commit -m "feat: add setup, imports, and named constants"
git commit -m "feat: sub-step 1 — social media EDA and class imbalance analysis"
git commit -m "feat: sub-step 2 — MNIST EDA and DataLoader preparation"
git commit -m "feat: sub-step 3 — CNN architecture, training, filter visualisation"
git commit -m "feat: sub-steps 4+5 — hate classifier, semantic search, two-stage pipeline"
git commit -m "feat: sub-step 6 (Hard) — TF-IDF vs embedding empirical comparison"
git commit -m "feat: sub-step 7 (Hard) — MNIST transfer learning experiment"
```
