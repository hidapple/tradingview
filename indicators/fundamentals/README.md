# Fundamentals

Indicators for checking a symbol's fundamentals directly on the chart.

## `fundamentals_table.pine`

Overlay indicator that draws a table of quarterly and annual fundamentals for the chart symbol, meant for a quick look at the numbers behind a chart that looks good technically. Hide it with the indicator's visibility toggle on the chart when it is not needed.

### Data Sources

- **EPS**: `request.financial(..., "EARNINGS_PER_SHARE", "FQ")` for quarters and `"FY"` for the annual total
- **Revenue**: `request.financial(..., "TOTAL_REVENUE", "FQ")` and `"FY"`
- **ROE**: `request.financial(..., "RETURN_ON_EQUITY", "FQ")` and `"FY"`, shown as a percentage as returned by TradingView
- **Listing date**: time of the first available bar via `request.security()` on the monthly feed (month precision), replaced by the daily feed's first bar when it reaches the same month (day precision)

Values are shown in the company's reporting currency. Symbols without financial data (ETFs, indices, crypto) show `n/a`.

### How Periods Are Collected

`request.financial()` returns the latest reported value on every bar, stepping to a new value at each fiscal period boundary. The indicator records the bar where the revenue series steps (EPS is used as the fallback while revenue is unavailable) and samples EPS, revenue and ROE at that bar, once for the quarterly series and once for the annual series.

Quarters are assigned to a fiscal year by their distance in months from that year's end: the quarter that steps together with the annual value is Q4, the one about three months earlier is Q3, and so on. Rounding tolerates boundaries that land a few days into the next month. The year label is the calendar year in which the fiscal year ended (a March 2025 year-end is labeled `2025`).

Quarters reported after the last completed fiscal year form the row for the current year, which counts toward the displayed years. Its unreported quarters and Total are left blank.

A value already present on the first chart bar belongs to an unknown period and is not counted, so the chart history must cover the requested years; Daily or higher timeframes are intended.

### Table Layout

| EPS     | Q1              | Q2              | Q3              | Q4              | Total           |
|---------|-----------------|-----------------|-----------------|-----------------|-----------------|
| 2022    | 2.10 (+25.0%)   | 1.52 (+8.6%)    | 1.20 (-7.7%)    | 1.29 (+13.2%)   | 6.11 (+8.9%)    |
| 2023    | 1.88 (-10.5%)   | 1.52 (+0.0%)    | 1.26 (+5.0%)    | 1.46 (+13.2%)   | 6.13 (+0.3%)    |
| 2024    | 2.18 (+16.0%)   | 1.53 (+0.7%)    | 1.40 (+11.1%)   | 0.97 (-33.6%)   | 6.08 (-0.8%)    |
| 2025    | 2.40 (+10.1%)   | 1.65 (+7.8%)    | 1.57 (+12.1%)   | 1.84 (+89.7%)   | 7.46 (+22.7%)   |
| 2026    | 2.51 (+4.6%)    | 1.70 (+3.0%)    |                 |                 |                 |
| Revenue | Q1              | Q2              | Q3              | Q4              | Total           |
| 2022    | 123.9B (+11.2%) | 97.28B (+8.6%)  | 82.96B (+1.9%)  | 90.15B (+8.1%)  | 394.33B (+7.8%) |
| ...     |                 |                 |                 |                 |                 |
| ROE     | Q1              | Q2              | Q3              | Q4              | Total           |
| 2022    | 145.6% (+8.3pt) | 149.0% (+21.4pt)| 160.9% (+13.1pt)| 175.5% (+28.1pt)| 175.5% (+28.1pt)|
| ...     |                 |                 |                 |                 |                 |
| Listed  | 1980/12/12 (45y 9m) |||||

- One block per enabled item: a shaded header row (item name, Q1 to Q4, Total) followed by one row per fiscal year, oldest first
- Each cell shows the value followed by the change vs the same period of the previous fiscal year in parentheses. EPS and revenue changes are percentages against the absolute previous value, so a loss shrinking toward profit reads positive; ROE changes are percentage points (`pt`). EPS and revenue text is green when the change is +25% or more, the normal text color between 0% and +25%, and red when negative; ROE text is green for any positive change and red for any negative one. The change is omitted when the previous period is unknown or its value is zero
- One extra completed year is collected beyond the displayed count so the oldest displayed year also shows its change
- **Total** is the annual value as reported, not a sum of the quarters (for ROE it is the annual ratio)
- Revenue is abbreviated with K / M / B / T
- `n/a` marks a period the company did not report (e.g. no quarterly data); blank cells in the last row are periods of the current year not reported yet
- **Listed**: first available bar plus elapsed time. Matches the IPO date for recent listings; for older symbols it is where TradingView's price history starts, not the IPO
- Text uses the chart's foreground color and the background follows the chart theme, so the table is readable on both light and dark charts

### Inputs

- **Fiscal years to show** (default 5, 1 to 10): rows in the table; the current year counts as one of them once it has a reported quarter
- **Table position** (default bottom left) and **Text size** (default small)
- **Background transparency** (default 20)
- **Items**: EPS, Revenue, ROE, Listing date (all on by default). Blocks for disabled items are removed from the table

### Limitations

- Eight request calls are used (six financial, two security), so this indicator counts toward the per-script request limit if merged with other scripts
- Not all companies report every field or every quarter; missing periods show `n/a`
- Fiscal-year labels and quarter slots are estimated from the boundary bars and can be off for companies with irregular fiscal calendars
- Not designed for the screener: it relies on `table` drawings, which the screener ignores
