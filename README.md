# Spotify x FC Barcelona Brand Impact Analysis

Analysis of consumer sentiment and market response to the Spotify x FC Barcelona partnership announcements.

## Overview

This repository examines how the Spotify–FC Barcelona partnership was received across two dimensions:

- **Public reaction**, measured through Reddit sentiment analysis
- **Market reaction**, measured through an event-study approach around key announcement dates

The project combines social-media text analysis with financial event analysis to evaluate whether brand partnerships can generate business impact even when fan sentiment is mixed.

## Research Focus

The analysis is built around three core questions:

1. How did online audiences react to the Spotify–FC Barcelona partnership announcements?
2. Did reactions differ between the initial deal announcement and the Camp Nou naming-rights announcement?
3. Did the announcement period show a measurable market response consistent with broader partnership impact?

## Methodology

### 1. Reddit Sentiment Analysis
The sentiment analysis workflow uses Reddit comments related to the Spotify–FC Barcelona partnership.

Key steps included:
- Collecting and organizing Reddit discussion data
- Cleaning comments by removing bots, spam, and low-information entries
- Fixing text encoding issues
- Running RoBERTa-based sentiment classification on cleaned comment data
- Comparing sentiment patterns across the two focal events

### 2. Event Study
The event-study workflow evaluates market reaction around the partnership announcement window.

Key steps included:
- Defining the event window around the announcement
- Measuring pre- and post-announcement price movement
- Interpreting whether investor response aligned with or diverged from public sentiment

## Key Findings

- The **deal announcement** generated broadly neutral sentiment overall.
- The **stadium naming-rights announcement** generated more negative sentiment.
- The market pattern showed a decline before the announcement and a recovery afterward.
- Taken together, the results suggest that brand partnerships can produce positive commercial signals even when fan reaction is uneven.

## Repository Contents

This repository contains the core project materials, including:

- Sentiment analysis notebooks
- Event-study notebooks
- Supporting project documentation
- Presentation materials

> Suggested key files:
> - `spotify_barcelona_reddit_sentiment.ipynb`
> - `MA_spotify_barca_event_study.ipynb`

## How to Use

### Option 1: Review the notebooks
Open the notebooks directly in GitHub or Google Colab to inspect the workflow, code, and outputs.

### Option 2: Reproduce the analysis
To reproduce the project, prepare the required datasets and run the notebooks in sequence:

1. Data preparation and cleaning
2. Reddit sentiment analysis
3. Event-study analysis
4. Interpretation of combined findings

## Tools and Libraries

Typical libraries used in this project include:

- Python
- pandas
- numpy
- matplotlib
- seaborn
- transformers
- scipy
- ftfy

## Business Takeaway

The project suggests that partnership effectiveness should not be judged only by whether audiences respond positively in public discussion. In this case, the evidence points to a more nuanced outcome: mixed fan sentiment, but signals of broader commercial and market relevance.

## Limitations

- Reddit comments reflect online discussion, not the full fan or customer base.
- Sentiment models can miss sarcasm, context, or football-specific language.
- Event-study results capture short-window market response rather than long-term brand equity.

## License

Add a license here if the repository will be shared publicly.
