# Ironhack First Project - Data Wrangling and Retreival
BMI, Happiness, and Life Expectancy Across Continents  

## Introduction
This project explores how obesity-related indicators (BMI) vary across continents and whether continents with lower BMI tend to have higher happiness levels and better health outcomes (life expectancy).  
We combine data from a public health API with a separate dataset containing quality-of-life metrics and perform cleaning, wrangling, merging, and exploratory data analysis (EDA).

## Project Requirements Covered
- Python (data structures, functions-ready workflow, transformations)
- Data collection using an API
- Two data sources (API + dataset)
- Data cleaning & wrangling (nulls, duplicates, formatting, mapping, merging)
- EDA + visualizations
- Narrative conclusions based on evidence

---

## Data Sources

### 1) WHO GHO API (API Source)
- **Endpoint used:**  
  `https://ghoapi.azureedge.net/api/NCD_BMI_30A?$top=1000`
- **Data pulled includes:**  
  - `ParentLocation` (region/continent)
  - `TimeDim` (year)
  - `Dim1` (gender code)
  - `Value` (BMI/obesity indicator values)

### 2) CSV Dataset (Second Source)
- **Source used in notebook (Google Sheets CSV export):**  
  `https://docs.google.com/spreadsheets/d/1iNp4gN_YY8yn1SwEdnfx0nD1gWGBSwhc0fE9TqXzX_I/export?format=csv`
- **Used fields after cleaning/renaming:**  
  - `continent` (mapped from City)
  - `happiness_level`
  - `life_expectancy`
  - `obesity_level`
  - `gym_membership`  
  *(additional columns were dropped to focus analysis)*

---

## Hypotheses
- **H1:** “Obesity levels vary significantly across continents.”
- **H2:** “Continents with lower obesity levels tend to show higher happiness levels and healthier living conditions.”

---

## Methodology

### Data Collection
- Retrieved WHO BMI indicator data via `requests.get(url).json()`
- Loaded the second dataset directly via `pd.read_csv(csv_url)`

### Data Cleaning & Wrangling (Main Steps)
**WHO API dataframe (`df_obesity`)**
- Selected key columns: `ParentLocation`, `TimeDim`, `Dim1`, `Value`
- Renamed columns to: `continent`, `year`, `gender`, `bmi`
- Converted gender codes:
  - `SEX_MLE → M`, `SEX_FMLE → F`, `SEX_BTSX → N`
- Extracted numeric BMI values from strings and converted to float
- Dropped rows with missing continent
- Removed duplicate rows

**CSV dataset dataframe (`df`)**
- Dropped non-essential columns (rank, sunshine hours, pollution index, etc.)
- Mapped **City → Continent**
- Renamed important columns for consistency:
  - `Obesity levels(Country)` → `obesity_level`
  - `Life expectancy(years) (Country)` → `life_expectancy`
  - `Happiness levels(Country)` → `happiness_level`
  - `Cost of a monthly gym membership(City)` → `gym_membership`
- Standardized column names to lowercase

### Continent Standardization (Important Challenge)
The two sources used different continent/region categories, so we aligned them:

**Dataset (`df`) adjustments**
- `North America` + `South America` → `Americas`
- `Europe/Asia` → `Asia`

**API (`df_obesity`) adjustments**
- `Western Pacific` → `Australia`
- `Eastern Mediterranean` → `Europe`
- `South-East Asia` → `Asia`

### Combining Data
- Dropped duplicates in `df` to keep one row per continent
- Merged:
  - `data = df_obesity.merge(df_new, on='continent', how='left', validate='many_to_one')`

---

## Exploratory Data Analysis (EDA)

After cleaning and merging the datasets at the continent level, we conducted exploratory data analysis to evaluate our hypotheses.

### 1. Obesity Variation Across Continents

We analyzed the average BMI by continent using boxplots and statistical significance testing.

Key observations:
- The top three continents with the highest BMI averages:
  1. Australia
  2. Americas
  3. Europe
- Australia showed the highest variability in BMI values.
- Europe and Africa displayed a higher number of outliers.

Statistical Testing:
We performed pairwise significance testing (p-value matrix).

- Australia shows statistically significant differences (p < 0.05) compared to all other continents.
- Africa and Asia are statistically similar.
- Europe and Americas are statistically similar.

Conclusion:
The hypothesis that “Obesity levels vary significantly across continents” is supported.

---

### 2. BMI and Happiness Relationship

We compared average BMI and happiness level by continent.

Findings:
- Continents with lower BMI values (Asia and Africa) do not consistently show the highest happiness levels.
- Europe and Americas show high happiness levels despite moderate BMI.
- Australia has high BMI and relatively high happiness.

Conclusion:
The relationship between BMI and happiness is not strictly inverse.
The hypothesis that “Continents with lower obesity levels tend to show higher happiness levels” is rejected.

---

### 3. BMI and Life Expectancy

We compared BMI and life expectancy averages by continent.

Findings:
- Asia shows the highest life expectancy despite low BMI.
- Australia and Europe also show high life expectancy with higher BMI values.
- Africa has both lower life expectancy and lower BMI.

Conclusion:
There is no clear linear relationship between BMI and life expectancy at the continent level.
Other socio-economic and healthcare factors likely influence life expectancy more strongly.

---

## Key Findings

1. BMI levels vary significantly across continents.
   - Australia has the highest average BMI and shows significant differences compared to other continents.
   - Africa and Asia are statistically similar.
   - Europe and Americas are statistically similar.

2. The hypothesis that obesity levels vary across continents is accepted.

3. The hypothesis that lower obesity leads to higher happiness is rejected.
   - High happiness levels are observed in Europe and Americas despite moderate BMI.
   - The relationship between BMI and happiness is not directly proportional.

4. No strong linear relationship was found between BMI and life expectancy at the continent level.

5. Aggregating data at the continent level may hide country-level variations.
---
## Final Conclusion

Our analysis confirms that obesity levels differ significantly across continents, supporting our first hypothesis.

However, the assumption that lower BMI automatically leads to higher happiness and life expectancy was not fully supported by the data.

This suggests that quality of life indicators such as happiness and longevity are influenced by multiple factors beyond BMI alone, including healthcare systems, economic stability, and social well-being.

Future improvements:
- Conduct country-level analysis instead of continent-level aggregation.
- Study gender-based differences in BMI.
- Integrate lifestyle and dietary datasets.
- Perform deeper statistical modeling.

---
## Limitations
- The second dataset is city-based and then mapped to continents, while the API is region/continent-based.
- The merge is continent-level, which reduces granularity (country-level matching would improve accuracy).

## Links

- WHO GHO API Endpoint:  
  https://ghoapi.azureedge.net/api/NCD_BMI_30A?$top=1000

- CSV Dataset (Google Sheets Export):  
  https://docs.google.com/spreadsheets/d/1iNp4gN_YY8yn1SwEdnfx0nD1gWGBSwhc0fE9TqXzX_I/export?format=csv

- Presentation Slides (Google Slides):  
  https://docs.google.com/presentation/d/1VO7XePg9diHPlvmG-8PD4PMAulUvEJjsWGNvZXKoLmg/edit?usp=sharing

- Kanban Board (Canva):  
  _ Canva link_
