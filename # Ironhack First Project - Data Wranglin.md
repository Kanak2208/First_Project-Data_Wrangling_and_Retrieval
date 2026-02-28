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

## Research Questions
1. Do obesity/BMI indicators vary significantly across continents?
2. Do continents with lower BMI tend to show higher happiness levels?
3. Is BMI related to life expectancy?

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
Visualizations created in the notebook:
1. **Average BMI by Continent** (bar chart)
2. **Average BMI vs Happiness Level by Continent** (grouped mean bar chart)
3. **BMI vs Happiness Level** (regression plot)
4. **BMI vs Life Expectancy** (regression plot)
5. **Observations per Continent by Gender** (countplot)

---

## Key Findings (From Notebook Analysis)
- BMI levels differ across continents (supporting H1).
- Continents with lower BMI tend to show higher happiness levels; the notebook highlights **Asia** as an example (supporting H2).
- The regression plots suggest a relationship pattern between BMI and life expectancy (direction/strength depends on data aggregation and limitations).

---

## Limitations
- The second dataset is city-based and then mapped to continents, while the API is region/continent-based.
- The merge is continent-level, which reduces granularity (country-level matching would improve accuracy).

