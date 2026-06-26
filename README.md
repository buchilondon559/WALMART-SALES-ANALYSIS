# WALMART-SALES-ANALYSIS
# 🛒 Walmart Stores Sales Performance Dashboard
### A Data Analytics Project Report
## 📋 Project Information
| Detail | Information |
|--------|------------|
| **Student Name** | Koji Gregory Abuchi |
| **Department** | Data Analytics |
| **Institution** | Attueyi Academy |
| **Supervisor** | Mr. Michael |
| **Tool Used** | Microsoft Power BI |
| **Dataset** | Walmart Sales Dataset (2010–2012) |
## 1. Introduction
This report presents the findings and methodology of a data analytics project focused on Walmart's retail sales performance across 45 stores over a three-year period (2010–2012). The primary objective of this project was to design and develop an interactive, multi-page Power BI dashboard that transforms raw sales data into meaningful business insights.
The dashboard was built to answer critical business questions around store performance, holiday sales impact, seasonal trends, and the influence of key economic factors such as unemployment rates, fuel prices, and the Consumer Price Index (CPI) on weekly sales figures.
## 2. Dataset Description
The dataset used for this project is the **Walmart Sales Dataset**, which contains weekly sales records for 45 Walmart stores across the United States from **February 2010 to October 2012**.
### 2.1 Dataset Columns
| Column | Description |
|--------|-------------|
| `Store_ID` | Unique identifier for each store |
| `Store` | Store number (1–45) |
| `Date` | Week of the sales record |
| `Weekly_Sales` | Total sales for that week |
| `Holiday_Flag` | 1 = Holiday week, 0 = Non-holiday week |
| `Holiday_Label` | Holiday or No Holiday |
| `Holiday_Name` | Name of the holiday (Super Bowl, Labour Day, Thanksgiving, Christmas) |
| `Temperature` | Average temperature in the region (°F) |
| `Fuel_Price` | Cost of fuel in the region |
| `CPI` | Consumer Price Index |
| `Unemployment` | Unemployment rate in the region |
| `Year` | Year of the record |
| `Month` | Month number |
| `Month_Name` | Month name |
| `Quarter` | Quarter of the year (Q1–Q4) |

