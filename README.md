# 🥗 NutriMap: Food Desert Detection & Grocery Inflation Forecasting

**Real-time geospatial analytics quantifying the 2.5× grocery affordability gap between food deserts and high-access areas across 13.6M Americans.**

![Dashboard Preview](powerbi/screenshots/page1_food_desert_map.png)

---

## 📊 Project Impact

- **Mapped 345 food desert counties** across 3,000+ U.S. counties using USDA Food Atlas data
- **Quantified a 2.5× weekly affordability gap** — food desert residents (avg $44K income) afford 11 grocery items/week vs. 35 for high-access residents
- **Forecasted food inflation with 0.28% MAPE** using Prophet vs. ARIMA model comparison across 6 CPI categories
- **Revealed healthy food costs 136% more per unit** than processed alternatives, creating a nutrition penalty for low-income households

---

## 🏗️ Architecture

```
┌─────────────────┐      ┌──────────────┐      ┌─────────────┐      ┌──────────────┐
│  USDA Food      │      │  Kroger API  │      │ BLS CPI API │      │ OpenFoodFacts│
│  Atlas (Excel)  │──┬──▶│  (48K prices)│──┬──▶│ (6 cats)    │──┬──▶│  (Nutriscore)│
└─────────────────┘  │   └──────────────┘  │   └─────────────┘  │   └──────────────┘
                     │                     │                    │
                     ▼                     ▼                    ▼
              ┌──────────────────────────────────────────────────────┐
              │         PostgreSQL (raw schema)                      │
              └──────────────────────────────────────────────────────┘
                                    │
                                    ▼
              ┌──────────────────────────────────────────────────────┐
              │    dbt (12 models: staging → marts)                  │
              │    • dim_location (food access tier classification)  │
              │    • fact_nutrition_price (48K product-price rows)   │
              │    • fact_cpi_forecast (Prophet vs. ARIMA)           │
              └──────────────────────────────────────────────────────┘
                                    │
                                    ▼
              ┌──────────────────────────────────────────────────────┐
              │       Power BI (3-page narrative dashboard)          │
              └──────────────────────────────────────────────────────┘
```

**Tech Stack:** Python · PostgreSQL · dbt · Prophet · statsmodels (ARIMA) · Power BI · Kroger API · USDA Data

---

## 🎯 Key Findings

### 1. Food Deserts Are Concentrated in the South
38% of all U.S. food desert counties are in just 3 states: Georgia, Texas, and Mississippi.

### 2. Low-Income Areas Are 3× More Likely to Be Food Deserts
Counties earning <$35K median income have a 3× higher rate of food desert status vs. high-income counties.

### 3. Food Inflation Is Accelerating for Staples
4 of 6 food categories are forecast to rise over the next 6 months, with fruits & vegetables leading at +1.9% (2× the Fed's inflation target).

### 4. Healthy Food Costs 136% More
Health products average $15.40/unit vs. snacks at $6.50 — making nutritious eating unaffordable for low-income households.

---

## 📸 Dashboard Preview

### Page 1: Food Desert Map
![Food Desert Map](powerbi/screenshots/page1_food_desert_map.png)

### Page 2: Cost of Nutrition
![Cost of Nutrition](powerbi/screenshots/page2_cost_nutrition.png)

### Page 3: Inflation Forecast
![Inflation Forecast](powerbi/screenshots/page3_inflation_forecast.png)

---

## 🚀 Reproducibility

**Prerequisites:** PostgreSQL 14+, Python 3.10+, Power BI Desktop

```bash
# 1. Clone repo
git clone https://github.com/adikan2k/NutriMap-Geospatial-Food-Analytics-Inflation-Forecasting.git
cd NutriMap-Geospatial-Food-Analytics-Inflation-Forecasting

# 2. Set up database
createdb nutrimap
psql -d nutrimap -f postgres/schema.sql

# 3. Install Python deps
pip install -r requirements.txt

# 4. Run ETL pipeline
python flows/ingest_food_atlas.py
python flows/ingest_kroger_products.py
python flows/ingest_openfoodfacts.py
python flows/ingest_cpi.py
python flows/forecast_cpi.py

# 5. Run dbt models
cd nutrimap && dbt run

# 6. Open Power BI
# File → Open → NutriMap.pbix
# Update data source to your localhost PostgreSQL

Full setup guide: [GETTING_STARTED.md](GETTING_STARTED.md)

---

## 📁 Project Structure

```
NutriMap-Geospatial-Food-Analytics-Inflation-Forecasting/
├── flows/              # Python ETL scripts (Kroger API, USDA ingestion, forecasting)
├── nutrimap/           # dbt project (12 models: staging + marts)
├── powerbi/            # Power BI dashboard + build guide
├── postgres/           # Database schema + ad-hoc queries
├── data/               # Raw data downloads
├── requirements.txt    # Python dependencies
├── GETTING_STARTED.md # Setup instructions
└── README.md           # This file
```

---

## � Download Dashboard

| Format | Size | Best For | Link |
|--------|------|----------|------|
| **Full Dashboard (.pbix)** | 98 MB | Complete interactive version with data | [Download (Git LFS)](https://github.com/adikan2k/NutriMap-Geospatial-Food-Analytics-Inflation-Forecasting/raw/main/NutriMap.pbix) |
| **Template (.pbit)** | 5 MB | Reproduce with your own data | [Download](powerbi/NutriMap.pbit) |
| **PDF (Static)** | 10 MB | Quick preview without software | [Download](powerbi/NutriMap_Dashboard.pdf) |

**All formats require Power BI Desktop (free) except PDF.**

---

## �🔗 Links

- **Live Dashboard:** [Link if you publish to Power BI Service]
- **Portfolio Writeup:** [Link to your portfolio page]
- **LinkedIn Post:** [Link to your project announcement]

---

## 👥 Contributors

- **Aditya Kanbargi** — Data Engineering, dbt Modeling, Power BI Dashboard
- **Sanjana Kadambe Muralidhar** — Data Analysis, Forecasting, Visualization

---

## 📝 License

MIT License - feel free to use this for learning or adapt for your own projects.
