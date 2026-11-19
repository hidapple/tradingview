# Stage Analysis

Indicators based on Stan Weinstein's four-stage analysis from "Secrets for Profiting in Bull and Bear Markets".

## `stage_analysis.pine`

Overlay indicator that classifies the current stage (1: basing, 2: advancing, 3: topping, 4: declining) from the 30-week SMA and colors the chart background accordingly.

### Calculation Method

All values are calculated on weekly bars via `request.security(syminfo.tickerid, "W", ...)`, so the result is the same on daily and weekly charts. The in-progress week is included, so on realtime bars the stage can change intraweek (historical bars show the value confirmed at each week's close).

- **Stage MA**: 30-week SMA of close
- **MA slope (% per week)**: `(MA - MA[5]) / MA[5] * 100 / 5`
- **Slope zones**: rising > +0.2%/week, falling < -0.2%/week, flat in between

### Stage Transitions

Stage 1 and 3 end with a **range breakout**, Stage 2 and 4 end when the **MA slope** leaves the rising/falling zone or **price stays on the other side of the MA**.

- **Range**: highest weekly high and lowest weekly low since the current Stage 1/3 started. The confirmation weeks before the stage change belong to the range, and a new Stage 1 starts its range low from the lowest low of the last 10 weeks (Stage 3: range high from the highest high), so the bottom (top) of the prior trend is part of the base. A breakout is a weekly close outside the range of the prior weeks (wicks do not count).
- **Min base length**: a breakout counts only after the stage has lasted at least 3 weeks.
- **Upside volume**: an upside breakout week needs volume >= 1.5x the prior 10-week average volume. Downside breakouts need no volume. Symbols without volume data (e.g. indices) skip the volume check.

| From | To | Condition | Type |
|---|---|---|---|
| 1 | 2 | Close > range high, slope >= -0.2 and volume >= 1.5x average | Breakout |
| 1 | 2 | Close > MA and slope rising (fallback for advances without a volume breakout) | Slope |
| 1 | 4 | Close < range low and slope <= +0.2 (base fails, decline resumes) | Breakout |
| 2 | 3 | Slope leaves the rising zone, after it has been rising during this Stage 2 | Slope |
| 2 | 3 | Close < MA, after the slope has been rising during this Stage 2 (catches sharp drops while the MA still rises) | MA |
| 2 | 1 or 3 | Close < MA before the slope ever reached the rising zone (failed breakout, back to the base it came from) | MA |
| 2 | 4 | Close < MA and slope falling | Slope |
| 3 | 2 | Close > range high, slope >= -0.2 and volume >= 1.5x average (advance resumes) | Breakout |
| 3 | 2 | Close > MA and slope rising (fallback) | Slope |
| 3 | 4 | Close < range low and slope <= +0.2 | Breakout |
| 4 | 1 | Slope leaves the falling zone, after it has been falling during this Stage 4 | Slope |
| 4 | 1 | Close > MA, after the slope has been falling during this Stage 4 (catches sharp rallies while the MA still falls) | MA |
| 4 | 1 or 3 | Close > MA before the slope ever reached the falling zone (failed breakdown, back to the base it came from) | MA |
| 4 | 2 | Close > MA and slope rising | Slope |

- A single weekly close across the MA is treated as noise: Stage 2/4 end only when price stays across the MA for the confirmation period. Stage 1/3 last until price leaves their range (or the MA clearly turns up).
- **Failed breakout/breakdown**: returning to the base restores its range and age, extended by the failed move, instead of starting a new base.
- **Confirmation**: breakout transitions apply immediately. Slope and MA transitions apply only after the new stage's condition holds for 2 consecutive weeks.
- **Initialization**: once 52 weeks of data are available, close > MA with a rising slope starts as Stage 2 and close < MA with a falling slope as Stage 4. Otherwise the position in the 52-week range decides: upper half = Stage 3, lower half = Stage 1.

All thresholds are exposed as inputs.

### Visual Characteristics

- **Stage 1**: normal chart background (no color)
- **Stage 2**: light green background (`color.green`, 90% transparency)
- **Stage 3**: light yellow background (`color.yellow`, 85% transparency)
- **Stage 4**: light red background (`color.red`, 90% transparency)
- **Stage MA** (optional, off by default): orange line (`color.rgb(255, 140, 0)`) of the 30-week SMA
- **Data window**: Stage, MA Slope %/W, Weeks in Stage, Weekly Volume Ratio (weekly volume / prior 10-week average)

### Limitations

- About 35 weeks of history are needed for the slope and 52 weeks before the first stage is assigned.
- Intended for daily and weekly charts.
- On realtime bars the in-progress week only has partial volume, so a volume-backed upside breakout tends to be recognized later in the week.
