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

## `vcp_screener.pine`

Pine Screener port of the [VCP Detector](../indicators/vcp/README.md) (`indicators/vcp/vcp_detector.pine`). Finds stocks currently sitting inside a Volatility Contraction Pattern and stocks that just broke out of one. Designed to be run against an index universe (S&P 500, Nasdaq 100, Russell 2000, ...) or a watchlist.

The swing detection, contraction sequencing, base consumption and breakout rules are identical to the indicator. Differences from the indicator:

- All parameters are fixed constants (see the top of the script) set to the indicator defaults, so every scan uses the same VCP definition; tune the results with column filters instead
- The optional "Require higher lows" check is not included (it is off by default in the indicator)
- Drawing objects (boxes, labels, pivot line) are removed
- The trend template is not a hard gate; it is exposed as a separate filter and a score column so it can be combined freely
- Only the last 500 bars are available to the screener, which is enough for the 252-bar 52-week range and the 200-day SMA

### Fixed Parameters

| Parameter | Value | Meaning |
|---|---|---|
| Pivot left / right | 5 / 3 | Bars required on each side to confirm a swing high/low |
| Min / max contractions | 2 / 6 | Number of contractions (T) accepted in a sequence |
| Max first contraction depth | 35% | Depth limit for T1 |
| Max final contraction depth | 10% | Depth limit for the last contraction |
| Contraction ratio | 0.75 | Each contraction must be no deeper than 75% of the previous one |
| Max pattern age | 40 bars | Bars allowed since the final swing low |
| Base consumption threshold | 5% | A close this far above the pivot ends the base |
| Volume dry-up ratio | 0.7 | 5-day average volume must be below 70% of the 50-day average |
| Breakout volume multiplier | 1.5 | Breakout bar volume must exceed 1.5x the 50-day average |
| Recent breakout window | 5 bars | Window used by the "Last 5 Bars" filter |

### Screener Columns (Plots)

Structure columns describe the live contracting sequence (price still inside the final contraction and the pattern not stale) **before** the depth and volume rules are applied, so you can tune the thresholds with column filters. They are empty (`T Count` = 0) when no live sequence exists or the base was consumed by a breakout.

- **T Count**: Number of contractions in the current sequence. Filter with `T Count >= 3` to require three or more contractions; 0 means no live sequence
- **T1 Depth %**: Depth of the first contraction
- **Final T Depth %**: Depth of the most recent contraction
- **% Below Pivot**: Distance from the close up to the pivot (buy point). Small values mean price is close to the buy point
- **Base Length (bars)**: Bars from the first contraction high to the current bar
- **Bars Since Final Low**: Bars since the most recent swing low
- **Vol Dry-up Ratio (5d/50d)**: 5-day average volume divided by 50-day average volume (lower = drier)
- **Vol Ratio (Today/50d)**: Current bar volume divided by 50-day average volume (breakout confirmation)
- **TT Score**: Minervini Trend Template score (0-8), same definition as `minervini_trend_template.pine`
- **Bars Since Breakout**: Bars since the last volume-confirmed VCP breakout (empty if none in the loaded history)

### Alert Conditions for Screening

- **VCP Active**: At least 2 contractions, T1 <= 35%, final T <= 10%, price inside the final contraction, pattern not stale, and 5-day volume dried up below 70% of the 50-day average
- **VCP Active (Trend OK)**: VCP Active plus price > 50 SMA > 150 SMA > 200 SMA with a rising 200 SMA (the indicator's default trend filter)
- **VCP Breakout Today**: Close above the pivot with volume >= 1.5x the 50-day average on the bar right after an active VCP
- **VCP Breakout (Last 5 Bars)**: A VCP breakout occurred within the last 5 bars

### Usage in TradingView Screener

1. Add this indicator to your chart and save it
2. Open the Screener tab, choose Pine Screener and select an index (e.g. S&P 500) or a watchlist as the symbol source
3. Choose this indicator from the dropdown and set the timeframe to Daily
4. Typical filter combinations:
   - `VCP Active (Trend OK) = True` and `T Count >= 3` to find tight multi-contraction bases in a Stage 2 uptrend
   - `VCP Active = True` and `% Below Pivot <= 3` to find bases about to test the buy point
   - `VCP Breakout (Last 5 Bars) = True` and `TT Score >= 7` to review recent breakouts
   - Sort by `Final T Depth %` ascending to rank the tightest final contractions
5. Run the scan

### Notes

- Use the **Daily (D)** timeframe. The volume averages and the 40-bar pattern age are calibrated for daily bars
- During market hours the last bar is still forming, so `Vol Ratio (Today/50d)` and the breakout filters reflect partial volume until the close

## `mvp_screener.pine`

Pine Screener for David Ryan's MVP indicator. Finds stocks showing strong accumulation over the last 15 trading days: persistent up closes (Momentum), rising volume (Volume) and a sharp price gain (Price).

### The 3 Conditions

| Condition | Rule |
|---|---|
| **M** (Momentum) | At least 12 of the last 15 bars closed higher than the previous close |
| **V** (Volume) | The 15-day average volume is at least 25% above the 50-day average volume measured right before the 15-day window |
| **P** (Price) | The close is at least 20% above the close 15 bars ago (the close right before the window) |

The volume baseline is `ta.sma(volume, 50)[15]`, so it covers the 50 bars before the 15-day window and is not inflated by the window itself.

### Screener Columns (Plots)

- **MVP**: 1 when all three conditions are met, otherwise 0. Green=1, Gray=0
- **M: Up Days (15)**: Number of up closes in the last 15 bars (threshold: 12)
- **V: Volume Increase %**: 15-day average volume vs the prior 50-day average, in % (threshold: 25)
- **P: 15D Gain %**: Price change over the last 15 bars, in % (threshold: 20)

### Alert Conditions for Screening

- **MVP**: All three conditions met
- **M: 12+ Up Days**: Momentum condition only
- **V: Volume +25%**: Volume condition only
- **P: Price +20%**: Price condition only

### Usage in TradingView Screener

1. Add this indicator to your chart and save it
2. Open the Screener tab, choose Pine Screener and select an index or a watchlist as the symbol source
3. Choose this indicator from the dropdown and set the timeframe to Daily
4. Typical filter combinations:
   - `MVP = 1` to find stocks meeting all three conditions
   - `M: Up Days (15) >= 12` and `V: Volume Increase % >= 25` to find accumulation that has not yet produced a 20% gain
   - Sort by `P: 15D Gain %` descending to rank the strongest movers
5. Run the scan

### Notes

- Use the **Daily (D)** timeframe. The 15-bar window and the 50-day volume baseline are calibrated for daily bars
- The script needs 66 bars of history (50 + 15 + 1), well within the screener's 500-bar limit
- During market hours the last bar is still forming, so the volume and up-day counts reflect a partial day until the close
