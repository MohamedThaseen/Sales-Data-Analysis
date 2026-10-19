# Blinkit Sales Data Analysis

An exploratory data analysis (EDA) of Blinkit's item- and outlet-level sales data, built in Python (Google Colab / Jupyter).

## Business Context

Blinkit is a quick-commerce (q-commerce) platform that delivers groceries and household essentials within minutes. It operates through a network of outlets (dark stores / supermarkets) that differ in size, type and location, each stocking a wide range of item categories, from fresh produce to household goods.

Sales performance can vary significantly with outlet characteristics (Grocery Store, Supermarket Type1/2/3; Tier 1/2/3 cities; Small/Medium/High sizes). Understanding what drives sales at the item and outlet level is critical for inventory planning, outlet expansion and revenue optimization.

## Business Objective

1. **Assess data quality**: identify and treat missing values, duplicates and inconsistent category labels (e.g. `Low Fat`, `LF`, `low fat`).
2. **Analyze sales performance** across Item Type, Item Fat Content, Outlet Size, Outlet Location Type and Outlet Type.
3. **Identify high- and low-performing segments**: which combinations of outlet characteristics and item attributes correlate with higher sales.
4. **Generate actionable insights** to help Blinkit:
   - Optimize product assortment and stocking by outlet type/location.
   - Spot underperforming item categories or outlet segments.
   - Support decisions on outlet expansion and inventory allocation.

> This analysis is exploratory. It surfaces patterns to guide further business and strategic decisions.

## Dataset

File: `blinkit_data.csv` (8,523 rows x 12 columns)

| Column | Type | Description |
|---|---|---|
| Item Fat Content | Categorical | Fat content of the item (Low Fat / Regular) |
| Item Identifier | Categorical | Unique product ID |
| Item Type | Categorical | Product category (16 types, e.g. Dairy, Snack Foods, Household) |
| Outlet Establishment Year | Numeric | Year the outlet was established (1998-2022) |
| Outlet Identifier | Categorical | Unique outlet ID |
| Outlet Location Type | Categorical | Tier 1 / Tier 2 / Tier 3 |
| Outlet Size | Categorical | Small / Medium / High |
| Outlet Type | Categorical | Grocery Store / Supermarket Type1 / Type2 / Type3 |
| Item Visibility | Numeric | Share of display area allotted to the item |
| Item Weight | Numeric | Weight of the item |
| Sales | Numeric | Sales value of the item at the outlet |
| Rating | Numeric | Customer rating (1.0-5.0) |

## Methodology

### 1. Data Inspection
- Previewed head/tail, checked shape, data types and numeric vs. categorical columns.

### 2. Data Cleaning
- **Duplicates:** none found.
- **Missing values:** `Item Weight` had 1,463 nulls. These rows were dropped (all belonged to outlet `OUT027`, Supermarket Type3), leaving 7,060 rows.
- **Negative values:** checked all numeric columns; none found.
- **Inconsistent labels:** standardised `LF` / `low fat` to `Low Fat` and `reg` to `Regular`.

### 3. Feature Preparation
- Encoded ordinal categoricals as numbers:
  - Outlet Location Type: Tier 1 = 1, Tier 2 = 2, Tier 3 = 3
  - Outlet Size: Small = 1, Medium = 2, High = 3
  - Outlet Type: Supermarket Type1 = 1, Type2 = 2, Grocery Store = 3
  - Item Fat Content: Low Fat = 1, Regular = 2
- Dropped identifier / non-analytic columns: `Item Identifier`, `Outlet Establishment Year`, `Outlet Identifier`.

### 4. Exploratory Data Analysis
Segment-level analysis of **maximum sales by Item Type**, focused on outlets in **Tier 1 and Tier 2** cities, **Small or Medium** size, operating as **Supermarket Type1**:

- **Low Fat items:** Tier 1 / Small, Tier 2 / Small, Tier 1 / Medium
- **Regular items:** Tier 1 / Small, Tier 2 / Small, Tier 2 / Medium, Tier 2 / High

Each segment is visualised as a bar chart (Seaborn) with labelled values.

## Tech Stack

- Python 3
- pandas, NumPy
- Matplotlib, Seaborn
- Folium (installed for geospatial work; not yet used)
- Google Colab

## Repository Structure

```
.
├── README.md
├── Blinkit_Analysis.ipynb     # Main analysis notebook
└── blinkit_data.csv           # Dataset (add your copy here)
```

## How to Run

1. Clone the repo:
   ```bash
   git clone <your-repo-url>
   cd <your-repo-name>
   ```
2. Install dependencies:
   ```bash
   pip install pandas numpy matplotlib seaborn folium
   ```
3. Update the CSV path in the notebook. It currently points to a Google Drive location; for local use:
   ```python
   blinkit = pd.read_csv("blinkit_data.csv")
   ```
4. Open and run the notebook:
   ```bash
   jupyter notebook Blinkit_Analysis.ipynb
   ```

## Key Observations So Far

- The dataset contains 16 item categories; **Fruits and Vegetables** (1,019 rows) and **Snack Foods** (988) are the most frequently listed.
- Most records are Low Fat (4,566) vs. Regular (2,494) after cleaning.
- Most records come from **Supermarket Type1** outlets (5,577 rows); Grocery Stores account for 555.
- Average rating is about 3.97, with the median at 4.0.
- Sales range from about 31.5 to 266.9, with a mean of about 141.2.

## Limitations & Next Steps

- Rows with missing `Item Weight` were dropped, which removes all `OUT027` (Supermarket Type3) data. Imputing weights (e.g. by Item Identifier) would retain this outlet.
- Current analysis uses *maximum* sales per category; adding mean/total sales and distribution views would give a fuller picture.
- Planned additions: correlation analysis, Outlet Type and Item Visibility effects on sales, and a predictive sales model.

## Author
A Mohamed Rifakath Ali
