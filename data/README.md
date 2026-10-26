# Data

The analysis uses one file, `transfers.csv`, which is **not included in this repository**. Download it and place it here as `data/transfers.csv`.

## Source

- **Dataset:** "Major European football leagues transfers dataset" by d2ski, file `dataset/transfers.csv` in <https://github.com/d2ski/football-transfers-data>
- **Original data:** scraped from [Transfermarkt](https://www.transfermarkt.com) by the dataset author.
- **Coverage:** transfers in and out of clubs in 7 first divisions, seasons 2009–2021: Premier League (`GB1`), LaLiga (`ES1`), Serie A (`IT1`), Bundesliga (`L1`), Ligue 1 (`FR1`), Liga Portugal (`PO1`), Eredivisie (`NL1`).
- **Size:** 70,006 rows, 23 columns, about 10.5 MB.

## Why it is not in the repo

The source repository has no license, and the data itself is scraped from Transfermarkt, whose terms of use restrict reuse of their content. Since redistribution is not clearly allowed, the file is git-ignored (`data/*.csv`). Only aggregated results (`tables/`) and figures are published here. One small exception is noted below.

## How to get it

From the repository root:

```bash
curl -L -o data/transfers.csv \
  https://raw.githubusercontent.com/d2ski/football-transfers-data/main/dataset/transfers.csv
```

Or open the GitHub page above, download `dataset/transfers.csv` and save it as `data/transfers.csv`.

To check that you have the exact file used for the results:

```bash
shasum -a 256 data/transfers.csv
# d55e546baf3777150b2f3ad6fe18ca984d51ce08705e6bd93c160df752f78922
```

The notebook reads it with the relative path `../data/transfers.csv`, so run it from `notebooks/`.

## Columns used

| Column | Used for |
|---|---|
| `league` | buying league (the main grouping variable) |
| `season` | season dummies, time trends, pre/post-2016 split |
| `dir` | keep `in` rows only, so each transfer is counted once, from the buyer's side |
| `is_loan`, `is_loan_end` | keep permanent transfers only |
| `transfer_fee_amnt` | fee in EUR (must be > 0) |
| `market_val_amnt` | Transfermarkt market value in EUR at the time of the transfer (must be > 0) |
| `player_age` | age bands, quadratic age profile |
| `player_pos` | 16 roles grouped into GK / DEF / MID / ATT |
| `team_country`, `counter_team_country` | domestic vs. cross-border purchase; country transfer network |
| `player_nation` | foreign vs. national player |
| `player_id`, `team_id` | counting distinct players and clubs (independence check) |
| `player_name`, `team_name` | listing the dropped outliers |

Not used: `window`, `player_nation2`, `counter_team_id`, `counter_team_name`, `is_free`, `is_retired`, `transfer_id`.

## Note on `tables/outliers_dropped.csv`

This table holds the 34 raw rows removed by the |z| > 3 outlier rule (all columns, plus the computed premium). It is the only file in the repo that contains record-level rows from the dataset.
