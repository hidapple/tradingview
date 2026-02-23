# Screeners

Market screening scripts for filtering stocks and assets based on specific criteria.

## `minervini_trend_template.pine`

Pine Script screener based on Mark Minervini's Trend Template from "Trade Like a Stock Market Wizard". Identifies stocks in a confirmed Stage 2 uptrend by checking 8 criteria.

### The 8 Conditions

1. Price > 150-day SMA
2. Price > 200-day SMA
3. 150-day SMA > 200-day SMA
4. 200-day SMA trending up (vs 20 trading days ago)
5. 50-day SMA > both 150-day and 200-day SMA
6. Price > 50-day SMA
7. Price is at least 25% above 52-week low
8. Price is within 25% of 52-week high

### Screener Columns (Plots)

- **TT Score**: Number of conditions met (0-8). Green=8, Orange=6-7, Red=<6

### Screener Columns (Plots) - Individual Conditions

Each condition is plotted as a numeric value (%) so you can sort and filter by actual values:

- **1: Price vs 150 SMA %**: Distance from 150-day SMA (positive = above)
- **2: Price vs 200 SMA %**: Distance from 200-day SMA (positive = above)
- **3: 150 vs 200 SMA %**: Gap between 150-day and 200-day SMA (positive = 150 above)
- **4: 200 SMA 1M Chg %**: 200-day SMA change over 20 trading days (positive = trending up)
- **5: 50 vs 150 SMA %**: Gap between 50-day and 150-day SMA (positive = 50 above)
- **6: Price vs 50 SMA %**: Distance from 50-day SMA (positive = above)
- **7: % Above 52W Low**: Distance above 52-week low (threshold: 25%)
- **8: % From 52W High**: Distance below 52-week high (threshold: 25%)

### Alert Conditions for Screening

- **All 8 Conditions Met**: Full Trend Template pass (score = 8)
- **7+ Conditions Met**: Near-qualifying stocks (score >= 7)

### Usage in TradingView Screener

1. Add this indicator to your chart
2. Go to the Screener tab in TradingView
3. Select your watchlist or market
4. Choose this indicator from the dropdown
5. Set filters based on plot values or alert conditions:
   - Filter by "TT Score = 8" to find stocks passing all conditions
   - Filter by "TT Score >= 7" to include near-qualifying stocks
   - Sort by "% From 52W High" to find stocks closest to new highs
6. Run the scan to find Stage 2 uptrend candidates

### Recommended Timeframe

Use on **Daily (D)** timeframe as the conditions are based on daily moving averages (50, 150, 200-day SMA).
