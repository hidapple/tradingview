# Moving Average Indicators

## `simple_moving_average.pine`

### Weekly time frame
- Gold: 10-period Simple Moving Average (represents ~10 weeks of price action)
- Orange: 30-period Simple Moving Average (represents ~7 months of price action)

### Other time frames
- Cyan: 10-period SMA (short-term trend)
- Pink: 21-period SMA (short to medium-term trend)
- Blue: 50-period SMA (medium-term trend)
- Red: 200-period SMA (long-term trend)

### 52-week High/Low Labels
- When a new 52-week high is made, the price is displayed in green above the candlestick
- When a new 52-week low is made, the price is displayed in red below the candlestick
- Lookback period adjusts by timeframe: 52 bars (weekly), 12 bars (monthly), 252 bars (daily/other)