# Power BI Bitcoin (BTC) Analysis Dashboard

This repository contains the details for a comprehensive Bitcoin (BTC) price analysis dashboard created in **Microsoft Power BI**. The dashboard provides a high-level overview of Bitcoin's market performance, key metrics, and historical trends from **2015 to 2024**.


## 📝 Project Overview

The primary goal of this project is to consolidate complex historical BTC data into an intuitive and interactive visual report. It serves as a powerful tool for analysts, traders, and crypto enthusiasts to quickly grasp market sentiment, price action, and significant trends over the past decade. The dashboard focuses on clarity and key performance indicators (KPIs) to facilitate data-driven insights.

---

## 📊 Key Metrics & Features

The dashboard highlights four crucial KPIs at the top for an immediate summary:

* **All-Time High (ATH):** The highest recorded price of Bitcoin in the dataset.
* **Total Return:** The total multiplicative return on investment from the earliest date in the dataset to the latest. A value of `216` signifies a `216x` or `21,600%` return.
* **Lastest Price:** The most recent closing price available in the dataset.
* **Overbought Days:** The total count of days where the 14-day Relative Strength Index (RSI) was above 70, indicating periods of potential market saturation or "buy-side" hype.

---

## 📈 Visualizations Breakdown

The dashboard is composed of five distinct visualizations designed to explore the data from different angles.

| Visualization                             | Description                                                                                                                                                           | Insights Gained                                                                          |
| ----------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------- |
| **Sum of rsi\_14 by Year** (Line Chart)     | Aggregates the daily RSI-14 values for each year.                                                                                                                     | Identifies years with prolonged periods of high/low momentum and market sentiment.       |
| **Moving Averages by Year** (Column Chart)  | Compares the sum of the daily close price, 50-day EMA, and 100-day SMA on a yearly basis.                                                                               | Shows the magnitude of price trends and the strength of bullish/bearish years.           |
| **Lastest Price and All-Time High** (Gauge) | A simple gauge that visually represents the current price in relation to its all-time high.                                                                           | Provides an instant understanding of the current market position relative to its peak.   |
| **Price and Volume by Year** (Combo Chart)  | Displays the sum of trading volume (columns) against the sum of the closing price (line) for each year.                                                               | Helps analyze the relationship between price movements and trading activity over time.   |
| **Data Bloom** (Slicer)                     | An interactive year slicer that allows the user to filter the entire dashboard report for a specific year (2015-2024).                                                 | Enables focused analysis on a particular period.                                         |

---

## 🛠️ Technical Details & DAX Measures

This dashboard leverages several custom DAX (Data Analysis Expressions) measures to calculate the key metrics. Below are the core formulas used.

### **All-Time High**

Calculates the maximum value in the `high` column across the entire table.

    All-Time High = MAXX('btc_2015_2024', 'btc_2015_2024'[high])

### **Lastest Price**

Finds the closing price corresponding to the most recent date in the dataset.

    Lastest Price = 
    CALCULATE(
        SUM('btc_2015_2024'[close]),
        LASTNONBLANK('btc_2015_2024'[date], 1)
    )

### **Overbought Days**

Counts the number of rows where the `rsi_14` value is greater than 70.

    Overbought Days = COUNTROWS(FILTER('btc_2015_2024', 'btc_2015_2024'[rsi_14] > 70))

### **Total Return**

Calculates the total investment return as a multiplier. It finds the first and last prices and computes the relative difference.

    Total Return = 
    VAR FirstPrice = CALCULATE(
        SUM('btc_2015_2024'[close]),
        FIRSTDATE('btc_2015_2024'[date])
    )
    VAR LastPrice = [Lastest Price]
    RETURN
        DIVIDE(LastPrice - FirstPrice, FirstPrice)

### **BG Transparent**

A simple utility measure used for aesthetic purposes, such as creating transparent backgrounds for visuals to blend them with the main background.

    BG Transparent = "rgba(255, 255, 255, 0)"

---

### Data Source

The analysis is based on a BTC dataset covering the period from **January 2015 to early 2024**. The dataset includes daily `open`, `high`, `low`, `close`, `volume`, and pre-calculated `rsi_14`, `ema_50`, and `sma_100` values.