### 2.2 Dataset Summary
- **Total Stores:** 45
- **Time Period:** 2010 – 2012
- **Total Records:** Approximately 6,435 weekly records
- **Key Metrics:** Weekly Sales, Unemployment, CPI, Fuel Price, Temperature
## 3. Tools and Technologies Used
| Tool | Purpose |
| **Microsoft Power BI** | Dashboard design and data visualization |
| **SQL Server** | Data querying and validation |
| **Microsoft Excel** | Data storage and initial review |
| **GitHub** | Project version control and submission |
## 4. Dashboard Overview
The dashboard is organized into **four interactive pages**, each focused on a specific area of analysis. All pages are connected through a **Page Navigator** for seamless navigation.
### 📄 Page 1 — Overview
This is the executive summary page of the dashboard. It provides a high-level snapshot of Walmart's overall sales performance across all 45 stores for the entire period.
**Visuals Included:**
- **KPI Cards** — Total Revenue, Average Weekly Sales, Highest Weekly Sales, Lowest Weekly Sales
- **Line Chart** — Monthly Revenue Trend (2010–2012)
- **Bar Chart** — Revenue by Year (2010, 2011, 2012)
- **Slicers** — Year, Quarter
**Purpose:** To give any viewer an immediate understanding of the overall business performance at a glance.
### 📄 Page 2 — Store Performance
This page dives into the comparative performance of all 45 Walmart stores, identifying top performers, underperformers, and revenue gaps.
**Visuals Included:**
- **Bar Chart** — Sales by Store (all 45 stores)
- **Bar Chart** — Top 5 Stores by Revenue
- **Bar Chart** — Bottom 5 Stores by Revenue
- **Table** — Full Store Rankings with Total and Average Weekly Sales
- **Cards** — Highest Revenue Store, Lowest Revenue Store, Revenue Gap
- **Slicers** — Year, Quarter, Store
**Purpose:** To enable store-level comparison and identify which stores need attention and which are driving the most revenue.
### 📄 Page 3 — Holiday Impact Analysis
This page examines how holidays affect Walmart's weekly sales, comparing holiday weeks against regular weeks and breaking down performance by each holiday.
**Visuals Included:**
- **Clustered Bar Chart** — Holiday vs Non-Holiday Average Sales
- **Bar Chart** — Sales by Holiday Type (Super Bowl, Labour Day, Thanksgiving, Christmas)
- **Donut Chart** — Holiday vs Non-Holiday Revenue Share
- **Table** — Holiday Sales Breakdown by Holiday Name
- **Cards** — Average Holiday Sales, Average Non-Holiday Sales
- **Slicer** — Year
**Purpose:** To understand how much of Walmart's revenue is driven by holiday periods and which holidays have the biggest impact on sales.
### 📄 Page 4 — Economic Factors Analysis
This page explores the relationship between external economic conditions and Walmart's weekly sales performance across all stores.
**Visuals Included:**
- **Line Chart** — Unemployment Rate Trend (2010–2012)
- **Line Chart** — CPI Trend (2010–2012)
- **Bar Chart** — Fuel Price by Year
- **Bar Chart** — Temperature Impact on Sales
- **Table** — Economic Factors Summary (Year, Unemployment, Fuel Price, CPI, Weekly Sales)
- **Cards** — Avg Unemployment Rate, Avg Fuel Price, Avg CPI, Avg Temperature
- **Slicers** — Year, Quarter, Store
**Purpose:** To identify how external economic pressures such as rising fuel prices, inflation (CPI), and unemployment levels correlate with Walmart's sales figures.
## 5. Key Findings
### 5.1 Overall Revenue
- Walmart generated a **total revenue of over $6.7 billion** across all 45 stores from 2010 to 2012.
- The **average weekly sales** across all stores was approximately **$1.05 million**.
### 5.2 Store Performance
- A small group of **top 5 stores** contributed a disproportionately large share of total network revenue.
- There was a significant **revenue gap** between the best and worst performing stores.
- Some stores consistently performed **above the network average** while others consistently underperformed.
### 5.3 Holiday Impact
- **Holiday weeks generated higher average sales** than non-holiday weeks.
- **Christmas and Thanksgiving** were the strongest holiday periods for driving sales.
- A small number of stores showed little to no holiday lift, suggesting location-specific factors.
### 5.4 Seasonal Trends
- Sales peaked in **Q4** (October–December) driven by the holiday season.
- **Q1** was the weakest quarter overall.
- Monthly trends revealed clear seasonality patterns across all three years.
### 5.5 Economic Factors
- Stores in regions with **lower unemployment rates** tended to record higher weekly sales.
- **Rising fuel prices** showed a mild negative correlation with sales in certain stores.
- **CPI** increased gradually from 2010 to 2012, reflecting broader inflationary trends during the period.
## 6. Conclusion
This project successfully demonstrates how raw retail data can be transformed into actionable business intelligence through effective dashboard design. The four-page Power BI dashboard provides Walmart stakeholders with a comprehensive view of sales performance at both the network and store level, while also highlighting the impact of holidays and economic conditions on consumer spending behavior.
The analysis reveals that while Walmart's overall performance remained strong across the three-year period, there are clear opportunities to investigate underperforming stores and better prepare for high-impact holiday periods.
## 7. Recommendations
1. **Focus resources on underperforming stores** — investigate why bottom 5 stores lag behind and develop targeted improvement strategies.
2. **Maximise holiday readiness** — stock and staff up ahead of Christmas and Thanksgiving which drive the highest sales lifts.
3. **Monitor economic indicators** — unemployment and fuel price trends should be tracked as early warning signals for regional sales dips.
4. **Replicate top store strategies** — identify what top performing stores are doing differently and apply those learnings network-wide.
## 8. References
- Walmart Sales Dataset (2010–2012) — provided by Attueyi Academy
- Microsoft Power BI Documentation
- SQL Server Query Documentation
*Report prepared by **Koji Gregory Abuchi** | Data Analytics | Attueyi Academy | Supervised by Mr. Michael*
