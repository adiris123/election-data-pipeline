# Election Data Pipeline

Part of the MPIDISM project (political ideology detection on Indian election data).

## What is in this repo

| File | Purpose |
| --- | --- |
| `merge_election_data.py` | Merges per-constituency election CSVs into one cleaned file, sorted by constituency and votes (highest first), with sanity checks. |
| `collect_daily.py` | Collects political news (RSS) and Reddit posts, cleans them, removes duplicates, sorts newest first and appends to `data/tweets_real.csv`. |
| `.github/workflows/daily.yml` | Runs `collect_daily.py` every day on GitHub and commits the new data. |
| `data/tweets_real.csv` | Real, collected posts (news sites and Reddit). Synthetic/generated rows were removed. Not yet labelled. |

## Usage

```bash
pip install -r requirements.txt
# put the election CSV files in data/raw/election_dataset/
python merge_election_data.py
```

Output: `output/final_cleaned_sorted.csv` with columns
`constituency, constituency_id, name, party, status, votes`.

`constituency_id` is the full file name, so same-named seats in different states
(for example Aurangabad, Maharajganj) stay separate.

The script prints a check at the end: 543 constituencies and exactly one winner each.

## Daily automatic collection

1. Create a free Reddit app at reddit.com/prefs/apps (type: script) to get a client ID and secret.
2. In the GitHub repo: Settings > Secrets and variables > Actions > add `REDDIT_CLIENT_ID` and `REDDIT_CLIENT_SECRET`.
3. Run it once from the Actions tab (Run workflow). After that it runs daily.

Without the Reddit secrets, only the news feeds are collected.

## Notes

- The Twitter scraper (`snscrape`) is no longer maintained and is not included.
- The raw election files are not stored in this repo (`data/raw/` is git-ignored).
