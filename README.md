# Spotify × FC Barcelona Sponsorship Analysis

A marketing analytics project examining how the Spotify–FC Barcelona partnership was received by fans and financial markets — using Reddit sentiment analysis and an event study to test whether public reaction and investor response told the same story.

> **Why this project?** Most sponsorship evaluations rely on brand recall surveys or press sentiment. This project takes a more rigorous, data-driven approach: using actual behavioral signals — Reddit comments and stock price data — to measure reaction to a major sports sponsorship announcement with reproducible, testable methods.

***

## The Central Question

> *Did the Spotify–FC Barcelona partnership generate measurable value — and did fans and investors see it the same way?*

Two sub-questions drive the analysis:
1. How did Reddit communities react to the deal announcement and the Camp Nou renaming?
2. Did the partnership announcement generate abnormal stock returns for Spotify?

***

## Why These Methods?

### Why Reddit for sentiment?
Social media captures **organic, unfiltered public reaction** — not the managed narratives brands put out for press. Reddit's structured, topic-focused communities (r/Barca, r/soccer, r/football) gave access to the exact audience most likely to have a strong and immediate opinion. Scraping these subreddits provided a diverse sample of 226 unique authors across three communities with different levels of attachment to FC Barcelona.

### Why RoBERTa over simpler sentiment tools?
VADER and basic lexicon-based tools struggle with informal language, sarcasm, and football slang. **RoBERTa** (`cardiffnlp/twitter-roberta-base-sentiment`), a transformer pre-trained on social media text, handles this significantly better. A **60% confidence threshold** was applied as a deliberate design choice: it filters out uncertain classifications without over-shrinking the sample — retaining 76.8% of comments and ensuring only high-confidence labels drive the results.

### Why an event study?
An event study is the **standard methodology in financial economics** for isolating a market's reaction to a specific announcement, stripped of broader market noise. By estimating a baseline model of expected returns over ~230 trading days before the announcement, the gap between actual and expected returns in the event window — the **abnormal return** — can be cleanly attributed to the announcement itself rather than overall market movement.

### Why two benchmark models?
Two specifications were used: a **value-weighted market index** and the **S&P 500**. Running both models is a standard robustness check — if the result holds across different benchmarks, it is more credible. Spotify is also a high-beta stock, meaning it amplifies broader market moves. Using two independent benchmarks helps confirm that the observed returns reflect the announcement rather than general market volatility.

***

## Key Findings

| Analysis | Finding |
|---|---|
| **Sentiment — Deal announcement** | Near-neutral net sentiment (+1.4%). Fans accepted the partnership but were not enthusiastic. |
| **Sentiment — Renaming announcement** | Strongly negative net sentiment (−70.8%). Negative comments more than doubled (17% → 37%). |
| **Hypothesis H1 (sentiment)** | Not supported. Neither event produced net positive sentiment. |
| **Hypothesis H2 (sentiment)** | Confirmed. Renaming sentiment was significantly worse than deal sentiment. |
| **Event Study — Value-Weighted** | CAR = **+2.21%** over 11-day window. Positive directional market reaction. |
| **Event Study — S&P 500** | CAR = **+4.15%** over 11-day window. Both models agree: positive market response. |
| **Hypothesis H1 (event study)** | Supported directionally. Abnormal returns were non-zero around the event date. |
| **Hypothesis H2 (event study)** | Supported directionally. CAR was positive across both model specifications. |

> **Important caveat:** Spotify is a high-beta stock that amplifies market moves. While the CAR is positive under both benchmarks, the result should be interpreted as a **directional signal** rather than a statistically conclusive finding — the high inherent volatility of Spotify's stock makes statistical significance harder to establish in a short event window.

> **The core insight:** Fans and investors did not react the same way. Fans tolerated the deal but rejected the rename. Investors responded positively to the announcement. This divergence suggests sponsorship value is not contingent on universal fan approval — commercial signals can hold even where sentiment is mixed.

***

## Analysis 1 — Reddit Sentiment

**Notebook:** `spotify_barcelona_reddit_sentiment.ipynb`

### Data

- **328** raw comments scraped → **289** after cleaning
- **11 posts** across **3 subreddits**: r/Barca, r/soccer, r/football
- **226 unique authors** across all communities
- Split into two events: **195 comments** (Deal Announcement) and **94 comments** (Renaming Announcement)

| Community | Comments | Avg Upvotes | Tone |
|---|---|---|---|
| r/Barca | 113 | 13.8 | Most engaged — emotionally invested community |
| r/soccer | 116 | 3.0 | Most balanced — 20% positive, 20% negative |
| r/football | 60 | 10.5 | Most negative — 32% negative, only 8% positive |

