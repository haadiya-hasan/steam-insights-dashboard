# STEAM INSIGHTS: The Data Behind the Games

An interactive, dark-mode analytics dashboard for ~27,000 games from the Steam Store and SteamSpy APIs. It is a single self-contained HTML file: no server, no framework, no external libraries.

![status](https://img.shields.io/badge/build-python%203-blue) ![data](https://img.shields.io/badge/games-27%2C075-66c0f4)

## Features

- **Overview**: KPI cards, releases by year, paid-price distribution, free vs paid, platform support, genre ranking (switchable metric), reviews vs positive share, price vs popularity.
- **Game Explorer**: sortable table (Shift-click for multi-column sort), pagination, adjustable page size, column visibility, CSV export, and a game profile pop-up with a Steam Store link.
- **Genres & Categories**: ranking by count, free-to-play share, median price, median reviews or positive share (rate metrics need at least 30 games), genre share over time, genre co-occurrence heatmap.
- **Publishers**: rank publishers or developers by several metrics with a minimum-games threshold, plus a searchable table with top-genre specialisation.
- **Player Insights**: estimated-owner distribution, free vs paid engagement, review and playtime leaderboards, correlation matrix.
- **Global filters** that apply to every view together: title search, genre, publisher, release years, max price, free/paid, minimum reviews, minimum estimated owners, platforms, single/multi-player, Early Access, minimum achievements, age-restricted. Filter chips, reset-all, collapsible panel, and an empty state.
- **Cross-filtering**: click a genre bar, a year point or a publisher row to filter the whole dashboard.
- **Automated written insights**: a deterministic rules engine writes findings from the current selection, plus a "What changed with these filters?" comparison against the full catalogue. No AI-generated text, and nothing is hardcoded.

## Quick start

Requires Python 3 with `pandas`.

```bash
pip install pandas
python build.py
```

Then open `dist/steam_insights.html` in any modern browser.

`build.py` cleans the data, writes `dist/steam_clean.csv`, and injects a compact copy of the data into `template.html` to produce the dashboard.

## Repository layout

```           
├── template.html        # dashboard UI (CSS + JS); __DATA__ is replaced at build time
├── data/steam.csv       # source dataset (add it yourself, see below)
```

## Dataset

Steam Store Games (Clean Dataset): 27,075 rows, 18 columns (`appid`, `name`, `release_date`, `developer`, `publisher`, `platforms`, `categories`, `genres`, `steamspy_tags`, `achievements`, `positive_ratings`, `negative_ratings`, `average_playtime`, `median_playtime`, `owners`, `price`, and others). Place it at `data/steam.csv`. Check the original dataset's licence before redistributing it.

## Data cleaning and assumptions

- No duplicate rows or duplicate `appid`s; all release dates parse.
- 15 missing values (1 developer, 14 publishers) are filled as "Unknown".
- `price` has no currency field. It is **assumed to be GBP (£)**. Games priced at 0 (2,560) are treated as free-to-play.
- Release data ends in **May 2019**, so 2019 is a partial year and is marked as such in the release chart.
- `owners` are SteamSpy **ranges (estimates)**, not exact player counts. Totals sum the range bounds.
- Review counts are cumulative observed reviews. They are a popularity proxy and are **not** sales or revenue.
- `median_playtime` is SteamSpy's median in minutes. The dataset has **no concurrent or current-player data**, and the dashboard makes no such claims.
- Correlations are Pearson and descriptive only; they do not imply causation.

## Known limitations

- The "Categories & tags" filter, significance tests and a publisher-by-genre heatmap are not implemented. Tag co-occurrence is covered at genre level only.
- Charts are lightweight inline SVG, so interactions are click-to-filter plus native tooltips rather than zoom and pan.
- CSV export of filtered results uses a browser download and clipboard fallback; some sandboxed viewers block both. Open the HTML file locally if so.
- Scatter plots draw a sample of at most ~1,800 points for performance.

## Design

Palette: background `#171a21`, secondary `#1b2838`, cards `#2a475e`, accent `#66c0f4`, text `#c7d5e0`.

## Licence

Add a licence of your choice for the code. The dataset remains subject to its original terms.
