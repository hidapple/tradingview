# Screeners

Market screening scripts for filtering stocks and assets based on specific criteria.

## `sp500_outperformance.pine`

Pine Script screener for identifying stocks outperforming the S&P 500 index.

### Features

- Compares stock performance against SPY (S&P 500 ETF) over multiple periods
- Calculates outperformance for 4, 13, 26, and 52-week periods
- Provides multiple plot columns for sorting and filtering

### Screener Columns (Plots)

- **4W Outperf %**: 4-week outperformance vs SPY
- **13W Outperf %**: 13-week (quarterly) outperformance vs SPY  
- **26W Outperf %**: 26-week (half-year) outperformance vs SPY
- **52W Outperf %**: 52-week (annual) outperformance vs SPY
- **52W Stock %**: Stock's absolute 52-week performance

### Alert Conditions for Screening

- **All Periods Outperforming**: Positive outperformance in all four periods
- **Strong Outperformance**: 13W > 5% AND 52W > 10% outperformance
- **52W Outperf > 20%**: Annual outperformance exceeds 20%

### Usage in TradingView Screener

1. Add this indicator to your chart
2. Go to the Screener tab in TradingView
3. Select your watchlist or market
4. Choose this indicator from the dropdown
5. Set filters based on plot values or alert conditions:
   - Filter by any outperformance column (e.g., "52W Outperf % > 10")
   - Use alert conditions for pre-defined screening criteria
6. Run the scan to find outperforming stocks

### Recommended Timeframe

Use on **Weekly (W)** timeframe for best results when screening for medium to long-term outperformers.