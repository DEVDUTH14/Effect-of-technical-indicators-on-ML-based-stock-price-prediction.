# 📈 LSTM-Based Stock Prediction with Technical Indicators (Banking Sector)

**Goal:** Compare two engineered sets of technical indicators using **LSTM** to identify which combination best predicts stock prices for major **Indian and international banks**.

## 🚀 Impact

- Delivered **model-backed guidance** on which indicators work best for banking stocks, instead of relying on intuition or generic recipes.
- Showed that **RSI + SMA + MACD + Stochastic Oscillator (Set 1)** generally outperform **ADX + ATR + OBV + Bollinger Bands (Set 2)** on MSE and MAE across HDFC, SBI, JPM, and BAC.
- Demonstrated that **LSTM + carefully curated indicators** significantly improves short-term prediction quality, supporting better entries, exits, and risk management.

## 🧾 Assets & Scope

- **Instruments:**
  - India: `HDFC`, `SBI`
  - International: `JPM`, `BAC`
- **Data source:** Daily OHLCV from **Yahoo Finance** (~1022 records per bank).
- **Task:** Next-step stock price prediction for each bank under two feature configurations (indicator sets).

## 🧪 Indicator Engineering

**Indicator Set 1 (Momentum & Trend)**  
- `RSI` (14-period) – overbought/oversold signal  
- `SMA` – trend smoothing  
- `MACD` (12 EMA – 26 EMA + signal line) – momentum shift detector  
- `Stochastic Oscillator` (%K, %D) – price vs 14-day high/low range  

**Indicator Set 2 (Trend Strength, Volatility & Volume)**  
- `ADX` (14-period) – trend strength  
- `ATR` (14-period) – volatility  
- `OBV` – volume-based momentum  
- `Bollinger Bands` (upper, middle, lower) – dynamic support/resistance and volatility  

Each bank gets **two datasets**: one with Set 1 features and one with Set 2.

## 🧹 Data Preparation

- **Raw features:** `DATE`, `OPEN`, `HIGH`, `LOW`, `CLOSE`, `ADJ CLOSE`, `VOLUME`.
- **Feature extraction:** Computed all 8 indicators and grouped them into Set 1 and Set 2.
- **Outlier handling:**
  - Dropped initial rows where indicators requiring a 14-period history were undefined.
  - Applied **Z-score filtering (|z| > 3)** to remove extreme anomalies.
- **Scaling:** Min–max scaling to `[0, 1]` across all numerical features to stabilize LSTM training.

## 🧠 Modeling (LSTM)

- **Model:** Long Short-Term Memory (LSTM) network for univariate price prediction with multivariate inputs (price + indicators).
- **Per-bank, per-set models:** Separate LSTM models for:
  - HDFC – Set 1 vs Set 2  
  - SBI – Set 1 vs Set 2  
  - JPM – Set 1 vs Set 2  
  - BAC – Set 1 vs Set 2
- **Evaluation metrics:**
  - `MSE` (Mean Squared Error)
  - `MAE` (Mean Absolute Error)
  - `Accuracy` / directional correctness (% of times movement direction was predicted correctly)

## 📊 Key Results (Summary)

- **HDFC:**
  - Set 1 → Lower `MSE` and `MAE` (better error profile)
  - Set 2 → Slightly higher accuracy (better direction hit-rate)
- **SBI:**
  - Set 1 → Lower `MSE` and `MAE`
  - Set 2 → Higher accuracy
- **JPM:**
  - Set 1 → Much lower `MSE` and `MAE` **and** higher accuracy → clear winner
- **BAC:**
  - Set 1 → Substantially lower `MSE` and `MAE` and higher accuracy than Set 2

**Overall:**  
- `Set 1 (RSI, SMA, MACD, Stochastic)` is the **default best choice** for banking stocks, offering a consistently better balance of low error and high accuracy.  
- `Set 2` can still be useful in specific contexts (e.g., HDFC’s marginally better directional accuracy), highlighting that **indicator choice should consider bank-specific behavior and regime**.

## 🧭 Takeaways

- LSTM’s ability to model **long-term dependencies in financial time series** makes it a strong backbone for indicator-based forecasting.
- Carefully engineered indicator sets matter as much as the model: **“good features + good model”** beats blindly throwing all indicators at the network.
- This framework can be extended to:
  - Other sectors (IT, energy, FMCG)
  - Additional indicator sets (volume-only, volatility-heavy, etc.)
  - Multi-step forecasting or classification (up/down) tasks.
