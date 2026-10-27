# Customer Analytics: Segmentation and Repeat-Purchase Prediction

Predicts which retail customers will buy again in the next 90 days, estimates how much they will spend, and groups customers into RFM segments.

**Live dashboard:** https://public.tableau.com/views/CustomerSegmentsandRepeat-PurchasePrediction/CustomerSegmentsandRepeat-PurchasePrediction?:language=en-US&:sid=&:redirect=auth&:display_count=n&:origin=viz_share_link

## Data
UCI Online Retail II: about 1.07M transactions from a UK online retailer (Dec 2009 to Dec 2011). After removing rows with no Customer ID, cancellations, and zero or negative quantities or prices, 805,549 rows and 5,878 customers remained.

## Method
- Cutoff date of 2011-09-09. Features were built only from purchases before the cutoff. Targets came from the 90 days after it, so there is no data leakage.
- 5,278 customers had purchase history before the cutoff. Features: recency, frequency, monetary value, total items, unique products, tenure and average order value.
- SQL (SQLite) was used for exploration. Buy-again rate rises from 20% for one-order customers to 84% for customers with 10+ orders.

## Models (scikit-learn)
| Task | Model | Test result |
|---|---|---|
| Will the customer buy again in 90 days? | Random forest classifier | AUC 0.778 |
| How much will they spend? | Random forest regressor | MAE $390, R2 0.565 |
| Customer segments | K-Means (k=4) on log-scaled RFM | 4 segments |

Results are from a held-out 20% test set. Scores in the dashboard file were produced by models trained on all customers, so they look better than they would on new data.

## Segments
- **Champions** (884 customers): recent, frequent, high spend. Average predicted chance of buying again: 82.5%
- **New / recent** (647): recent buyers with few orders. 56.0%
- **Lapsing loyalists** (1,622): decent spend but about 195 days since last order. 46.8%
- **Lost / one-time** (2,125): about 342 days since last order, 1.4 orders on average. 20.5%

## Limitations
- The 90-day window covers Sep to Dec, the retailer's peak season, so results may not hold for other periods.
- About 600 customers who first bought after the cutoff were excluded, since they have no history to build features from.
- Spend is heavily skewed by a few wholesale buyers, so the regression is best described as moderate.

## Files
- `notebooks/`: conversion, customer table, SQL exploration, models
- `dashboards/customers_scored.csv`: scored customers used in Tableau