### Pipeline

```
Scrape (PRAW) → Clean → RoBERTa Inference → Confidence Filter (≥60%) → Net Sentiment Comparison
```

1. **Scrape** — Reddit posts and comments collected via PRAW
2. **Clean** — Remove bots (AutoModerator, BotDefense), spam, and sub-10-character comments; fix encoding with `ftfy`
3. **Classify** — RoBERTa inference: `cardiffnlp/twitter-roberta-base-sentiment`
4. **Filter** — Apply 60% confidence threshold; retain high-confidence labels only
5. **Compare** — Net sentiment (% positive − % negative) per event

### Results

| | Deal Announcement | Renaming Announcement |
|---|---|---|
| Negative | 17% | 37% |
| Neutral | 40% | 36% |
| Positive | 18% | 6% |
| Uncertain | 25% | 20% |
| **Net sentiment (polar)** | **+1.4%** | **−70.8%** |

### Hypotheses

- **H1** — Average sentiment > 0 per event → **Not supported.** Neither event was net positive.
- **H2** — Renaming sentiment < Deal sentiment → **Confirmed emphatically.** Fans drew a clear line between accepting a jersey sponsor and renaming a 70-year-old stadium.

***

## Analysis 2 — Event Study

**Notebook:** `MA_spotify_barca_event_study.ipynb`

### Data

- Spotify daily stock returns + market index returns sourced from **WRDS**
- Event date: **March 15, 2022** (Barcelona deal announcement)

### Method

```
Retrieve WRDS Data → Merge Datasets → Estimate Market Model (OLS) → Calculate AR & CAR
```

- **Estimation window:** ~230 trading days before announcement (establishes expected return baseline)
- **Market Model:** `Spotify Return = α + β × Market Return` — OLS regression; β significant at p < 0.05
- **Event window:** 11 days (−5 to +5 around March 15, 2022)
- **Metrics:** Daily Abnormal Returns (AR) and Cumulative Abnormal Return (CAR)

> **Why this matters methodologically:** Spotify has a high beta, meaning it naturally amplifies market-wide moves. The OLS regression explicitly accounts for this by modelling the expected market-driven component — so any residual in the event window is more cleanly attributable to the announcement. Two benchmarks (value-weighted index and S&P 500) were used as a cross-validation check.

### Results

| Benchmark | CAR (11-day window) | Direction |
|---|---|---|
| Value-Weighted Market Index | **+2.21%** | Positive |
| S&P 500 | **+4.15%** | Positive |

Both models agree: the announcement coincided with positive abnormal returns. The market appeared to view the deal as value-creating in the short run.

### Hypotheses

- **H1** — Average AR ≠ 0 around event date → **Supported directionally**
- **H2** — CAR indicates a significant market reaction → **Supported directionally** (positive under both specifications; note that given Spotify's inherent volatility, results are best read as directional rather than statistically conclusive)

***

## Repository Structure

```
spotify-barcelona-sponsorship-analysis/
├── spotify_barcelona_reddit_sentiment.ipynb   # Sentiment analysis pipeline
├── MA_spotify_barca_event_study.ipynb         # Event study pipeline
├── images/
│   ├── sentiment_distribution.png             # Sentiment by event (Deal vs. Rename)
│   ├── net_sentiment_comparison.png           # Net sentiment: +1.4% vs −70.8%
│   ├── sentiment_by_subreddit.png             # Sentiment breakdown per community
│   ├── car_value_weighted.png                 # CAR chart – value-weighted model
│   └── car_sp500.png                          # CAR chart – S&P 500 model
└── README.md
```

***

## Tech Stack

| Layer | Tools |
|---|---|
| Data collection | PRAW (Reddit API), WRDS |
| Data cleaning | `pandas`, `ftfy` |
| Sentiment model | `transformers` — RoBERTa (`cardiffnlp/twitter-roberta-base-sentiment`) |
| Statistical modelling | `numpy`, `scipy`, `statsmodels` |
| Visualisation | `matplotlib`, `seaborn` |
| Environment | Google Colab |

***

## Limitations

- Reddit comments represent vocal online communities, not the broader fan or consumer base
- RoBERTa may misclassify football-specific slang, mixed-language comments, or irony
- The 60% confidence threshold retains 76.8% of comments — the excluded 23.2% may not be randomly distributed
- The event study covers a short 11-day window; long-term brand equity effects are not captured
- Spotify's high beta coefficient increases noise in abnormal return estimates, making statistical significance harder to establish cleanly in a short window
- CAR positivity is directional; no claim is made about causal attribution beyond the announcement window

***

## About

Marketing analytics project combining NLP and financial event study methods.  
Tools: Python, Google Colab, WRDS.
