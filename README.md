# 🏡 Seattle Airbnb – ML-Powered Best Value Finder

A machine learning web application that helps users discover the **best-value Airbnb listings in Seattle** using a hybrid scoring system combining a trained CatBoost model with expert heuristics.

🔗 **Live App:** [seattle-airbnb-ai.streamlit.app](https://seattle-airbnb-ai.streamlit.app)

---

## Overview

This app goes beyond basic filtering. Instead of just sorting by price or reviews, it trains a **CatBoost regression model** at runtime to predict booking likelihood for each listing, then blends that ML signal with a rule-based heuristic score to surface listings that are genuinely worth booking.

**Final Score = 70% ML (CatBoost) + 30% Expert Heuristic**

---

## Features

- **CatBoost ML Model** — Trained on an 80/20 split at app startup; predicts booking likelihood from price, location, reviews, room type, availability, and more
- **Hybrid Scoring** — Combines ML predictions with weighted heuristics (price, proximity, reviews, room type, availability penalty)
- **Interactive Filters** — Budget, minimum reviews, distance to downtown, neighborhood, and room type
- **Live Model Metrics** — R² and RMSE displayed in the UI so you can evaluate model quality yourself
- **Clustered Map** — Folium map with color-coded markers (red → high score, orange → mid, blue → lower) and clustering for dense areas
- **Haversine Distance** — Computes real geodesic distance from each listing to Seattle downtown (47.6062°N, 122.3321°W)

---

## Tech Stack

| Layer | Tools |
|---|---|
| Frontend / App | Streamlit |
| ML Model | CatBoost (`CatBoostRegressor`) |
| Data Processing | Pandas, NumPy, Scikit-learn |
| Mapping | Folium, streamlit-folium |
| Data Source | Seattle Airbnb listings (`listings1.csv`) — November 2025 |

---

## How the Scoring Works

### ML Score
CatBoost is trained to predict a **booking proxy** — a normalized ratio of recent reviews to availability — as a signal for how frequently a listing actually gets booked. The raw predictions are min-max scaled to a 0–100 range.

### Heuristic Score
A rule-based score weighted across five signals:

| Signal | Weight |
|---|---|
| Price (lower is better) | 30% |
| Number of reviews | 25% |
| Distance to downtown | 20% |
| Room type bonus | 15% |
| Availability penalty | 10% |

### Final Smart Score
```
Smart Score = 0.7 × ML Score + 0.3 × Heuristic Score
```

---

## Getting Started

### Prerequisites
- Python 3.9+
- `listings1.csv`(Seattle Airbnb dataset)

### Installation

```bash
git clone https://github.com/chickopico/AIES-Project.git
cd AIES-Project
pip install -r requirements.txt
streamlit run app.py
```

### Required packages
```
streamlit
pandas
numpy
scikit-learn
catboost
folium
streamlit-folium
```

> **Note:** The CatBoost model trains at startup (~10–20 seconds on first load). Subsequent loads are cached via `@st.cache_resource`.

---

## Dataset

The app uses the **Seattle Airbnb listings dataset** (`listings1.csv`). Key fields used:

- `price`, `latitude`, `longitude`
- `room_type`, `neighbourhood_group`, `neighbourhood`
- `number_of_reviews`, `reviews_per_month`, `number_of_reviews_ltm`
- `availability_365`, `minimum_nights`
- `calculated_host_listings_count`

Price is cleaned (stripped of `$`/`,`), filtered to the `$30–$1500` range, and capped at the 99th percentile.

---

## Project Structure

```
AIES-Project/
├── app.py              # Main Streamlit application
├── listings1.csv       # Seattle Airbnb dataset 
├── requirements.txt    # Python dependencies
└── README.md
```


