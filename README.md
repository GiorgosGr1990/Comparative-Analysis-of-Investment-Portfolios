# 📊 Comparative Analysis of Investment Portfolios  
<div align="center">
<img width="768" height="512" alt="header" src="https://github.com/user-attachments/assets/05560146-81a3-4c58-8821-3179a018663a" />

 </div>
 
---

## 📝 Introduction  
Welcome to my **Investment Portfolio Analysis**, where I’ve harnessed my data analytics skills to tackle a real-world challenge: helping investors navigate the **risk vs. return trade-off**.  

As an aspiring data professional eager to make an impact, I’ve built this project using **Python, SQL, and Power BI** to answer a key question:  

*👉 Which portfolio offers the best balance between growth and downside protection?*  

By analyzing **10 years of ETF data**, I’ve created a practical tool to deliver actionable insights—perfect for kickstarting a career in **data analysis** or **finance**.  

---

## 🎯 Purpose  
This project addresses a common investor dilemma by comparing portfolio strategies (e.g., **60/40, All Weather, Permanent, Golden Butterfly**) to identify the best balance of **growth and downside protection**.  

It demonstrates my ability to **process financial data** and provide meaningful insights, making it a strong showcase for **entry-level data analysis or finance roles**.  

---

## 🔎 Overview  
This project:  
- Fetches **~10 years of daily ETF data** (SPY, AGG, TLT, etc.) from Yahoo Finance with Python.  
- Calculates **returns, CAGR, volatility, Sharpe Ratio, max drawdowns**.  
- Rebalances portfolios every **21 trading days (~monthly)**.  
- Stores results in **CSV files** and a **SQLite database** (SQL queries included for demonstration).  
- Visualizes results in an **interactive Power BI dashboard**.  

📅 Example dataset covered: *September 18, 2015 → September 12, 2025*.  

---

## 📌 Questions Answered  
- What is the **Compound Annual Growth Rate (CAGR)** for each strategy over 10 years?  
- Which portfolio has the **highest Sharpe Ratio** (risk-adjusted return, 1% risk-free)?  
- What is the **maximum drawdown** for each strategy compared to SPY?  
- How do **diversified portfolios** balance growth vs resilience?  

---

## 📊 Portfolio Intents & Distinctions  

### 🔹 Summary Table  

| Portfolio           | Intent                                   | Pitch                                  | Unique Angle                        | Target Audience                               |
|---------------------|------------------------------------------|----------------------------------------|--------------------------------------|-----------------------------------------------|
| **60/40**           | Balance growth & stability               | Classic “default” retirement strategy   | Trust in U.S. growth + bonds         | Mainstream investors                          |
| **All-Weather**     | Thrive in all economic climates          | Macro-hedged portfolio                  | Designed for “economic seasons”       | Investors wary of macro surprises             |
| **Permanent**       | Wealth preservation via balance          | “Sleep-well-at-night” simplicity        | Equal allocation, psychological safety| Risk-averse, hands-off investors              |
| **Golden Butterfly**| Blend resilience with growth             | Permanent Portfolio + equity exposure   | Balanced yet ambitious evolution      | Long-term investors seeking growth + safety   |
| **SPY**             | Pure growth, high volatility             | Bet on American capitalism              | Benchmark, uncushioned upside         | Optimists, risk-tolerant investors            |

---

### 🔹 Detailed Descriptions  

<details>
<summary>📂 Expand to read portfolio details</summary>  

- **60/40 Portfolio**  
  - **Intent**: Balance of growth and stability.  
  - **Pitch**: The classic “default” portfolio. Simple to understand, widely used, and a baseline for most retirement strategies.  
  - **Unique Angle**: Trust in U.S. growth with bonds as stabilizers.  
  - **Target**: Mainstream investors who want straightforward exposure.  

- **All-Weather (Ray Dalio)**  
  - **Composition**: 30% stocks (SPY), 40% long bonds (TLT), 15% intermediate bonds (IEF), 7.5% commodities (DBC), 7.5% gold (GLD).  
  - **Intent**: Thrive across all economic environments (growth, recession, inflation, deflation).  
  - **Pitch**: A macro-hedged portfolio. Commodities and gold guard against inflation, stocks capture prosperity, bonds protect against slowdowns.  
  - **Unique Angle**: Actively engineered for “economic seasons,” more nuanced than just safety.  
  - **Target**: Investors who want protection from macro surprises and value broad diversification.  

- **Permanent Portfolio (Harry Browne)**  
  - **Composition**: 25% stocks (SPY), 25% long bonds (TLT), 25% gold (GLD), 25% cash/short bonds (BIL).  
  - **Intent**: Wealth preservation through radical balance.  
  - **Pitch**: The original “sleep-well-at-night” portfolio. Equal slices across four simple assets make it robust in any scenario without tinkering.  
  - **Unique Angle**: Maximal simplicity + psychological safety.  
  - **Target**: Risk-averse investors who want a true “set it and forget it” design.  

