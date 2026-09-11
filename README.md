
# Snapdeal — Customer Purchasing Behavior & Product Recommendation Analysis

An end-to-end analysis of an 800-respondent Snapdeal customer survey, covering data
cleaning, descriptive behavior analysis, customer segmentation, recommendation/review
effectiveness, and market basket analysis — translated into concrete business
recommendations.

## Project Overview

Snapdeal collected survey responses from 800 customers spanning demographics, shopping
habits, cart behavior, product search/browsing patterns, reviews, and satisfaction ratings.
This project cleans that raw data, explores it for behavioral patterns, segments customers
both by business rules and by unsupervised clustering, tests whether recommendations and
reviews actually drive satisfaction, and mines product-category co-purchase patterns with
market basket analysis (Apriori). The findings are summarized into prioritized,
actionable recommendations for the business.

## Contents

| File | Description |
|---|---|
| `SnapDeal.csv` | Raw source dataset — 800 rows × 24 columns, one row per survey respondent |
| `SnapDeal_cleaned.csv` | Cleaned dataset produced by the notebook (standardized categories, parsed timestamps, numeric ratings, derived fields) |
| `Snapdeal_Analysis.ipynb` | Full analysis notebook: cleaning → EDA → segmentation → recommendation/review analysis → market basket analysis |
| `Snapdeal_Findings_Presentation.pptx` | 14-slide stakeholder presentation summarizing findings and next steps |
| `README.md` | This file |

## Dataset

The raw data (`SnapDeal.csv`) contains one row per respondent with fields covering:

- **Demographics:** age, gender
- **Purchase behavior:** purchase frequency, purchase categories (multi-select), browsing
  frequency, product search method, cart behavior (add-to-cart, completion, abandonment
  factors, save-for-later)
- **Reviews:** whether a review was left, perceived review reliability/helpfulness
- **Recommendations:** personalized recommendation frequency and perceived helpfulness
- **Satisfaction:** rating accuracy, shopping satisfaction, service appreciation, open-text
  improvement areas
- **Metadata:** timestamp, transaction ID

`SnapDeal_cleaned.csv` is the output of the cleaning steps in the notebook: trimmed/
deduplicated column names, standardized category labels, `.`/blank values treated as
missing, ratings coerced to numeric types, timestamps parsed, and multi-select purchase
categories exploded into a `Purchase_Categories_List` field for market basket analysis.

## Notebook Structure (`Snapdeal_Analysis.ipynb`)

1. **Data Cleaning and Preparation** — fix column names (including a duplicated
   `Personalized_Recommendation_Frequency` column), remove duplicate rows/transactions,
   parse timestamps, standardize categorical labels, convert ratings to numeric, and split
   multi-select purchase categories into lists.
2. **Descriptive Behavior Analysis** — demographics, purchase frequency, category mix,
   browsing/search methods, cart abandonment factors, and distribution of the four core
   rating metrics.
3. **Customer Segmentation and Profiling** — rule-based segments (Frequent Buyer, At-Risk,
   etc.) plus K-Means clustering (k=4) on 12 behavioral features, visualized with PCA and a
   standardized heatmap.
4. **Recommendation and Review Insights** — correlation analysis testing whether
   recommendation helpfulness and review trust move satisfaction/rating accuracy.
5. **Market Basket Analysis and Visual Summary** — Apriori association rule mining on
   purchase categories to find products frequently bought together.
6. **Key Takeaways** — consolidated, prioritized recommendations.

### Requirements

The notebook uses:
```
pandas, numpy, matplotlib, seaborn, scikit-learn, mlxtend
```
`mlxtend` (used for Apriori/association rules) is installed automatically at the top of the
notebook if not already present.

### Running it

```bash
pip install pandas numpy matplotlib seaborn scikit-learn mlxtend
jupyter notebook Snapdeal_Analysis.ipynb
```
Run all cells in order — later sections depend on the cleaned DataFrame produced in
Section 1.

## Key Findings

- **56% of customers are "At-Risk"** on satisfaction/cart-completion grounds, and a
  data-driven cluster of high-frequency-but-low-satisfaction ("Frequent but Unhappy")
  customers represents the most revenue at risk.
- **Price and shipping — not app usability — drive cart abandonment.** These outrank any
  interface-related complaint.
- **Today's recommendations and reviews barely move the needle.** Correlations between
  recommendation helpfulness / review trust and satisfaction / rating accuracy are all weak
  (roughly -0.07 to +0.10); only ~30% of customers find personalized recommendations
  consistently helpful.
- **Clothing and Fashion is the basket's anchor category**, appearing as the top
  consequent in association rules (up to ~74% confidence when Groceries, Home & Kitchen,
  and Other are already in the basket) — a natural fit for cross-sell and bundle
  promotions.
- **Behavior segments the customer base far better than demographics.** Age and gender
  show little differentiation; purchase frequency, cart completion, and satisfaction do.

## Recommendations

1. **Retention first** — launch win-back offers targeted at At-Risk and "Frequent but
   Unhappy" segments.
2. **Reduce price/shipping friction** — address the top two cart-abandonment drivers
   directly (e.g., discount thresholds, free-shipping tiers).
3. **Rebuild recommendations around basket affinity** — replace generic popularity-based
   logic with the Clothing & Fashion-anchored association rules from the market basket
   analysis.
4. **Segment by behavior, not demographics** — target campaigns using purchase frequency,
   cart completion, and satisfaction rather than age/gender.
5. **Invest in review quality signals** (verified-purchase badges, helpful-vote counts)
   rather than raw review volume, since review trust doesn't track with perceived rating
   accuracy.

## Presentation

`Snapdeal_Findings_Presentation.pptx` is a 14-slide summary for a non-technical audience,
structured as:
- **Agenda** — what's in the data, what we found, what it means
- **Part 1 — What's there:** dataset overview
- **Part 2 — What we found:** demographics, category mix, cart abandonment drivers,
  satisfaction distribution, segmentation (rule-based and clustering), recommendation/review
  effectiveness, and basket analysis
- **Part 3 — What this means:** recommendation strategy shift and suggested next steps
- **Questions & discussion**
