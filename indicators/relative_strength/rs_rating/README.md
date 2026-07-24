# RS Rating Indicator

## `rs_rating.pine`

IBD-style Relative Strength Rating (1-99) displayed in a separate pane.
Based on "RS Rating" by Fred6724, simplified and restyled for this repository.

### Improvements over the original

- Runs in its own pane (`overlay = false`) instead of being drawn over the price chart,
  so all price-scaling parameters of the original (approximate SPX value, offset %) are removed
- All inputs use `display = display.none`, keeping the indicator status line clean
  (no long `SP:SPX 4200 80 ...` argument list next to the title)
- Converted to Pine Script v6

### Calculation Method

- RS Line: Current symbol close / comparative symbol close (default `SP:SPX`), plotted as a raw ratio
- RS Score: IBD-style weighted 12-month performance relative to S&P 500, with the most
  recent quarter weighted double:
  `0.4 * perf(63d) + 0.2 * perf(126d) + 0.2 * perf(189d) + 0.2 * perf(252d)`
  computed for both the symbol and `SP:SPX`, then `score = stock / reference * 100`
- RS Rating: The score is mapped to a 1-99 percentile using 7 market-wide distribution
  thresholds (percentiles 98, 89, 69, 49, 29, 9, 1 of ~6,600 US stocks) published daily
  via Pine Seeds (`seed_fred6725_rs_rating:RSRATING`), with piecewise linear interpolation
  between thresholds
- For recent IPOs the look-back periods shrink to the available history to avoid na results

### Display

- Blue RS line (color configurable): rising = outperforming, falling = underperforming S&P 500
- Blue Dot (circle): RS line at new 52-week high while price is NOT at new high (RS leading price)
- Orange Triangle: RS line and price both at new 52-week highs
- Rating label just above the last point of the RS line showing the 1-99 rating
  (shows `RS` without a number when the rating is unavailable)
- `RS Rating` value is also available in the data window

### Notes

- The rating is only computed on the daily timeframe because the seed distribution data is daily
- In bar replay mode enable "Use fixed values for replay mode" and fill in the 7 threshold
  values from the `SEED_FRED6725_RS_RATING:RSRATING` ticker
