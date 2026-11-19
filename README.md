# Bumble Profile Analysis — Exploratory Data Analysis (EDA) Case Study

## Project Overview

Bumble is a dating platform where users connect based on mutual interests and compatibility. To improve matchmaking, Bumble collects profile details covering demographics, lifestyle habits, and personal preferences.

This project analyzes **59,946 user profiles** (`bumble.csv`) in Python (`Bumbel_MileStone_4.ipynb`) to uncover user demographics, lifestyle trends, income patterns, geographic concentration, and profile completeness gaps—and translates those findings into practical product, marketing, and monetization recommendations.

---

## Business Problem

To grow match quality and subscription revenue, Bumble needs clear answers to four operational questions:

1. **Marketplace Balance:** How skewed is the user base by gender and age, and where does supply/demand imbalance hurt match quality?
2. **Profile Data Gaps:** Which profile fields have the highest non-disclosure rates, and how do missing values limit matchmaking filters?
3. **Lifestyle Compatibility:** How do habits like drinking, diet, and pet ownership cluster across age and gender groups?
4. **Local Monetization Fit:** How do age and income vary across cities, and how should pricing and promotions adapt?

---

## Dataset Summary

| Attribute | Details |
| :--- | :--- |
| **Source File** | `bumble.csv` (analyzed in `Bumbel_MileStone_4.ipynb`) |
| **Raw Dimensions** | 59,946 rows × 17 columns |
| **Cleaned Dimensions** | 59,010 rows × 25 columns (17 original + 8 engineered features) |
| **Rows Removed** | 936 rows (invalid ages > 70, heights outside 58–78 in, extreme incomes > $220,000) |
| **Geographic Scope** | 99.85% California (58,920 profiles), heavily concentrated in the San Francisco Bay Area |

**Original 17 Columns:** `age`, `status`, `gender`, `body_type`, `diet`, `drinks`, `education`, `ethnicity`, `height`, `income`, `job`, `last_online`, `location`, `pets`, `religion`, `sign`, `speaks`.

---

## Tools & Tech Stack

- **Language:** Python 3
- **Data Manipulation:** `pandas`, `numpy`
- **Data Visualization:** `matplotlib`, `seaborn`
- **Environment:** Jupyter Notebook (`Bumbel_MileStone_4.ipynb`)

---

## EDA Workflow (Steps Taken)

1. **Data Inspection & Missing Value Audit:** Checked all 17 columns for explicit `NaN` values (`diet`: 40.69%, `religion`: 33.74%, `pets`: 33.23%, `sign`: 18.44%, `job`: 13.68%). Confirmed no column exceeded 50% explicit missingness.
2. **Hidden Missing Values (`income`):** Identified **48,442 rows (80.81%)** coded as `-1` (undisclosed income). Converted `-1` to `NaN` and kept them missing rather than imputing, since non-disclosure reflects user privacy choices.
3. **Imputation & Type Conversion:**
   - Imputed 3 missing `height` values with the median.
   - Imputed 9 categorical columns with `"Unknown"` to preserve sample size; left `speaks` (50 NaNs, 0.08%) unchanged.
   - Converted `last_online` from string (`object`) to `datetime64`.
4. **Outlier Filtering (IQR + Domain Judgment):**
   - `age`: Kept ages `18–70` to preserve genuine mature users; dropped 2 impossible rows (`109`, `110`).
   - `height`: Dropped unrealistic heights outside `58–78` inches.
   - `income`: Dropped 718 extreme upper-tail outliers above the `$220,000` IQR fence (`$250k`, `$500k`, `$1M`), leaving **10,722** valid income records (max `$150,000`).
5. **Feature Engineering:** Created `age_group`, quartile-based `income_group` (`Low`, `Medium`, `High`), `profile_completeness` (across 10 core fields), `height_cm`, `city`, `state`, `sign_fixed`, `just_sign`, and binary pet preference flags (`likes dogs`, `likes cats`, `has dogs`, `has cats`, `dislikes dogs`, `dislikes cats`).
6. **Analysis & Visual Refinement:** Explored demographic distributions, correlations, lifestyle cross-tabs, city-level income/age splits, and profile completeness. Replaced cluttered visuals (such as a 12-slice zodiac pie chart) with sorted horizontal bar charts.

---

## Key Findings

### Core Metrics (n = 59,010 Cleaned Profiles)

| Metric | Result |
| :--- | :--- |
| **Gender Split** | 59.45% Male (35,079) vs. 40.55% Female (23,931) — 18.90 pp gap |
| **Primary Age Group** | 26–35 years: 47.77% (28,189); Overall Mean Age: 32.33 years |
| **Relationship Status** | 92.95% Single (54,847); 3.43% Seeing Someone; 3.09% Available; 0.52% Married |
| **Disclosed Income (n = 10,722)** | Mean: $57,369.89 | Median: $50,000 | Q1: $20,000 | Q3: $80,000 |
| **Age–Income Correlation** | `r = 0.35` (moderate positive; peaks in ages 36–45 at $74,627.98) |
| **Drinking Habits** | 69.87% Social Drinkers; strict vegans (25.89%) and strict halal (43.75%) abstain most |
| **Pet Preferences** | 28,449 like dogs; 20,958 like cats; women own dogs (17.91% vs. 11.53%) and cats (17.81% vs. 8.25%) at higher rates |
| **Profile Completeness (10 fields)** | ~80% average across age/gender; highest for 45+ (81.15%); lowest for Unknown Job (60.95%) |

### Top 5 Cities by Profile Volume

| City | Profiles (% of Total) | Avg Age | Avg Disclosed Income | High-Income Share |
| :--- | :--- | :--- | :--- | :--- |
| **San Francisco** | 30,558 (51.78%) | 31.63 yrs | $65,361.05 (n = 4,667) | 29.16% |
| **Oakland** | 7,083 (12.00%) | 33.22 yrs | $46,414.18 (n = 1,439) | 10.70% |
| **Berkeley** | 4,155 (7.04%) | 31.42 yrs | $44,761.90 (n = 693) | 11.54% |
| **San Mateo** | 1,305 (2.21%) | 33.44 yrs | $64,983.16 (n = 297) | 27.27% |
| **Palo Alto** | 1,053 (1.78%) | 32.02 yrs | $73,101.60 (n = 187) | 37.43% |

---

## Business Recommendations

1. **Narrow the Gender Gap:** Run targeted female-acquisition campaigns, referral incentives, and local community events in San Francisco and Oakland to bring the 6:4 male-to-female ratio closer to balance.
2. **Lift Profile Completeness Without Friction:** Use one-tap lifestyle badges (diet, pets, job) to nudge profiles below 75% completeness—especially users missing a job field (currently 60.95% complete). Keep income optional given 80.81% non-disclosure.
3. **Strengthen Lifestyle Matching Filters:** Highlight pet ownership/affinity filters (high value for female and 36+ users) and pair diet with drinking filters for health-focused or alcohol-free segments.
4. **Tier Pricing by Local Market & Age:** Focus premium subscriptions and boosts on affluent hubs (Palo Alto, San Francisco, San Mateo) and ages 26–45, while offering lower-cost passes or student bundles in younger, lower-income markets like Berkeley (33.50% aged 18–25).

---

## Data Limitations

- **Regional Concentration:** 99.85% of profiles are in California (over half in San Francisco). Findings reflect the SF Bay Area urban market and should not be generalized nationally or globally.
- **Static Profile Data Only:** The dataset does not include swipe behavior, match rates, messaging activity, or essay text. Combining profile data with engagement logs is the recommended next step for predictive modeling.
