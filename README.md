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

## Research Questions & Hypotheses

### Hypothesis 1 – Obesity Variation  
Obesity levels vary across continents due to lifestyle and dietary differences.

### Hypothesis 2 – Happiness Correlation  
Continents with lower obesity levels tend to show higher happiness levels.

### Guiding Questions

- How does BMI differ by continent?
- Is BMI related to happiness?
- Is BMI related to life expectancy?

---

## Data Sources

### 1) WHO GHO API (Primary Data Source)

Endpoint used:  
https://ghoapi.azureedge.net/api/NCD_BMI_30A?$top=1000  

Fields extracted:
- ParentLocation (continent/region)
- TimeDim (year)
- Dim1 (gender)
- Value (BMI indicator)

Data was retrieved via an HTTP GET request and normalized from JSON into a Pandas DataFrame.

---

### 2) Kaggle Dataset – Healthy Lifestyle Cities Report 2021

Source:  
https://www.kaggle.com/datasets/prasertk/healthy-lifestyle-cities-report-2021  

Fields used:
- City
- Continent (mapped from city)
- Happiness level
- Life expectancy
- Obesity level
- Gym membership cost

The dataset was cleaned and adjusted to match continent categories from the API.

---

## Methodology

### Data Cleaning & Wrangling

Key steps applied:

- Selected relevant columns and renamed them for clarity
- Cleaned gender labels:
  - SEX_MLE → M
  - SEX_FMLE → F
  - SEX_BTSX → N
- Extracted numeric BMI values from strings and converted to float
- Removed null continents and duplicate rows
- Mapped cities to continents
- Standardized continent names across both datasets:
  - North/South America → Americas
  - Western Pacific → Australia
  - Eastern Mediterranean → Europe
  - South-East Asia → Asia

---

### Combining the Data

- Removed duplicate continent rows from the lifestyle dataset
- Merged both datasets on `continent`
- Used `validate='many_to_one'` to ensure merge integrity

This allowed multiple BMI records per continent to match with a single row of lifestyle metrics.

---

## Exploratory Data Analysis (EDA)

### 1) Obesity Variation Across Continents

We analyzed BMI distribution using boxplots and statistical significance testing.

Key findings:

- Top 3 continents with higher BMI:
  1. Australia
  2. Americas
  3. Europe

- Australia showed the highest variability in BMI.
- Europe and Africa showed a higher number of outliers.

Statistical Testing (p-value analysis):

- Australia shows significant differences compared to all other continents.
- Africa and Asia are statistically similar.
- Europe and Americas are statistically similar.

Conclusion:  
Hypothesis 1 is accepted. BMI levels vary significantly across continents.

---

### 2) BMI vs Happiness

We compared average BMI and happiness levels by continent.

Findings:

- Top 3 continents with higher BMI:
  - Australia
  - Americas
  - Europe

- Top 3 continents with higher happiness:
  - Europe
  - Americas
  - Australia

Happiness levels were relatively high in continents with moderate to high BMI.

Conclusion:  
The hypothesis that lower obesity leads to higher happiness is rejected.

There is no clear inverse relationship between BMI and happiness at the continent level.

---

### 3) BMI vs Life Expectancy

We compared average BMI and life expectancy by continent.

Findings:

- Top 3 continents with lower BMI:
  - Asia
  - Africa
  - Europe

- Top 3 continents with higher life expectancy:
  - Asia
  - Australia
  - Americas

Asia had both the lowest BMI and the highest life expectancy.

However, continents with higher BMI (Australia, Europe) also showed high life expectancy.

Conclusion:  
No strong linear relationship exists between BMI and life expectancy at the continent level.  
Other socio-economic and healthcare factors likely influence life expectancy more strongly.

---

## Major Obstacles

- Finding a reliable API compatible with an external dataset
- Converting city-level data to continent-level for merging
- Aligning continent names between sources
- Choosing the right visualizations to clearly communicate findings

---

## Teamwork & Project Management

- Used collaborative tools such as Slack and Canva
- Divided tasks across API, cleaning, visualization, and presentation
- Held frequent discussions to solve integration and cleaning issues
- Adjusted workflow when encountering technical challenges

---

## Key Findings Summary

- BMI differs significantly across continents.
- Australia shows statistically significant differences compared to all others.
- Europe and Americas show similar BMI patterns.
- Africa and Asia show similar BMI patterns.
- The “lower obesity → higher happiness” hypothesis is rejected.
- Asia shows both the lowest BMI and highest life expectancy.
- Continent-level aggregation may hide country-level variations.

---

## Future Improvements

- Conduct country-level analysis instead of continent-level
- Study BMI differences by gender
- Integrate additional datasets related to diet and lifestyle
- Perform deeper statistical modeling

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
  https://datapt13thjanuary2026.slack.com/docs/T0A66JWNC1K/F0AGDK1KMJM


