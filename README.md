# A/B Testing: Statistical Significance Analysis

Statistical significance analysis of 4 A/B tests for an e-commerce platform's interface, using a two-proportion z-test (`statsmodels`). Data is processed at the raw event level (~800K rows) and broken down by segment: country, device, continent, and traffic channel.

## Project Goal

Determine which of the 4 new interface versions produce a statistically significant improvement or decline in key conversion metrics, and identify user segments where the effect diverges from the overall result.

## Data

- **Source:** raw event-level data (Google Analytics-style structure), ~800,000 rows
- **Groups:** Control (old version) vs Test (new version), 4 independent A/B tests
- **Conversion metrics:**
  - `add_payment_info/session`
  - `add_shipping_info/session`
  - `begin_checkout/session`
  - `newaccount/session`
- **Segmentation:** overall, by test, by country, device, continent, traffic channel

## Tech Stack

- **Python** (Google Colab) — pandas, statsmodels (`proportions_ztest`)
- **Tableau** — interactive results dashboard

## Analysis Workflow

1. Load raw event-level data from Google Drive
2. Build a pivot table of sessions and conversion events by group
3. Calculate conversion rate for Control and Test groups
4. Run a two-proportion z-test (`statsmodels.stats.proportions_ztest`) for each metric
5. Repeat the calculation for every segment (country / device / continent / channel) within each test
6. Flag statistically significant results (p < 0.05) and calculate % metric change
7. Export results to CSV and build the Tableau dashboard

## Key Insights

- **Test 1 — the only clearly successful variant.** Add payment info +12.54%, add shipping info +6.56%, begin checkout +6.66%. The strongest driver was the European market (+40.32% in add payment info). Red flag: Tablet dropped 33–36%.
- **Test 2 — neutral overall, but anomalous at the segment level.** No significant change overall, but Organic Search dropped 9–14% and the African market fell 51–60%. The Undefined channel spiked +25–31%, a possible sign of bot traffic.
- **Tests 3 and 4 — negative impact.** Test 3 reduced begin checkout by 3.35% (mostly driven by mobile and tablet). Test 4 simultaneously hurt begin checkout (-2.35%) and newaccount (-3.36%), with a near-total collapse on Tablet (down to -49.13%).
- **A recurring problem: Organic Search.** A consistent conversion drop from organic search appears across Tests 1, 2, and 4 — worth a dedicated technical investigation.
- **Data quality concerns.** Anomalous growth in the Undefined segment (Tests 1–3) signals possible tracking issues or bot traffic.

## Strategic Recommendations

1. Halt the rollout of Tests 3 and 4 — they harm conversion
2. Ship Test 1 for desktop and mobile, but not tablet (needs further work)
3. Investigate the Organic Search decline appearing across multiple tests
4. Audit tracking setup given the anomalies in the Undefined segment

## Tableau Dashboard

🔗 [A/B Testing Significance — Tableau Public](https://public.tableau.com/app/profile/anna.savchuk6598/viz/Signification/Significance?publish=yes)

