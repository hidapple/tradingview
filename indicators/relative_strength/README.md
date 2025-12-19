# Relative Strength Indicator

## `rs_sp500.pine`

### Calculation Method
- Raw relative strength: Current symbol close price / SPY close price
- Baseline: 100-day simple moving average of raw relative strength
- Normalized RS: ((Raw RS / Baseline) - 1) × 100

### Display Lines
- White: Current normalized relative strength (%)
- Cyan: 10-day SMA of normalized RS (short-term trend)
- Pink: 21-day SMA of normalized RS (medium-term trend)
- Blue: 50-day SMA of normalized RS (long-term trend)
- Gray dashed: 0% neutral line (baseline level)

### Interpretation
- Above 0%: Outperforming S&P 500 compared to 100-day average
- Below 0%: Underperforming S&P 500 compared to 100-day average
- Moving average crossovers: Trend change signals
- Distance from 0%: Strength of relative performance

