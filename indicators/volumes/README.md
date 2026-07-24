# Volumes Indicator

## `volume.pine`

Displays volume bars with colors based on both volume trend and candle direction. On the daily timeframe it also projects where today's volume is likely to finish.

### Daily time frame
- **Solid green/red**: Volume > 2x (5-day average) - strong volume
- **Light green/red**: Volume > 1.4x (5-day average) - elevated volume
- **Faded green/red**: Volume < 0.5x (5-day average) - low volume
- **Gray**: Volume in normal range
- **Gold line**: 50-day moving average of volume
- **Gray ghost bar** (translucent column behind today's bar, live sessions only): projected end-of-day volume

### End-of-day volume projection (daily only)
While the market is open, the developing daily bar is colored by its **projected** end-of-day volume instead of the volume traded so far, so unusually heavy days are flagged early in the session.

- Projection formula: `projected = cumulative volume so far / p(t)`, where `p(t)` is the average fraction of a session's total volume traded by the same intraday position over the last 30 sessions
- The profile is built from 30-minute intraday data via `request.security_lower_tf()`, so the U-shaped intraday volume curve is reflected (no linear extrapolation bias)
- Requires at least 5 recorded sessions and roughly 30 minutes of elapsed session time before the first projection appears
- On historical bars and closed sessions the indicator behaves exactly as before (actual volume only)

### Weekly time frame
- **Solid green/red**: Volume > 2x (4-week average) - strong volume
- **Light green/red**: Volume > 1.4x (4-week average) - elevated volume
- **Faded green/red**: Volume < 0.5x (4-week average) - low volume
- **Gray**: Volume in normal range
- **Gold line**: 10-week moving average of volume

### Other timeframes
- **Green**: Bullish candle (close ≥ open)
- **Red**: Bearish candle (close < open)