- **Golden Butterfly**  
  - **Composition**: 20% short-term bonds (SHY), 20% small-cap value (VBR), 20% large-cap (VV), 20% long bonds (TLT), 20% gold (GLD).  
  - **Intent**: Blend of safety and equity-driven growth.  
  - **Pitch**: A Permanent Portfolio with wings. Keeps the same resilience, but adds both small-cap and large-cap exposure for more long-term upside.  
  - **Unique Angle**: A “balanced but ambitious” version — not just safety, but also designed to outperform in growth periods.  
  - **Target**: Investors who like the Permanent Portfolio’s safety but don’t want to miss out on stock-driven wealth building.  

- **SPY (100% Equities)**  
  - **Intent**: Pure growth, pure volatility.  
  - **Pitch**: A bet on American capitalism. All upside and downside, no cushions.  
  - **Unique Angle**: The benchmark for everything else.  
  - **Target**: Optimists, risk-tolerant investors.  

</details>  

---

**✅ How they differ despite overlap**:  
- **All-Weather** = macro-hedged, designed to respond to economic forces.  
- **Permanent** = psychological and practical simplicity, equal parts defense & offense.  
- **Golden Butterfly** = evolution of Permanent, adds large + small-cap tilt for growth potential.  

---

## ⚙️ How I Worked to Find the Results  

### Step 1: Data Extraction and Processing (Python)  
- Used `yfinance` to download adjusted closing prices for ETFs.  
- Calculated daily returns and cumulative values with Pandas and NumPy.  

<details>
<summary>📜 Example data extraction snippet</summary>

```python
import yfinance as yf
import pandas as pd
from datetime import date, timedelta

tickers = ["SPY", "AGG", "TLT", "GLD", "DBC", "IEF", "SHY", "BIL", "VBR", "VV"]
start_date = date.today() - timedelta(days=365*10)
end_date = date.today()

data = yf.download(tickers, start=start_date, end=end_date, auto_adjust=True)["Close"]
df_daily_returns = data.pct_change()
```
</details> 

---

### Step 2: Portfolio Calculation and Metrics  
- Rebalanced portfolios every 21 trading days (approximately monthly) using a custom function.  
- Computed metrics: CAGR, Annual Volatility, Sharpe Ratio (risk-free rate = 1%), and Maximum Drawdown.

<details>
<summary>📜 Example CAGR Calculation</summary>

```python
for etf in df_compound_values.columns:
    wealth = df_compound_values[etf]
    total_return = wealth.iloc[-1] - 1
    days = (wealth.index[-1] - wealth.index[0]).days
    years = days / 365.25 
    CAGR = (wealth.iloc[-1]) ** (1/years) - 1
```
</details> 


---

### Step 3: Data Storage and Querying (SQL)  
- Stored results in CSV files and a SQLite database (with SQL queries to demonstrate my skills, not critical to the analysis).  
- Used SQL to extract sample insights.  

<details>
<summary>📜 Example SQL Query</summary>
  
```sql
-- Average CAGR
SELECT PRINTF('%.2f%%', AVG(CAGR) * 100) AS AVERAGE_CAGR
FROM portfolio_metrics
WHERE name IN ("SPY", "Golden_Butterfly", "60_40", "All_Weather", "Permanent");
```
</details>

---

### Step 4: Visualization (Power BI)  
- Imported CSV data into Power BI.  
- Built a data model with table relationships.  
- Created charts and visualizations using Power BI, with metrics pre-computed in Python, and used DAX for the interactive features I developed.  
- The dashboard features interactivity with clickable metrics, dynamically displaying the best and worst portfolios by selected metric, enhancing user engagement and insight.

&nbsp;

📊 <ins>**Power BI Data Model Screenshot:**</ins>

&nbsp;
<img width="1332" height="742" alt="Project Dashboard" src="https://github.com/user-attachments/assets/50a60190-b3f1-4196-91dc-8f4a5c137786" />  

&nbsp;

📊 <ins> **Model view Screenshot:**</ins> 

&nbsp;
<img width="1708" height="802" alt="DATA MODEL" src="https://github.com/user-attachments/assets/130c13da-6f0d-4bc2-b647-8c0b2aa98820" />

&nbsp;

<details>
 
<summary>📜 Example DAX Measure (Best Portfolio):</summary>

```dax
  Best SelectedMetric = 
VAR SelectedMetric = SELECTEDVALUE('Metric Selector'[Metric])
VAR BestValue = 
    SWITCH(
        SelectedMetric,
        "CAGR", MAX('portfolio_metrics'[CAGR]),
        "Max Drawdown", MAX('portfolio_metrics'[MaxDrawdown]),
        "Sharpe Ratio", MAX('portfolio_metrics'[Sharpe]),
        "Volatility", MIN('portfolio_metrics'[Annual Volatility])
    )
RETURN BestValue
```
</details>

---

## 📈 Findings  

