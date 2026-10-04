# Seattle Public Library — Checkouts Analytics (2020–2023)

## Overview

This analysis covers 16,451,540 checkouts recorded by Seattle Public Library across 48 consecutive months, January 2020 through December 2023. The source is the library's "Checkouts by Title" dataset, pulled from the Socrata open-data API and filtered to physical books, ebooks and audiobooks that reached at least five checkouts in a given month, leaving 1,342,223 title-month records. The window brackets the pandemic branch closures and the three years of recovery that followed, so the data captures both the collapse of physical circulation and the format shift that outlasted it: audiobooks drawing level with ebooks, digital holding 70% of volume long after the branches reopened, and total circulation growing every year. Built as part of a data analytics portfolio; the notebook, cleaned dataset and Power BI aggregations are listed at the end.

## Key Findings

- **Physical circulation produced no qualifying titles for four consecutive months, April through July 2020**: not one physical title reached five checkouts in a month during the closures, and volume resumed at just 641 in August 2020 before reaching 18,310 that September.
- **Digital still accounted for 70.2% of checkouts in 2023**: three years after the branches reopened, the pandemic shift held — digital came off its 81.4% peak in 2020 but never returned to a print-led split.
- **Audiobooks matched ebooks as the largest format in 2023, within 0.6 percentage points**: 35.4% against 34.8%, with ebooks ahead by ~1,769 checkouts once each format's top 50 titles are excluded (robustness check in Q7), after audiobooks sat near 31% for three straight years.
- **Total checkouts grew every year, from 3.28M in 2020 to 4.96M in 2023**: circulation rose 51% across the period, with each year higher than the last despite the closure year at the start.
- **Ebook share fell from 49.7% to 34.8% over four years**: ebooks carried the 2020 closure alone, then gave share back to both audiobooks and returning print.
- **Normalizing titles and creators collapsed 42% of title variants and 25% of creator variants, revealing the true rankings**: the physical and digital catalogs spell the same work differently, so pre-normalization rankings split a single book across three entries and understated its real demand.
- **James Patterson and Taylor Jenkins Reid draw the same volume from catalogs 34 times apart**: Patterson takes 52,775 checkouts across 338 titles against Reid's 52,505 across 10, a 0.5% difference in volume — catalog breadth and per-title velocity arriving at the same place by opposite routes.
- **Where the Crawdads Sing is the most-borrowed title at 27,896 checkouts**: it finishes 2.5% ahead of Braiding Sweetgrass at 27,214. Braiding Sweetgrass's print edition is catalogued without its subtitle and counted separately (1,496 checkouts); added back, it would lead, so the top of the list is a tie in practice.

## Methodology Notes

- Source: Seattle Public Library "Checkouts by Title", retrieved from the Socrata open-data API at data.seattle.gov.
- Scope: checkout years 2020–2023, restricted to the three main material types — BOOK, EBOOK and AUDIOBOOK.
- Threshold: records below five checkouts in a month were excluded at download, which keeps the extract tractable but means a zero in these tables says no title cleared that floor, not that nothing circulated.
- Grain: one row per title, edition, material type, checkout type, year and month. `checkouts` is a monthly sum, so every volume figure sums that column; row counts are never used as a stand-in. Rows are not deduplicated, since repeated titles are separate editions.
- Normalization: titles have the print catalog's spaced subtitle colon (" : ") and repeated whitespace collapsed, are lowercased, and lose "(unabridged)", bracketed format markers, trailing " / Author." catalog suffixes, series suffixes such as ": … series, book 1", and stock subtitles (": a novel", ": a memoir", ": a story", ": a true story", ": the unabridged edition"). Creators are converted from "Surname, Forename, 1966-" to "Forename Surname", with publisher and placeholder names (DK, Disney, Various, Anonymous) blanked. Title and author rankings quote post-normalization figures throughout.
- Limitation: subtitle stripping covers a fixed list of stock phrases, so a print record catalogued without a subtitle that its digital edition carries (bare "Braiding Sweetgrass" against "Braiding Sweetgrass: Indigenous Wisdom, …") still counts separately, and two different works sharing a cleaned title still merge.
- Limitation: creator title-casing flattens internal capitals, rendering "McDonald" as "Mcdonald".

## Files

- `Notebooks/01_eda.ipynb` — full exploratory analysis: API pull, data quality checks, monthly trends, the COVID gap investigation, rankings, and title/creator normalization.
- `Data/Clean/seattle_checkouts_2020_2023_clean.csv` — the filtered dataset with `date`, `clean_title` and `clean_creator` added alongside the original columns.
- `Output/monthly_checkouts_by_usageclass.csv` — monthly checkouts, digital versus physical.
- `Output/monthly_checkouts_by_materialtype.csv` — monthly checkouts by material type.
- `Output/format_share_by_year.csv` — yearly checkouts and percentage share per material type.
- `Output/top_100_titles.csv` — top 100 titles on the normalized key, with each title's primary format.
- `Output/top_50_creators.csv` — top 50 creators on the normalized key, with distinct title counts.
