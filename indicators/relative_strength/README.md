# Relative Strength Indicator

## `rs_sp500.pine`

### Calculation Method
- Relative strength ratio: Current symbol close / SPY close
- The raw ratio is plotted directly (no normalization or smoothing)

### Display
- Green: Ratio is rising (outperforming S&P 500)
- Red: Ratio is falling (underperforming S&P 500)
- Blue Dot (circle): RS line at new 52-week high while stock price is NOT at new 52-week high
- Orange Triangle: RS line at new 52-week high while stock price is also at new 52-week high

### Interpretation
- Line trending up: Stock is outperforming S&P 500
- Line trending down: Stock is underperforming S&P 500
- The absolute value represents the price ratio between the stock and SPY
- Blue Dot signals that relative strength is leading price — a bullish sign that the stock may be about to break out to new highs
- Green Triangle confirms both RS and price are at new highs — the stock is in a strong uptrend with market-leading relative strength

