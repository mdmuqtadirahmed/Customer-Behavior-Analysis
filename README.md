# Customer Behavior Analysis

Analysis of customer shopping behavior using Python, SQL Server and Power BI.

## Project Workflow

### 1. Data preparation in Python (Jupyter notebook)

1. Loaded `customer_shopping_behavior.csv` with pandas and checked the shape and null counts.
2. Filled missing `Review Rating` values with the median rating of each product category.
3. Standardized column names to lowercase with underscores and renamed `purchase_amount_(usd)` to `purchase_amount`.
4. Created `age_group` (Young Adult, Adult, Middle-aged, Senior) by splitting age into four bins.
5. Created `purchase_frequency_days` by mapping `frequency_of_purchases` (Weekly, Fortnightly, Monthly, Quarterly, Annually and others) to a number of days.
6. Confirmed `discount_applied` and `promo_code_used` are identical in every row and dropped `promo_code_used` as redundant.

### 2. Notebook connected to SQL Server

1. Connected the notebook to SQL Server using `pyodbc` and SQLAlchemy.
2. Loaded the cleaned DataFrame into the `customer_shopping_behavior` table in the `customer_behaviour` database with `to_sql`.
3. Verified the load by querying the table back into pandas.
4. Wrote the 10 business questions as SQL queries in `customer_behavior_sql_analysis.sql`, covering revenue by gender, discount behavior, top-rated products, shipping, subscription, customer segments, category rankings and age-group revenue.

### 3. SQL Server connected to Power BI

1. Connected Power BI Desktop to the SQL Server table.
2. Built the Customer Behavior Analysis dashboard with KPI cards (total customers, average order value, average rating).
3. Added visuals for customers by subscription status, revenue and customers by category, and revenue and customers by age group.
4. Added slicers for subscription status, gender, category and shipment type.

## Dataset Overview

| Metric | Value |
|---|---|
| Customers | 3,900 |
| Total revenue | $233,081 |
| Average order value | $59.76 |
| Average review rating | 3.75 |
| Subscribers | 27% (1,053) |

## Insights

**1. Spend per customer is flat everywhere, so revenue comes from headcount.**
Average order value (AOV) stays between $57 and $62 across gender, age group, category, subscription status, shipping type, season, payment method and purchase frequency.

**2. Male customers drive two-thirds of revenue because there are more of them.**
Men are 68% of customers and generate $157.9K (68% of revenue). Women generate $75.2K. AOV is almost identical ($59.54 for men, $60.25 for women), so women spend as much per order as men. There are just fewer of them.

**3. Clothing and Accessories carry the business.**
Clothing is 45% of revenue ($104K) and Accessories is 32% ($74K). Footwear is 15% and Outerwear 8%. Outerwear also has the lowest AOV ($57.17), which is unusual for a category that normally has the highest ticket prices.

**4. Subscribers do not spend more.**
AOV is $59.49 for subscribers and $59.87 for non-subscribers. Every subscriber order has a discount applied (1,053 of 1,053), while only 22% of non-subscriber orders do. Subscription currently works as a discount program with no spend lift.

**5. Discounts do not lift basket size.**
Discounted orders average $59.28 vs $60.13 without a discount. Discounts are applied to 43% of orders. The most discounted items are Hat (50%), Sneakers (49.7%), Coat (49.1%), Sweater (48.2%) and Pants (47.4%). Socks is the least discounted (32.7%).

**6. Subscription rate rises with tenure, but modestly.**
New customers subscribe at 18%, Returning at 24% and Loyal at 28%. Customers with more than 5 previous purchases subscribe at 27.6% vs 22.4% for the rest.

**7. Satisfaction is uniform and is not a lever.**
Ratings range from 3.62 to 3.86 across all items. The best-rated items are Gloves (3.86), Sandals (3.84) and Boots (3.82). The weakest are Shirt (3.62), Jeans (3.65) and Blouse (3.68).

**8. Seasonality and geography are weak.**
Fall has the highest AOV ($61.56) and Summer the lowest ($58.41). The top states (Montana, Illinois, California, Idaho, Nevada) are within a few hundred dollars of each other.

## Recommendations

1. **Raise AOV with bundles and thresholds.** AOV is stuck near $60 regardless of segment, so basket size is the cheapest growth lever. Test "add an accessory" bundles on Clothing orders and a free-shipping threshold around $75.
2. **Run a targeted acquisition test for women.** They convert to the same AOV as men, so a 10-point gain in female share is worth about $23K on this base. Start with Clothing and Accessories campaigns, since that is where the volume is.
3. **Redesign the subscription.** Subscribers get discounts on every order. Replace blanket discounts with perks tied to higher spend, such as early access, free shipping above a threshold or tiered rewards. Run it as an A/B test.
4. **Cut discount depth on items that sell without help.** Hat, Sneakers and Coat are discounted on roughly half of orders with no visible AOV gain. Hold discounts back for a test group and compare volume.
5. **Fix or retire the weak product lines.** Review Jeans and Shirt (the lowest ratings) for fit, quality or pricing issues. Feature the best-rated items (Gloves, Sandals, Boots) in merchandising.
6. **Review Outerwear pricing.** The lowest AOV in a category that should be premium suggests heavy discounting, a thin range or missing high-end SKUs.
7. **Move new customers toward subscription.** Only 18% of New customers subscribe vs 28% of Loyal. A post-first-purchase offer is the natural place to test.

## Caveats

- The dataset has no cost, margin, date or repeat-order data. All findings are hypotheses to test, not proof of cause. The discount finding, for example, is observational.
- The Loyal segment is 80% of customers (3,116 of 3,900) because Loyal is defined as more than 10 previous purchases. New is only 83 customers. A tighter threshold, such as quartiles of previous purchases, would make the segmentation more useful.

## Note

- AI was leveraged to build this project