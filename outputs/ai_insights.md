# AI-generated insights

## Key findings

- **Scope.** After cleaning, the data covers 01 Dec 2009 to 09 Dec 2011: 39,521 invoices, 5,853 registered customers and £19,413,406 of revenue. The average order is £491, but the median order is only £301, so a minority of large (wholesale) orders pulls the average up.

- **Growth.** For the 11 months present in both 2010 and 2011 (partial final month excluded), revenue changed by +2.1% (£8,605,482 to £8,786,758).

- **Seasonality.** The strongest complete month is 2011-11 with £1,453,266 in revenue.

- In 2010, September to November delivered 36% of the year's revenue (the build-up to Christmas).

- **Geography.** The UK generates 85% of revenue. The leading export markets are EIRE (£633,269), Netherlands (£549,773), Germany (£383,289).

- **Products.** The three best sellers by revenue are Regency Cakestand 3 Tier (£330,590); White Hanging Heart T-Light Holder (£260,990); Party Bunting (£148,318), together 3.8% of revenue.

- **Customers.** The top 10% of registered customers produce 63% of registered-customer revenue. Guest checkouts account for 7% of invoices and 13% of revenue.

- **Timing.** Thursday is the strongest trading day and 12:00 is the strongest hour. The weakest weekday is Saturday.

- **Cancellations.** 1.8% of product lines are cancelled, worth £471,132 (2.4% of net revenue).

- **Data quality.** Cleaning removed duplicates (including the December 2010 sheet overlap), cancellations, adjustments, non-product codes and fully reversed giant orders. 94.0% of the raw rows were kept; outliers were capped, not deleted, to preserve legitimate wholesale demand.

- **Prediction.** Using only order size and context (no price information), the best model for flagging high-value invoices (above £301) is Decision Tree with 83.9% test accuracy (F1 0.84, ROC-AUC 0.92) against a 50% majority-class baseline. The most informative inputs are total quantity, number of lines, hour of day. Linear Regression explains 80% of the variance in log invoice value.

## Recommendations

1. Revenue is growing: keep investing in what works (see products and customers below).
2. Plan stock, staffing and marketing around the autumn peak: September to November produced 36% of 2010 revenue.
3. Revenue is concentrated in the UK (85%). Test targeted promotions in the top export markets to diversify.
4. Revenue depends on a small group of customers (63% from the top decile). Build a retention programme (account managers, volume pricing) for them.
5. Schedule promotions and customer-service cover around Thursdays and the 12:00 hour.
6. Use the high-value probability from the model (exported for the dashboard) to prioritise sales follow-up on large pending orders and to forecast demand.
