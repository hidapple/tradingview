# Volumes Indicator

## `volume.pine`

Displays volume bars with colors based on both volume trend and candle direction.

### Daily time frame
- **Solid green/red**: Volume > 2x (5-day average) - strong volume
- **Light green/red**: Volume > 1.4x (5-day average) - elevated volume
- **Faded green/red**: Volume < 0.5x (5-day average) - low volume
- **Gray**: Volume in normal range
- **Gold line**: 50-day moving average of volume

### Weekly time frame
- **Solid green/red**: Volume > 2x (4-week average) - strong volume
- **Light green/red**: Volume > 1.4x (4-week average) - elevated volume
- **Faded green/red**: Volume < 0.5x (4-week average) - low volume
- **Gray**: Volume in normal range
- **Gold line**: 10-week moving average of volume

### Other timeframes
- **Green**: Bullish candle (close ≥ open)
- **Red**: Bearish candle (close < open)