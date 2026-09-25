# Indian Startup Funding — Data Cleaning & Analysis

A end-to-end analysis of ~3,000 Indian startup funding deals (2015–2020), built from a public dataset that arrived in genuinely rough shape. This project is less about the final charts and more about the cleaning decisions that had to happen before any chart could be trusted — inconsistent city names, a 500+ value "industry" column that was mostly product taglines, funding amounts stored as text, and a handful of broken dates.

**Tools:** Microsoft Excel (data cleaning) · Power Query · Power BI (modeling & dashboard)

---
![image alt](https://github.com/kishan45yadav/Indian_startup_funding_analysis/blob/main/Screenshot%20(618).png?raw=true)
![image alt](https://github.com/kishan45yadav/Indian_startup_funding_analysis/blob/main/Screenshot%20(619).png?raw=true)
## Why this dataset

Most beginner portfolio projects use data that's already clean, which ends up showcasing chart-building more than problem-solving. This one wasn't like that. Fixing it required actual judgment calls — not "run a formula and move on" but "decide what 27% of unmapped industry records should mean" — and that's the part worth documenting.

## What's in this repo

| File | Description |
|---|---|
| `startup_funding_cleaned.xlsx` | Cleaned dataset with all derived columns (Amount_Clean, Is_Disclosed, City_Clean, Industry_Clean) |
| `Industry_Mapping.xlsx` | Lookup table used to standardize the Industry Vertical field |
| `startup_funding_dashboard.pbix` | Power BI file — two pages (Overview, Investor Analysis) |
| `Startup_Funding_Analysis_Report.docx` | Full business analysis — data questions, findings, and stated limitations |
| `dashboard_overview.png` / `dashboard_investors.png` | Screenshots of both dashboard pages |

## The data problems, and how each was handled

**Funding amounts stored as text.** Values had commas, a trailing `+` on some entries, and placeholder strings (`undisclosed`, `nan`, `unknown`, `N/A`) — a few of which carried invisible non-breaking-space characters that broke even basic text matching. Fixed with a numeric-conversion formula that treats anything that fails to convert as undisclosed, plus a separate `Is_Disclosed` flag so the non-disclosure rate stays visible instead of disappearing.

**City names, three problems deep.** Spelling variants (Bangalore/Bengaluru, Delhi/New Delhi), neighbourhood names standing in for the city (Koramangala, Andheri), and state/country-level entries with no real city (Karnataka, USA). Cells also mixed delimiters — comma, slash, `&`, and the word "and" all showed up in the same column. Solved with a normalization step feeding into a mapping table, with unmatched state/country entries grouped into an explicit `Not Specified` bucket rather than guessed at.

**Industry labels that weren't really categories.** Over 500 unique values, but most of them were product descriptions ("Splitting Bills Mobile App") rather than sector names. After mapping the genuinely common categories, ~27% of records still didn't fit anywhere — that gap was kept as an explicit `Other/Niche` bucket instead of being forced into a category it didn't belong in.

**Multiple investors packed into one cell.** Split using Power Query's Unpivot so each investor gets their own row against the same deal — necessary for any investor-level ranking, and the reason the investor-level funding total (summed across investor-deal pairs) is a different, larger number than the deal-level total.

**Dates without the separating slash.** A handful of entries (`05/072018` instead of `05/07/2018`) silently broke Power BI's date parsing. Caught using Power Query's error indicator rather than scanning row by row.

## Dashboard

**Page 1 — Overview:** total funding, average deal size, and deal count, broken down by year, city, and industry, with slicers for year/city/industry.

**Page 2 — Investor Analysis:** top investors by deal count vs. top investors by total capital deployed (these are two different lists), plus a sortable investor detail table.

## Key findings

- Funding peaked in 2017 (~$10.4B) and 2019 (~$9.4B), with 2020 showing only a partial year of data — not a real decline.
- Bengaluru dominates total funding, with Mumbai, Gurgaon, and New Delhi as a distant second tier.
- E-Commerce and Consumer Internet lead by sector, but ~27% of records had industry descriptions too specific or inconsistent to classify — a data-entry pattern worth flagging on its own.
- The investor who appears in the most deals (Accel Partners, 46) is not the investor who deployed the most capital (Westbridge Capital) — deal frequency and deal value tell different stories.
- A meaningful share of deals have undisclosed amounts; all totals in this analysis reflect disclosed deals only.

Full methodology and every finding — including caveats not summarized here — are in `Startup_Funding_Analysis_Report.docx`.

## Known limitations

- Analysis is based on disclosed amounts only; true total capital raised is higher.
- 2020 data is partial and shouldn't be compared directly against full years.
- Placeholder investor labels (`Undisclosed Investors`, `Undisclosed Investor`, `N/A`) were not yet fully consolidated into a single category at the time of this analysis.

## Contact

**Kishan Yadav**
GitHub: [github.com/kishan45yadav](https://github.com/kishan45yadav) · Portfolio: [kishan45yadav.github.io](https://kishan45yadav.github.io)
