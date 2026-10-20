# Supermart Grocery Sales Dashboard

An interactive dashboard that answers the four management questions in the SapphireIQ brief:

1. How is the overall grocery business performing?
2. Which products and categories are driving sales and profit?
3. How are discounts and profit related to business performance?
4. Which locations are contributing most to the business?

**Live page:** open `index.html` (works offline), or enable GitHub Pages (Settings > Pages > Deploy from branch > `main` / root) and share the link.

## What is in this repository

| Path | What it is |
|---|---|
| `index.html` | The dashboard. One self-contained page with the data and Chart.js built in. Filters for year, category, region and city. |
| `assets/chart.umd.js` | Chart.js 4.4.1 (MIT licence). Also inlined in `index.html`, so that file works on its own. |
| `data/raw/` | The original file, untouched. |
| `data/supermart_cleaned.csv` | Cleaned dataset (9,994 rows) with helper columns. |
| `data/Supermart_Cleaned_with_Pivots.xlsx` | Cleaned data, cleaning log and five pivot-style sheets (live SUMIFS formulas). |
| `docs/cleaning_report.md` | Every issue found and how it was fixed. |
| `scripts/` | `clean_data.py`, `build_excel.py`, `build_dashboard.py` and the HTML template. |

## Data cleaning summary

| Issue | Fix | Rows |
|---|---|---|
| First column had a blank header (it holds the order IDs) | Renamed to `Order ID` | 9,994 |
| About 4,000 order dates had day and month swapped (source was dd-mm-yyyy) | Swapped back; all dates now in one `yyyy-mm-dd` format | 3,683 |
| City spelling "Ramanadhapuram" | Standardised to "Ramanathapuram" | 421 |
| No analysis columns | Added Year, Quarter, Month, Year-Month, Profit Margin, Discount Band | all |

No missing values, duplicate rows or extra spaces were found. See `docs/cleaning_report.md` for details.

## Rebuild from scratch

```bash
pip install -r requirements.txt
python scripts/clean_data.py       # raw xlsx -> cleaned csv + log
python scripts/build_excel.py      # cleaned csv -> workbook with pivot sheets
python scripts/build_dashboard.py  # cleaned csv -> index.html
```

## Key findings (full dataset)

- Sales ₹1,49,56,982, profit ₹37,47,121, margin 25.1%, 9,994 orders.
- All seven categories are close: Eggs, Meat & Fish leads with 15.2% of sales; Oil & Masala is lowest at 13.6%.
- Sales grew every year, from ₹29.8 L in 2015 to ₹49.8 L in 2018.
- West is the top region (32.1% of sales), followed by East and Central.
- Discount and profit show no relationship (correlation 0.00); margins stay between 24.8% and 25.6% in every discount band.
- All orders are in Tamil Nadu, so locations are compared by city. Kanyakumari, Vellore and Bodi lead on sales.
- Cities with above-median sales but below-median margin: Kanyakumari, Salem, Tenkasi, Coimbatore.

## Data caveats

- About 4,000 order dates had day and month swapped in the source and were corrected.
- Region "North" has a single order. Every city appears in several regions.
- The file has no product-name column, so sub-category is used as the product level.
- Currency is not stated in the file; rupees are assumed.

## Tools

Python (pandas, openpyxl), Excel formulas (SUMIFS, COUNTIFS, CORREL), HTML, CSS, JavaScript, Chart.js.

*Built as a SapphireIQ data analytics student project.*
