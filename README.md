# A Decade of Movement

A personal travel journal built from 10 years of Google Maps Timeline data with Claude Code — visualized as a year-in-review story and a chronological trip log.

**[View Live →](https://jgsilvey.github.io/decade-of-movement)**

---

## What It Shows

**Year in Review** — one card per year from 2016–2025, each with a headline, narrative summary, visit count, trip count, busiest month, and countries visited. COVID years are visually flagged in red. A bar across each card shows activity level relative to the peak year.

**Journey View** — every major trip chronologically, filterable by international or domestic. Purple glowing dots mark international trips; each entry shows destination, country flags, dates, and duration.

---

## How It Was Built

### Step 1 — Exporting the data
Data was exported from the Google Maps app on Android. The export produces a single `Timeline.json` file containing semantic location segments — each one representing a detected visit or movement, with coordinates, timestamps, and inferred location type.

### Step 2 — Parsing and cleaning with AI
The raw Timeline JSON has three main sections: `semanticSegments`, `rawSignals`, and `userLocationProfile`. I used Claude to:

- Parse the semantic segments and filter out visits labeled HOME, INFERRED_HOME, WORK, and INFERRED_WORK to isolate genuine out-of-home activity
- Apply a custom home-radius filter to remove any remaining visits near my home addresses (pre-2020 and post-2020 separately, accounting for a move in March 2020)
- Run offline reverse geocoding on all coordinates using the `reverse_geocoder` Python library to attach city, state, and country labels to each visit
- Validate the geocoded results by checking suspicious entries — identified that New Jersey visits were legitimate (EWR airport), Indiana was correct (GenCon), and Mississippi was likely GPS drift from a New Orleans swamp tour near the state line
- Identify distinct trips by clustering consecutive days of visits more than 30 miles from home

### Step 3 — Building the visualization
Also built with Claude. Key design decisions:

- The page intentionally avoids charts — data is presented as editorial cards and a timeline, closer to a travel diary than a metrics dashboard
- The file is fully self-contained HTML with all data embedded, so it works offline and hosts easily on GitHub Pages
- Year cards include narrative headlines and summaries, using the data as evidence rather than just displaying raw numbers

---

## Key Findings

**Travel**
- 23 countries visited across 5 continents since tracking began
- 2019 was the most internationally traveled year before COVID — Chile, Argentina, South Korea, and Taiwan all in one year
- 2023 was the comeback year — first trip to Europe (Italy, France, Vatican, Ireland) plus India, adding 6 new countries in a single year
- 2025 was the most internationally active year on record, anchored by a 22-day Europe trip covering Italy, Denmark, Switzerland, Belgium, the Netherlands, and Germany

**Activity**
- Peak year was 2019 with 1,902 out-of-home visits — nearly 5x the 403 recorded in 2021 at the height of COVID recovery
- COVID-19 is visible in the data: March 2020 visits dropped to near zero and international travel went to zero for over two years
- The 2022 domestic travel resurgence — 14 trips with no international destinations — reflects a deliberate return to movement before international travel resumed

---

## What I Learned

The most interesting part of this project was getting an at-a-glance look at how I travelled. Me from ten years ago wouldn't have anticipated some of the palces I've gone!

Working with AI to clean and analyze this data required some wrangling: deciding what counted as a "visit," how to define home radius, which geocoding errors were worth correcting, and how to frame the narrative for each year. The output is only as good as those decisions — which is the same skill that matters in any data-adjacent role.

---

## Data Privacy Note

All precise coordinate data has been removed from the published file. The page embeds only aggregated counts and city-level location names. No specific addresses or movement patterns that could identify home or work locations are included.

---

## Updating With New Data

1. Export a fresh `Timeline.json` from Google Maps on your phone
2. Upload to Claude and ask it to re-run the same parsing and filtering logic
3. Replace the embedded data arrays in `index.html`
4. Push to GitHub — the live site updates automatically

---

## Tools Used

- **Google Maps Timeline** — data source
- **Claude (Anthropic)** — data parsing, cleaning, geocoding, trip detection, and page build
- **reverse_geocoder** — offline Python library for coordinate-to-city lookup
- **GitHub Pages** — hosting
