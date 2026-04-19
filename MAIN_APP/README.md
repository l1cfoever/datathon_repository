# ViralWave — Section B MVP

Cross-cultural YouTube trend arbitrage tool. Built for the Innovation Challenge, Theme 1.

**Section B ownership:** Technical Execution (15%) + Innovation Implementation (15%) = **30% of the score**.

---

## 60-second setup

```bash
# 1. install
pip install -r requirements.txt

# 2. drop Member C's data (one-time)
mkdir -p data
cp /path/to/cleaned_core_trends.csv data/

# 3. (optional) enable live AI — without this, the mock fallback still works
export GEMINI_API_KEY=your_key_here     # macOS/Linux
# Windows PowerShell:  $env:GEMINI_API_KEY="your_key_here"

# 4. run
streamlit run app.py
```

Opens at `http://localhost:8501`. Free Gemini key: https://aistudio.google.com/apikey

The sidebar shows a status indicator — if you see **🟢 Real data (10,000 rows)** the integration worked.


---

## Important framing

The dataset is a **single-day snapshot** (Feb 26, 2026). This means we **cannot** truthfully claim "Canada trends 48 hours before Country X" — that requires multi-day data.

We pivoted the arbitrage feature to **cultural overlap**, computed from the data:

> **What we measure now:** Of the videos trending in Country A, what % also trend in Country B? Higher overlap = stronger cultural corridor = more reliable arbitrage signal.

> **What v2 unlocks:** Once a multi-day snapshot is delivered, we add the temporal lag dimension — predicting **when** a trend will cross, not just **whether**.

**Real corridors the app surfaces from the data:**
- Canada ↔ USA: 89% overlap (1,015 shared videos)
- Canada ↔ UK: 86% overlap (957 shared)
- UK ↔ USA: 84% overlap (933 shared)
- ...plus 52 more, all computed live from Member C's data.

---

## What this ships

| Page | Rubric coverage |
|---|---|
| **Dashboard** — hero metrics, top trending, country × category heatmap | Product Design + Data Strategy |
| **Virality Predictor** — keyword → score + AI brief + comparable real videos | **Technical Execution (core)** |
| **Cross-Cultural Arbitrage** — Sankey of corridor flows, top opportunities, v2 roadmap | **Innovation (core)** |
| **Data Insights** — ECR, Time-to-Trend distribution, raw data view | Data Strategy |


---


## Files

```
viralwave/
├── app.py                              # single-file Streamlit app
├── requirements.txt                    # deps
├── README.md                           # this
└── data/
    └── cleaned_core_trends.csv         # Member C's data (you drop it here)
```

---

## Schema reference 

The app expects these columns in the CSV. If Member C's schema changes, edit the rename block in `load_dataset()`:

| Required | Type | Source |
|---|---|---|
| `video_id` | str | Member C |
| `Country` | str (ISO code) | Member C |
| `Category_Name` | str | Member C |
| `title` | str | Member C |
| `views`, `likes`, `comments` | int | Member C |
| `Engagement_Rate` | float (0-1) | Member C (pre-computed) |
| `Time_to_Trend_hrs` | float | Member C (pre-computed) |

Ship it.
