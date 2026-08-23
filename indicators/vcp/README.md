# VCP (Volatility Contraction Pattern)

Indicators for detecting Mark Minervini's Volatility Contraction Pattern from "Trade Like a Stock Market Wizard".

## `vcp_detector.pine`

Overlay indicator that detects VCP bases: a series of price contractions that progressively shrink from left to right, accompanied by a volume dry-up, forming during a Stage 2 uptrend.

### Detection Logic

1. **Swing detection**: Confirmed swing highs/lows via `ta.pivothigh()` / `ta.pivotlow()` (default: 5 left / 3 right bars). Consecutive same-type pivots are merged, keeping the more extreme one.
2. **Contraction measurement**: Each swing high to the following swing low is one contraction (T). Depth is measured in % from the high.
3. **Contraction sequence**: Starting from the most recent contraction and walking backward, each contraction must be no deeper than 75% (configurable) of the previous one. Higher lows are NOT required by the VCP definition (shakeouts are allowed); an optional "Require higher lows" input (off by default) adds that strictness. Adjacent contractions must also overlap in price (the later contraction's low must be below the earlier contraction's high) — a pullback formed well above the previous contraction belongs to a new structure and never chains onto the old base. The sequence qualifies when:
   - At least 2 contractions (max 6 considered)
   - First contraction depth <= 35%
   - Final contraction depth <= 10%
4. **Volume dry-up**: 5-day average volume <= 70% of 50-day average volume.
5. **Trend filter** (optional, on by default): price > 50 SMA > 150 SMA > 200 SMA, with the 200 SMA rising vs 20 bars ago (simplified Minervini Trend Template).
6. **Pattern validity**: price must still sit inside the final contraction (below the pivot, above the final low), and the final swing low must be within 40 bars.
7. **Breakout consumes the base**: the base is consumed by a volume-confirmed breakout, or by a close more than 5% (configurable) above the pivot. Contractions that started at or before that bar are permanently excluded from future sequences: the T-count resets, and only a fresh base formed after the breakout can qualify as the next VCP. A brief poke above the pivot within the 5% tolerance is treated as a failed test of the highs — if price falls back, the pattern continues and the poke's swing high becomes the next contraction's start.

### Visual Characteristics

- **Orange background**: VCP is currently active (all conditions met)
- **Translucent blue boxes** (last bar only): for each contraction, a borderless box spanning from the high bar to the low bar vertically bounded by the swing high and swing low levels
- **Blue text** (last bar only): plain text (no balloon) at each contraction low showing the contraction number and depth together, e.g. "T1(10.5%)" (toggle via "Show contraction labels"). The pivot price is likewise shown as plain green text
- **Green dashed line**: the pivot (buy point) = high of the final contraction, extended to the right with a "Pivot" price label
- **Green triangle with "BO"**: breakout bar — close crosses above the pivot with volume >= 1.5x the 50-day average, immediately following an active VCP

### Alerts

- **VCP Detected**: fires when a VCP becomes active
- **VCP Breakout**: fires on a volume-backed close above the pivot

### Limitations

- Swing confirmation lags by the "Pivot right bars" setting (default 3 bars), so pattern recognition is delayed by design.
- VCP is inherently a discretionary pattern; all thresholds (contraction ratio, depth limits, dry-up ratio, etc.) are exposed as inputs for tuning.
- Intended for daily charts. On other timeframes, adjust the pivot and age settings accordingly.
