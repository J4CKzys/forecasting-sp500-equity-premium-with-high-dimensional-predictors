# Input data

The notebook's saved outputs can be inspected without data. Running the analysis requires a private `data/monthly_data.csv`, excluded from version control.

Use 720 monthly rows from January 1960–December 2019, with unique month-start dates in chronological order. The CSV must have a header, no saved index, and exactly 136 columns in the names and order listed in [`columns.txt`](columns.txt).

| Fields | Meaning |
|---|---|
| `date` | Month-start date in `YYYY-MM-01` format |
| `SPX` | S&P 500 price-index level |
| `S&P div yield` | Annual dividend yield, in percent |
| `TB3MS` | Annual three-month Treasury bill rate, in percent |
| Other 132 columns | 118 macro/financial predictors followed by 14 binary technical signals |

All numeric values must be finite and non-missing; `SPX` must be positive and technical signals must be 0 or 1. The four named fields above are excluded from predictors. The last 14 predictor columns are treated as technical signals; inputs are used as supplied.

The target premium is measured in percentage points:

```text
premium[t] = 100 × log(SPX[t] / SPX[t−1])
             + S&P div yield[t] / 12 − TB3MS[t] / 12
```

Predictors at month `t` forecast `premium[t+1]`, giving 719 aligned pairs. The first 324 pairs train the initial model, leaving 395 OOS target months from February 1987–December 2019: 251 before January 2008 and 144 afterward. Recursive training expands each month; rolling training uses the latest 324 pairs.

The prepared inputs include series associated with FRED-MD and supplied technical signals; source lineage, vintages, transformations and release history are incomplete. No data download pipeline is included.
