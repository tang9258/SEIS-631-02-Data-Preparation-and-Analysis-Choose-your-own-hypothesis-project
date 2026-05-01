# Are Used Electric Vehicles Priced Higher Than Gas Vehicles?

## 1. Research Question

Are used electric vehicle (EV) listings on Craigslist priced higher than used gas powered vehicle listings?

As EV adoption is increasing, understanding pricing differences can provide insight into market trends and affordability. This question matters because it can help buyers and policymakers understand the cost premium (if any) of choosing an EV in the used car market.


## 2. Hypothesis

1. Null Hypothesis (H₀): There is no difference in listing price between EV and gas vehicles on Craigslist.
2. Alternative Hypothesis (H₁): EV listings have higher prices than gas vehicle listings.


## 3. Data Description

Source: `car_listings.csv` — Craigslist used car listings (local dataset provided for this course).

Unit of Analysis: One row is one vehicle listing.

Full dataset size: About 4.5 million rows, 23 columns.

Columns used: `price` (USD asking price), `fuel` (gas / electric / …), `year`, `make`, `model`, `odometer`.

After cleaning:

1. Removed rows with missing or zero `price`.
2. Removed extreme price outliers (kept $500 – $100,000).
3. Filtered to `fuel == "electric"` (EV) or `fuel == "gas"` (Gas).
4. Created `vehicle_type` column: `"EV"` or `"Gas"`.

Data: `car_listings.csv` is stored locally in the `data/` folder and is not committed to this repository due to its large size. The file was provided by the course instructor.


## 4. Methods

Test statistic: Difference in median listing price (EV minus Gas).

Why median? Car prices are right skewed with extreme outliers; the median is robust to these. The Central Limit Theorem (CLT) applies well to means, but not reliably to medians, so bootstrapping is used for uncertainty estimation.

Permutation test: Labels ("EV" / "Gas") were shuffled 1,000 times; the median difference was recomputed each time to build a null distribution. The one sided p value counts how often a random shuffle produced a difference at least as large as the observed value.

Bootstrap CI (EV median): 1,000 resamples with replacement from the EV price array; 2.5th and 97.5th percentiles give the 95% CI.

Bootstrap CI (difference): 1,000 paired resamples (one from each group); median difference computed each time; 2.5th and 97.5th percentiles give the 95% CI.


## 5. Results

Rows after cleaning: 293,519 (1,341 EV, 292,178 Gas).

Median EV listing price: $24,000.

Median Gas listing price: $6,000.

Observed median difference (EV minus Gas): $18,000.

Permutation test p value (one sided): 0.0000.

95% Bootstrap CI for median EV price: [$22,900, $25,000].

95% Bootstrap CI for median difference: [$16,950, $19,000].

Visualizations in the notebook:

1. Overall price histogram.
2. Overlaid histograms (EV vs Gas).
3. Boxplot comparison.
4. Permutation null distribution (with observed statistic marked).
5. Bootstrap distribution for EV median (with 95% CI shaded).
6. Bootstrap distribution for median difference (with 95% CI shaded).


## 6. Uncertainty Estimation

1,000 resamples were used for both the permutation test and both bootstrap confidence intervals.

The permutation distribution was centered around 0 (as expected under H₀); the observed difference fell in the tail — the p value tells us how far.

The bootstrap distributions were roughly bell shaped; the 95% CIs are printed in the notebook output.

Because the median is not a mean or proportion, the CLT does not apply cleanly. Bootstrapping is the correct, assumption free method for building confidence intervals here.


## 7. Limitations

Selection bias: This is Craigslist data only. Prices may differ significantly on dealership lots or other platforms.

No control variables: EVs may be newer or lower mileage on average. The raw price gap may partly reflect differences in vehicle age, mileage, and brand composition rather than fuel type alone.

Small EV sample: The dataset contains far fewer EV listings than gas listings, which results in wider bootstrap confidence intervals for EV specific estimates.

Asking price is not the same as sale price: Craigslist prices are asking prices; final negotiated prices may differ.

Snapshot in time: The dataset reflects listings from a specific period. EV prices and market share have shifted considerably over recent years.


## 8. References

Dataset: Craigslist Used Cars dataset `car_listings.csv` (Provided in class)
