# Data

No data files are committed. The notebook is saved **with its outputs**, so all results are readable on GitHub without running it. To re-run it, save the price file as `data/raw/Data_PCLab1_Stock.csv` (the `data/raw/` folder is excluded by `.gitignore`).

## Eight-stock price file

**Original source:** course-provided file; not redistributable here.

**Format:** CSV, one row per trading day (2,159 rows), columns:

| Column | Description |
|--------|-------------|
| `Date` | Trading date (YYYY-MM-DD) |
| `AAPL`, `BA`, `T`, `MGM`, `AMZN`, `IBM`, `TSLA`, `GOOG` | Daily closing price, split-adjusted, **not** dividend-adjusted |
| `sp500` | S&P 500 price index level |

**Period:** 12 January 2012 to 11 August 2020.

**Rebuilding an equivalent file from public data (approximate).** Using the unofficial `yfinance` package (`pip install yfinance`), price levels will differ because Yahoo applies later stock splits, but daily returns should be very close. Expect small numerical differences from the committed results.

```python
import yfinance as yf

tickers = ["AAPL", "BA", "T", "MGM", "AMZN", "IBM", "TSLA", "GOOG", "^GSPC"]
px = yf.download(tickers, start="2012-01-12", end="2020-08-12",
                 auto_adjust=False)["Close"]
px = px.rename(columns={"^GSPC": "sp500"})[
    ["AAPL", "BA", "T", "MGM", "AMZN", "IBM", "TSLA", "GOOG", "sp500"]].reset_index()
px.to_csv("data/raw/Data_PCLab1_Stock.csv", index=False)
```