Based on a sample run from **September 18, 2015 → September 12, 2025**  
*(note: dates will vary with current run as of September 18, 2025; results may shift in different periods but reflect general trends over this decade)*:  

- **Average CAGR**: **8.71%** across key portfolios, showing how diversification tempers SPY's high **14.81%** growth to more sustainable levels, though at the cost of upside in bull markets.  
- **Best Sharpe Ratio**: **Permanent Portfolio (0.91)** – fulfills its promise of wealth preservation with a **CAGR 7.52%**, **Volatility 7.17%**, and **Max Drawdown -17.51%**, delivering radical balance and efficiency.  
- **Worst Max Drawdown**: **SPY (-33.72%)** – aligns with its pure growth intent but exposes the **Volatility 18.09%** and **Sharpe 0.80** trade-off for uncushioned upside.  

**Diversification Benefit (judged against promises):**  
- **60/40**: Meets its balance of growth and stability with a **CAGR 9.89%**, **Sharpe 0.81**, **Volatility 11.17%**, and **Max Drawdown -20.91%**. Provides straightforward exposure but without superior adjusted returns over SPY.  
- **Permanent**: Excels in its *“set it and forget it”* simplicity, offering **resilient performance** across assets with minimal tinkering.  
- **Golden Butterfly**: Achieves its ambitious evolution of Permanent by boosting **CAGR 8.35%**, **Sharpe 0.85**, **Volatility 8.67%**, and **Max Drawdown -18.84%**. Enhances growth while preserving safety.  
- **All Weather**: Falls short of thriving in all environments, with a **CAGR 6.33%**, **Volatility 8.73%**, **Sharpe 0.63**, and **Max Drawdown -23.53%**—the second-worst after SPY—due to long-term bond (TLT) underperformance in this rate-sensitive era.  

**Key Insight**:  
**Permanent** and **Golden Butterfly** deliver on their promises of efficiency and balanced ambition, **outperforming in risk-adjusted terms**; **60/40** provides reliable stability akin to SPY but with less volatility; **All Weather's macro-hedge faltered**, highlighting bond risks—**diversification tempers extremes**, guiding investors to strategies matching their tolerance.  

---

## 🚀 Future Improvements  

- Experiment with **different rebalancing periods** (monthly, quarterly, yearly) to assess impact on results.  
- Analyze **correlation with SPY** and **beta** to assess diversification effectiveness.  
- Calculate **time to recover from drawdowns** for each portfolio.  
- Add **interactivity** to the Power BI dashboard, allowing users to select different starting and ending points to see how metrics (CAGR, Sharpe Ratio, etc.) change dynamically.  
- Optimize **Python code** for additional robustness and performance.  


---

## 🛠️ Tech Stack and Skills Demonstrated  
- **Python**: Data extraction (`yfinance`), processing (Pandas, NumPy), and metric computation – showcases programming and analytical skills.  
- **SQL**: Basic database management (SQLite) for demonstration – highlights query-writing ability.  
- **Power BI**: Data modeling, DAX for interactivity, dashboard design – demonstrates visualization expertise.  
- **Other Skills**: ETL processes, financial analysis, and clear communication – valuable for entry-level data analyst roles.  


This project showcases the **end-to-end data analysis pipeline**: from raw data extraction to business-ready insights.  

---

## 💻 How to See and Reproduce It  

- **View on GitHub**: Browse files directly—check `powerbi/` for screenshots, `notebooks/` for code, and `data/` for results. All files are publicly downloadable, but opening them requires compatible software (e.g., Power BI Desktop for `.pbix`, Jupyter for `.ipynb`).  

- **Reproduce Locally**:  
  1. Download the repo: Click “Code” > “Download ZIP.”  
  2. Install dependencies: Run `pip install pandas numpy yfinance sqlite3` in your Python environment.  
  3. Run the notebook: Open `portfolio_analysis.ipynb` in Google Colab (preferred for easy setup) or Jupyter Notebook and execute cells to generate data. Note that the start date will be approximately 10 years before today (e.g., around September 15, 2015, as of now) and the end date will be today, differing from the showcased run (September 15, 2015, to September 12, 2025).  
  4. View Power BI: Check `powerbi/` screenshots or import `powerbi/portfolio_analysis.pbix` (if uploaded) into Power BI Desktop.  



---
## 📌 Conclusion

This project demonstrates how **portfolio construction impacts the risk/return trade-off**.

While simplified, it highlights my ability to combine **Python (analysis), SQL (storage), and Power BI (visualization)** into an end-to-end workflow that answers a clear business question: _How do different strategies perform in terms of growth vs downside risk?_

* * *
## 📬 Contact  
- Email: [giorgos.grigoriou@yahoo.gr]  
- I’m excited to apply my skills to real-world challenges—let’s talk about how I can support your team!  

---

## 🙏 Acknowledgments  
Data sourced from Yahoo Finance via yfinance. Inspired by Ray Dalio’s All Weather and Harry Browne’s Permanent Portfolio strategies, with thanks to the open-source community.  

