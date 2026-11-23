## Seasonal and weather effects on bike-sharing demand

#### Business analytics portfolio project

### Project overview
Bike-sharing operations need to anticipate changes in rental demand to plan bicycle availability, staffing, rebalancing and maintenance. This project investigates whether average daily bike rentals differ by season and weather conditions, and weather the clear vs. cloudy demand difference changes across seasons.

Business questions:
1. Does average daily rental demand differ across seasons?
2. Does demand differ between clear and misty/cloudy conditions?
3. Does the weather-related difference vary across seasons?

#### Dataset

Source: UCI Bike Sharing Dataset
System and period: Capital Bikeshare, Washington, D.C., 2011–2012
Unit of analysis: One day; total daily rentals (cnt)
Original dataset: 731 daily observations
Analytical sample: 710 observations, restricted to clear and misty/cloudy conditions
Independent variables: Season (four categories) and weather (two categories)
Control variables: Year and working-day status
The primary analysis uses a 4 × 2 factorial design. The 21 excluded observations belong to less common weather categories.

#### Analytical methodology

The analysis follows six stages:
1. Data preparation and EDA: Assess data quality, calculate descriptive statistics, and visualize rental demand patterns.
2. Two-way ANOVA: Test the main effects of season and weather and their interaction using Type III sums of squares and sum-to-zero contrasts.
3. Assumption diagnostics: Examine residual normality, homogeneity of variance, and temporal autocorrelation.
4. Adjusted regression: Include year and working-day status to account for additional demand variation.
5. Robustness analysis: Use HC3 and HAC-robust inference, including sensitivity tests across HAC lag specifications.
6. Post-hoc analysis: Estimate seasonal weather contrasts, perform direct between-season comparisons, and apply Holm's multiple-testing correction.

7. Technologies: Python, Pandas, NumPy, Matplotlib, Seaborn, SciPy, Statsmodels and Patsy

### Key Findings

#### 1. Adjusted Model Performance
The baseline model explained 41.5% of observed variation in daily rentals (R² = 0.415).
After controlling for year and working-day status, the adjusted model explained 73.9% (R² = 0.739).
This represents an improvement of 32.4 percentage points in in-sample explained variation.

#### 2. Season and Weather Effects
Both season and weather remained strongly associated with daily rental demand under seven-lag HAC-robust inference (p < 0.0001).
The analysis identified positive clear-versus-cloudy demand contrasts across all four seasons. All four within-season comparisons remained statistically significant after Holm correction under both seven- and fourteen-lag HAC specifications.

#### 3. Season × Weather Interaction
The overall Season × Weather interaction was sensitive to HAC lag selection. Across four lag specifications, the interaction p-values ranged from 0.0384 to 0.0537.
This provides suggestive, but not consistently conclusive, evidence that weather-associated demand differences vary by season.

#### 4. Spring-Specific Differences
Spring exhibited the largest estimated clear-versus-cloudy demand contrast.Direct comparisons showed that its weather contrast was approximately 597 rentals/day greater than winter's, with Holm-adjusted p-values of 0.0321 (seven lags) and 0.0280 (fourteen lags).
However, the corresponding differences relative to summer and fall were not statistically significant after correction.

### Visualizations

#### Adjusted rental demand by season and weather
<img width="2963" height="1452" alt="03_spring_interaction_comparisons" src="https://github.com/user-attachments/assets/6f0077f0-f73b-4cd9-ac88-b6b4cc66ef60" />
The interaction plot shows model-adjusted mean daily demand across seasons and weather conditions, accounting for year and working-day status.

#### Seasonal weather contrasts and confidence intervals
<img width="2955" height="1602" alt="02_weather_contrasts_ci" src="https://github.com/user-attachments/assets/175c4db9-4350-491c-8942-9c9f55c0fef1" />
The figure shows the magnitude and uncertainty of the adjusted clear-versus-cloudy demand contrasts in each season.

#### Spring versus other seasons
<img width="2960" height="1752" alt="01_adjusted_interaction" src="https://github.com/user-attachments/assets/32704c5c-1d45-4293-bc85-c0ec5e28ce0d" />
Direct comparisons investigate whether the weather contrast in spring differs from those of other seasons.

The displayed confidence intervals are individual 95% intervals. Statistical conclusions after multiple testing use Holm-adjusted p-values.

### Business recommendations

1. Incorporate seasonality into fleet planning. Use historical seasonal demand patterns to inform bicycle availability, staffing, and rebalancing decisions.
2. Evaluate weather-aware demand forecasting. Compare a calendar-based forecasting model with one that incorporates forecast weather conditions. Assess performance using chronological out-of-sample validation and operational metrics.
3. Investigate spring-specific operational flexibility. Spring exhibited the largest estimated weather contrast, although evidence for differences relative to summer and fall was insufficient after correction.
4. Explore maintenance scheduling during lower-demand periods. Assess whether periods of forecast lower demand offer opportunities for nonurgent maintenance without compromising bicycle availability or safety.

5. ### Limitations

Observational data: The findings establish associations rather than causal effects.
Temporal autocorrelation: The adjusted model retained positive residual autocorrelation (Durbin–Watson = 0.908).
HAC limitations: Excluding 21 days creates irregular spacing in the retained observations; HAC lags do not correspond exactly to calendar-day intervals.
Historical data: The dataset covers 2011–2012 and may not reflect current demand patterns.
Missing operational information: Station-level capacity, fleet constraints, and costs were not available.
Interaction uncertainty: The overall Season × Weather interaction was sensitive to HAC lag selection.

### Reproducing the analysis
Clone the repository:
git clone https://github.com/mrsmv/bike-sharing-anova.git
cd bike-sharing-anova
Install the required Python packages:
python -m pip install numpy pandas matplotlib seaborn scipy statsmodels patsy notebook
Launch the notebook:
jupyter notebook 02_notebooks/01_bike_demand_anova.ipynb
The input dataset is located at 01_data/01_raw/day.csv.
Run the notebook from beginning to end. The final visualization cells export their figures to reports/figures/.

### Dataset attribution

Fanaee-T, H. (2013). Bike Sharing [Dataset]. UCI Machine Learning Repository.
https://doi.org/10.24432/C5W894
