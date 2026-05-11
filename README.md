# Stock Data Fetcher

This repository fetches minute-level stock price data from Yahoo Finance, stores it in a local SQLite database, and writes a summary output to `README.md`.

## Prerequisites

- Python 3.8+ installed
- Internet connection for Yahoo Finance data
- The following Python packages installed:
  - `pytz`
  - `yfinance`
  - `pandas`

Install dependencies with:

```bash
pip install -r requirements.txt
```

## How to run

Run the main script from the repository root:

```bash
python data_fetch.py
```

When the script starts, it reads `symbols.json` and prompts you to enter the key for the symbol set you want to use.

Example prompt:

```text
Enter the key for STOCKS in symbols.json ['STOCKS', 'large_cap', 'mid_cap', 'small_cap']: 
```

Enter one of the available keys exactly as shown.

## How output is generated

When `data_fetch.py` runs:

1. It loads the selected symbol list from `symbols.json`.
2. It creates a local SQLite database file and table(s) for each symbol.
3. It downloads 1-minute interval price data for each symbol.
4. It inserts the fetched rows into the database using `INSERT OR IGNORE` to avoid duplicates.
5. It updates `README.md` with the latest snapshot table rows for each symbol.
6. It writes logs to `data_fetch.log` with rotation enabled when the file exceeds 5 MB.

## Result files

- `*.db` — SQLite database containing data for the selected symbol set.
- `README.md` — updated stock data snapshot and timestamp.
- `data_fetch.log` — runtime log file with size-based rotation.

## How to update `symbols.json`

Open `symbols.json` and add or modify keys at the top level. Each key should hold a list of ticker symbols.

Example structure:

```json
{
  "STOCKS": [
    "RELIANCE.NS",
    "HDFCBANK.NS",
    "INFY.NS"
  ],
  "large_cap": [
    "RELIANCE.NS",
    "HDFCBANK.NS"
  ],
  "mid_cap": [
    "ALKEM.NS",
    "APLAPOLLO.NS"
  ]
}
```

To use a different symbol set, update the list under the desired key, save the file, and run `python data_fetch.py` again.

## How to choose the desired symbols

When the script runs, type the exact key name from `symbols.json` that corresponds to the symbol list you want.

For example:

- `STOCKS` to use the default top 20 list
- `large_cap` to use the large-cap list
- `mid_cap` to use the mid-cap list
- `small_cap` to use the small-cap list

If the key is missing or invalid, the script will raise an error.

## Notes

- Keep `symbols.json` in the same directory as `data_fetch.py`.
- The script currently uses Yahoo Finance tickers with `.NS` for NSE India.
- The generated `README.md` title is preserved between runs, and only the stock data snapshot is refreshed.
