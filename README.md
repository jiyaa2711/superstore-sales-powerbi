# 📊 Superstore Sales Analytics Dashboard (Power BI)

An interactive, 3-page **Power BI** dashboard that analyses four years of retail sales (2015–2018): revenue trends, growth, customers, products, regions and shipping performance. The model includes a dedicated date table and 36 DAX measures covering KPIs, year-over-year growth, YTD and rolling calculations, rankings and customer behaviour.

> **Developed by:** Jiya Dodhiya  
> **GitHub:** https://github.com/jiyaa2711  **LinkedIn:** https://www.linkedin.com/in/jiya-dodhiya-66077938a/

---

## 📁 Dataset

| Item | Details |
|---|---|
| **Source** | [Kaggle – Superstore Sales Dataset](https://www.kaggle.com/)  |
| **File used** | `train.csv` |
| **Size** | 9,800 rows × 18 columns |
| **Period** | 03-Jan-2015 to 30-Dec-2018 |
| **Granularity** | One row per product line item in an order |
| **Geography** | United States (4 regions, 49 states, 529 cities) |

**Columns:** Row ID, Order ID, Order Date, Ship Date, Ship Mode, Customer ID, Customer Name, Segment, Country, City, State, Postal Code, Region, Product ID, Category, Sub-Category, Product Name, Sales.

**Key facts**
- 4,922 unique orders, 793 customers, 1,861 products
- 3 categories (Furniture, Office Supplies, Technology) and 17 sub-categories
- 3 customer segments (Consumer, Corporate, Home Office) and 4 ship modes
- ⚠️ The dataset has **no Profit, Quantity or Discount** columns, so profit/margin analysis is not included.

The original data belongs to its Kaggle publisher. Please check the dataset's licence on Kaggle before reusing or redistributing it.

---

## 🧱 Data Model

```
Calendar (date table)  1 ───────< *  Sales (fact table)
   Date  ───────────────────────────  Order Date
```

- **Sales** – fact table loaded from the CSV (the CSV is embedded in the model, so no file path needs to be configured). Power Query converts date columns (dd/mm/yyyy), sets data types, cleans blank postal codes and adds a calculated **Ship Days** column (`Ship Date − Order Date`).
- **Calendar** – DAX-generated date table (Date, Year, Quarter, Month, Month Number, Year-Month, Year-Quarter, Weekday, Weekday Number), marked as the model's date table. Required for all time-intelligence measures.

---

## 🖥️ Dashboard Pages

Every page has dropdown slicers and KPI cards, and all visuals cross-filter each other.

### 1️⃣ Executive Overview
| Section | What it shows |
|---|---|
| **Slicers** | Year, Region, Segment, Category, State |
| **KPI cards** | Total Sales, Total Orders, Total Customers, Avg Order Value, YoY Growth %, Avg Ship Days |
| **Line chart** | Monthly sales vs. the same month last year |
| **Donut – Segment** | Sales share by customer segment |
| **Bar – Region** | Sales by region |
| **Bar – Sub-Category** | Sales by sub-category (sorted high → low) |
| **Donut – Category** | Sales share by category |

### 2️⃣ Products & Customers
| Section | What it shows |
|---|---|
| **Slicers** | Year, Region, Segment, Category, Ship Mode |
| **KPI cards** | Products Sold, Avg Sales / Customer, Orders / Customer, Repeat Customer %, Largest Order, Top Sub-Category |
| **Product Leaderboard** | Rank, product, category, sales and % of total |
| **Customer Leaderboard** | Rank, customer, segment, orders and sales |
| **Category chart** | Sales and orders by category |
| **Ship Mode donut** | Orders by shipping mode |
| **Avg Order Value** | By customer segment |

### 3️⃣ Geography & Time Intelligence
| Section | What it shows |
|---|---|
| **Slicers** | Year, Region, Segment, Category |
| **KPI cards** | Sales YTD, YTD vs Last Year %, Rolling 3M Sales, Avg Monthly Sales, Top State, Best Month |
| **Bar – State** | Sales by state (all 49, sorted) |
| **Seasonality line chart** | Monthly sales with one line per year |
| **Matrix** | Sales by Year × Quarter |
| **Column – Weekday** | Sales by day of week |

---

## 🧮 DAX Measures (36)

All measures live in the `Sales` table, grouped into display folders.

**1 – KPIs**
`Total Sales` · `Total Orders` · `Total Customers` · `Total Products` · `Line Items` · `Avg Order Value` · `Avg Sales per Customer` · `Orders per Customer` · `Items per Order` · `Avg Ship Days` · `Largest Order` · `Avg Monthly Sales`

**2 – Growth** (based on the latest year in the current filter, so cards work with or without a Year slicer)
`Sales CY` · `Sales PY` · `YoY Growth %` · `YoY Growth Amount`

**3 – Time Intelligence**
`Sales LY` · `Sales YoY % (period)` · `Sales PM` · `Sales MoM %` · `Sales YTD` · `Sales YTD LY` · `Sales YTD YoY %` · `Rolling 3M Sales`

**4 – Ranking & Contribution**
`% of Total Sales` · `Product Rank` · `Customer Rank` · `Top Category` · `Top Sub-Category` · `Top Region` · `Top State` · `Best Month`

**5 – Customers**
`Repeat Customers` · `Repeat Customer %`

Example:

```DAX
YoY Growth % =
DIVIDE ( [Sales CY] - [Sales PY], [Sales PY] )

Sales YTD =
TOTALYTD ( [Total Sales], 'Calendar'[Date] )

Repeat Customers =
COUNTROWS (
    FILTER ( VALUES ( Sales[Customer ID] ), CALCULATE ( DISTINCTCOUNT ( Sales[Order ID] ) ) > 1 )
)
```

---

## 🚀 How to Open

**Requirements:** Power BI Desktop (latest version, Windows).

1. Clone or download this repository.
2. Double-click **`SuperstoreSales.pbip`**.
3. Click **Home → Refresh** once to load the data.
4. *(Optional)* **File → Save as** to create a single `.pbix` file.

If Power BI Desktop reports a format problem, enable these under **File → Options → Preview features** and restart:
- Power BI Project (.pbip) save option
- Store semantic model using TMDL format
- Store reports using enhanced metadata format (PBIR)

---

## 🗂️ Project Structure

```
SuperstoreSales.pbip                      # open this file
SuperstoreSales.SemanticModel/            # data model (TMDL)
 └─ definition/
     ├─ model.tmdl
     ├─ relationships.tmdl
     └─ tables/  Sales.tmdl · Calendar.tmdl
SuperstoreSales.Report/                   # report pages and visuals (PBIR)
 └─ definition/pages/  overview · products · geotime
```

---

## 🛠️ Tools & Skills Used

- Power BI Desktop (Power BI Project / `.pbip` format)
- Power Query (M) – data cleaning and typing
- DAX – measures, time intelligence, ranking
- Data modelling – star schema with a date table
- Dashboard design – KPI cards, trends, drill-down by slicers

---

## 🔮 Possible Improvements

- Add Profit, Discount and Quantity data to analyse margins
- Add a map visual for state-level sales
- Add drill-through pages for products and customers
- Add forecasting and targets vs. actual

---

## 👤 Author

Jiya Dodhiya
📧 jiyadodhiya@gmail.com · 🔗 LinkedIn:https://www.linkedin.com/in/jiya-dodhiya-66077938a/  💻 GitHub:https://github.com/jiyaa2711

⭐ If you found this project useful, consider giving the repository a star.
