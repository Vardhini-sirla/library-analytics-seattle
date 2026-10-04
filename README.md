# Seattle Public Library — Checkouts Analytics (2020–2023)

An analysis of 16,451,540 checkouts recorded by Seattle Public Library between January 2020 and December 2023, drawn from the library's "Checkouts by Title" dataset via the Socrata open-data API and filtered to books, ebooks and audiobooks reaching at least five checkouts in a month. The data tracks a collection through the pandemic closures and out the other side: physical circulation stops dead for four months, digital never gives back the share it gained, and audiobooks pass ebooks to become the largest format by 2023. This is Project 1 of a three-project data analytics portfolio.

## Key Findings

Full write-up with context for each: [findings.md](findings.md)

- Physical circulation produced no qualifying titles for four consecutive months, April through July 2020
- Digital still accounted for 70.2% of checkouts in 2023
- Audiobooks became the largest format in 2023 at 35.4% of checkouts
- Total checkouts grew every year, from 3.28M in 2020 to 4.96M in 2023
- Ebook share fell from 49.7% to 34.8% over four years
- Normalizing titles and creators collapsed 30% of title variants and 25% of creator variants, revealing the true rankings
- James Patterson and Taylor Jenkins Reid draw the same volume from catalogs 38 times apart
- Where the Crawdads Sing is the most-borrowed title at 27,896 checkouts

## Repository Structure

```
Library Analytics/
├── Notebooks/
│   └── 01_eda.ipynb      Main analysis: API pull, quality checks, trends,
│                         COVID gap, rankings, normalization, exports
├── findings.md           Recruiter-facing summary of the eight findings
├── Output/
│   ├── monthly_checkouts_by_usageclass.csv     Monthly digital vs physical, full 96-row grid
│   ├── monthly_checkouts_by_materialtype.csv   Monthly totals per format, full 144-row grid
│   ├── format_share_by_year.csv                Yearly checkouts and % share per format
│   ├── top_100_titles.csv                      Top 100 titles, normalized, with primary format
│   └── top_50_creators.csv                     Top 50 creators, normalized, with title counts
├── Data/
│   ├── Raw/              Socrata extract (gitignored, ~310 MB)
│   └── Clean/            Extract plus date and normalized keys (gitignored, ~399 MB)
└── .gitignore
```

Both `Data/` folders are gitignored: the files exceed GitHub's 100 MB limit and are fully reproducible by running the notebook. The `Output/` CSVs are committed so the aggregations can be inspected without regenerating anything.

## Reproducing the Analysis

1. Clone the repository:
   ```
   git clone https://github.com/Vardhini-sirla/<repo>.git
   cd <repo>
   ```
2. Install the dependencies:
   ```
   pip install pandas jupyter
   ```
3. Open the notebook:
   ```
   jupyter notebook Notebooks/01_eda.ipynb
   ```
4. Run all cells. The first data cell pulls roughly 1.34 million rows fresh from the Socrata API, which normally takes one to three minutes.
5. Step 5a writes the cleaned dataset to `Data/Clean/`, and Step 5b writes the five aggregation CSVs to `Output/`. Both cells verify their own totals and fail loudly on a mismatch.

## Methodology Notes

The grain is one row per title, edition, material type, checkout type, year and month, where `checkouts` is a monthly sum — so every figure sums that column rather than counting rows, and duplicate-looking rows are separate editions that are never deduplicated. The extract covers checkout years 2020–2023, material types BOOK, EBOOK and AUDIOBOOK, and only records reaching five checkouts in a month; a zero therefore means no title cleared that floor, not that nothing circulated. Step 4b normalizes titles and creators across the two source catalogs, merging 30% of title variants and 25% of creator variants. Filter rationale and the known cleaning limitations are documented in [findings.md](findings.md).

## Tools

- Python (pandas)
- Jupyter / VS Code
- Power BI (dashboards forthcoming)

## About

Shia (Vardhini Sirla), MS Computer Science at Clark University — data analytics portfolio: [github.com/Vardhini-sirla](https://github.com/Vardhini-sirla)
