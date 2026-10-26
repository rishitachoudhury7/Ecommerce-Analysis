# E-commerce Sales Analysis: Revenue, Discounts and Delivery

An end-to-end analysis of 50,000 global e-commerce orders (Jan 2023 – Jun 2026). It finds where revenue comes from, whether discounts pay off, and how delivery speed affects customer ratings. The work was done in Python (Google Colab) and Google Sheets, with a Looker Studio dashboard and a management presentation.

**[View the interactive dashboard](https://datastudio.google.com/reporting/13057c7d-9cfe-45da-9790-937be8598d1b)**


---

## Key results at a glance

| Metric | Value |
|---|---|
| Orders analysed | 50,000 (6 countries, 5 categories, 25 products) |
| Total revenue | $32.1M |
| Average order value | $642.53 |
| Average customer rating | 3.33 / 5 |
| Orders delivered late (11+ days) | 33.3% |
| Discounts given | $5.30M (14.2% of pre-discount sales) |

---

## Business questions

1. Which product categories, countries and customers generate the most revenue, and is the business growing?
2. Do deeper discounts lead customers to buy more?
3. How does delivery time relate to customer ratings?
4. Which customer segments bring the highest value per order?

---

## Main findings

| # | Finding | Evidence |
|---|---|---|
| 1 | **Nov–Dec generates about 44% of annual revenue.** | 46.0%, 43.7% and 44.0% of revenue in 2023, 2024 and 2025. An average Nov–Dec month earns $2.18M vs $0.54M for other months. |
| 2 | **Electronics carry the business.** | 77.3% of revenue from 30% of orders. The top 5 products are all Electronics (14.9–15.9% each). |
| 3 | **A third of orders arrive late, and ratings collapse after day 10.** | Average rating is about 3.98 for deliveries in 1–10 days and about 2.03 from day 11. 95.2% of late orders are rated below 3, and the late share is 32–34% in every country. |
| 4 | **In regular months, bigger discounts reduce revenue per order without selling more items.** | In Jan–Oct, orders at 0–5%, 6–10% and 11–15% discount all average about 2.0 items, while revenue per order falls from $592 to $563 to $517. |
| 5 | **Ages 18–34 bring 61% of revenue.** | They are 49.9% of orders. They buy more Electronics (40% of their orders vs 11–22% for older groups). Spend within a category is the same across ages. |
| 6 | **No country, gender or payment method is a weak spot.** | All six countries earn $5.19M–$5.47M, and order value is $621–$660 in every country. |
| 7 | **Growth is flat.** | About $10M a year: $9.68M (2023), $9.59M (2024), $10.04M (2025). Orders stay around 15,000 a year. |

### The surprising result: discounts don't drive basket size

I expected bigger discounts to mean more items per order. At first the data agreed: orders with up to 15% discount averaged about 2 items, and orders with 16% or more averaged about 3.5. But once I split the data into **Jan–Oct** and **Nov–Dec**, the link disappeared:

| Season | Discount band | Orders | Avg items per order | Avg order value |
|---|---|---|---|---|
| Jan–Oct | 0–5% | 12,473 | 2.02 | $591.76 |
| Jan–Oct | 6–10% | 11,461 | 2.01 | $562.68 |
| Jan–Oct | 11–15% | 10,138 | 2.01 | $516.89 |
| Nov–Dec | 11–15% | 488 | 3.52 | $836.67 |
| Nov–Dec | 16–20% | 5,431 | 3.49 | $894.82 |
| Nov–Dec | 21–30% | 10,009 | 3.49 | $778.12 |

Within each season, the correlation between discount and items is essentially zero (−0.009 in Jan–Oct, +0.002 in Nov–Dec). Both items per order and discounts step up between seasons, not between discount levels. The data cannot separate the holiday effect from the discount effect, because Jan–Oct discounts never exceed 15% and Nov–Dec discounts never go below 15%.

---

## Recommendations

| Priority | Recommendation | Why | How to measure |
|---|---|---|---|
| 1 | **Fix late deliveries.** Make 10 days the standard and bring the late share from 33% to about 20%. Find where the delays come from first. | Finding 3 | % delivered within 10 days (now 66.7%), average rating (now about 3.33) |
| 2 | **Test smaller discounts in Jan–Oct.** Cap discounts at 5–10% in some markets for 3–6 months and compare with the rest. | Finding 4 | Items and revenue per order, test vs comparison group, plus ratings and repeat purchases |
| 3 | **Plan early for Nov–Dec.** Have stock, delivery capacity and support staff ready by September. | Findings 1 and 2 | Stock availability of top 5 products, on-time rate and revenue in Nov–Dec vs last year |

Illustrative impact: at a 20% late share the average rating would rise from about 3.33 to about 3.59, and the discount test could be worth up to about $0.26M a year. Both are upper-limit illustrations, not forecasts.

---

## Approach

1. **Data cleaning** (all checks run in the notebook)
   - No missing values, no duplicate rows or Transaction_IDs, no spelling variants, and all numeric values within valid ranges.
   - Total_Revenue matches Unit_Price × Quantity × (1 − Discount) on every row.
   - Converted `Date` from text to a date type.
   - Flagged 648 revenue outliers per product category with the IQR rule. Kept them (all are Nov–Dec orders with large baskets) and checked that key results hold without them.
   - Found that `Customer_ID` is unreliable (14,093 of 18,352 IDs appear with more than one age, and 11,112 with more than one gender), so the analysis is at order level and the column is dropped from the final dataset.
2. **Feature engineering**: year, month, season (Nov–Dec vs Jan–Oct), age group, delivery bucket and on-time/late status, discount band, revenue before discount, discount amount, net price per unit. Delivery and discount bands follow where the data actually changes (day 11, 16% discount).
3. **Exploratory analysis**: seasonality, category concentration, delivery vs ratings, discounts vs basket size, age and category mix, country/gender/payment segments, growth trend, and a check with and without outliers.
4. **Dashboard** in Looker Studio and a 7-slide management presentation.

---

## Limitations

- **Patterns, not proven causes.** Late delivery goes with low ratings, but this data can't prove it causes them.
- **No cost or profit data.** Everything is about revenue. I can't say whether discounts make or lose money.
- **`Customer_ID` can't identify real customers**, so there are no repeat-buyer or lifetime-value claims.
- **The data looks very tidy and may be simulated.** For example, every segment has almost the same rating, and the rating drop at exactly day 11 is unusually sharp. Findings may not carry over to a real business.
- **Short history.** The data ends on 1 June 2026, and Nov–Dec has been observed only three times. 2026 is compared with earlier years on Jan–May only.
- **One conclusion the data can't support:** that cutting Jan–Oct discounts will raise profit. That needs margin data and a controlled test.

---

## Repository contents

| File | What it is |
|---|---|
| [`Ecommerce_Analysis_Rishita.ipynb`](Ecommerce_Analysis_Rishita.ipynb) | Colab notebook: cleaning, feature engineering and EDA with all outputs |
| [`Ecommerce Analysis Sheet- Rishita.xlsx`](Ecommerce%20Analysis%20Sheet-%20Rishita.xlsx) | Assessment workbook: dataset, processed data and written answers (Q1–Q8) |
| [`E-commerce Performance Findings & Recommendations.pptx`](E-commerce%20Performance%20Findings%20%26%20Recommendations.pptx) | 7-slide management presentation |
| [`cleaned_global_ecommerce_data.csv`](cleaned_global_ecommerce_data.csv) | Source dataset (50,000 rows × 15 columns) |
| [`Final_Cleaned_processed_ecommerce_data.csv`](Final_Cleaned_processed_ecommerce_data.csv) | Processed dataset used for the dashboard (50,000 rows × 25 columns) |

---

## How to reproduce

1. Open `Ecommerce_Analysis_Rishita.ipynb` in Google Colab.
2. Run the first cell and upload `cleaned_global_ecommerce_data.csv` when asked.
3. Run all cells in order. The notebook creates `processed_ecommerce_data.csv` for the dashboard.

Requires Python 3 with pandas, numpy, matplotlib and seaborn (all pre-installed in Colab).

---

## Data source

[50K Global Retail Sales & Customer Ratings Data](https://www.kaggle.com/datasets/hanzalamushtaq/50k-global-retail-sales-and-customer-ratings-data) on Kaggle. 50,000 transactions from 1 Jan 2023 to 1 Jun 2026 with date, customer age, gender and country, product category and name, unit price, quantity, discount, total revenue, payment method, delivery days and customer rating. The file was already cleaned when downloaded.

## Tools

Python (pandas, numpy, matplotlib, seaborn), Google Colab, Google Sheets, Looker Studio, Google Slides/PowerPoint. AI assistants (Claude and ChatGPT) were used for debugging code, chart ideas and presentation wording. All figures were checked against the notebook outputs.

## Author

**Rishita Choudhury** · [GitHub](https://github.com/rishitachoudhury7)
