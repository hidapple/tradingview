# Volumes Indicator

## `volume.pine`

Displays volume bars with colors based on both volume trend and candle direction.

### Daily time frame
- **Solid green**: Volume > 2x (5-day average) with bullish candle
- **Solid red**: Volume > 2x (5-day average) with bearish candle
- **Transparent green**: Volume < 0.5x (5-day average) with bullish candle
- **Transparent red**: Volume < 0.5x (5-day average) with bearish candle
- **Gray**: Volume in normal range

### Weekly time frame
- **Solid green**: Volume > 2x (4-week average) with bullish candle
- **Solid red**: Volume > 2x (4-week average) with bearish candle
- **Transparent green**: Volume < 0.5x (4-week average) with bullish candle
- **Transparent red**: Volume < 0.5x (4-week average) with bearish candle
- **Gray**: Volume in normal range
- **Blue line**: 10-week moving average of volume

### Other timeframes
- **Green**: Bullish candle (close ≥ open)
- **Red**: Bearish candle (close < open)