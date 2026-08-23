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

### 52-week High/Low Levels
- The current 52-week high is always shown as a thin dotted translucent green line, anchored at the bar that set the high and extending to the right of the latest bar, with the price as green text at the right end
- The current 52-week low is shown the same way in red
- Both levels update as the rolling window moves, so they always reflect the actual current 52-week extremes
- Lookback period adjusts by timeframe: 52 bars (weekly), 12 bars (monthly), 252 bars (daily/other)