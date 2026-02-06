# 📊 Blinkit Sales Analysis - SQL & Power BI Dashboard

![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)
![SQL](https://img.shields.io/badge/SQL-4479A1?style=for-the-badge&logo=mysql&logoColor=white)
![Excel](https://img.shields.io/badge/Excel-217346?style=for-the-badge&logo=microsoft-excel&logoColor=white)

---

## 📋 Table of Contents
- [Project Overview](#-project-overview)
- [Problem Statement](#-problem-statement)
- [Tools & Technologies](#️-tools--technologies)
- [Dataset Information](#-dataset-information)
- [SQL Analysis](#-sql-analysis)
- [Power BI Dashboard](#-power-bi-dashboard)
- [Key Insights](#-key-insights)
- [Conclusion](#-conclusion)
- [Future Improvements](#-future-improvements)
- [How to Run This Project](#-how-to-run-this-project)
- [Connect With Me](#-connect-with-me)

---

## 🎯 Project Overview

This project presents a comprehensive analysis of **Blinkit** (India's Last Minute App) sales data using **SQL** for data cleaning and analysis, combined with **Power BI** for creating an interactive dashboard. The analysis focuses on understanding sales performance, customer satisfaction, and inventory distribution across different outlets and product categories.

The project demonstrates end-to-end data analytics skills including data extraction, transformation, analysis, visualization, and deriving actionable business insights.

---

## 📝 Problem Statement

### **Business Requirement**

To conduct a comprehensive analysis of Blinkit's sales performance, customer satisfaction, and inventory distribution to identify key insights and opportunities for optimization using various KPIs and visualizations in Power BI.

### **Primary KPI Requirements**

1. **Total Sales** - The overall revenue generated from all items sold
2. **Average Sales** - The average revenue per sale
3. **Number of Items** - The total count of different items sold
4. **Average Rating** - The average customer rating for items sold

### **Granular Analysis Requirements**

1. **Total Sales by Fat Content**
   - Objective: Analyze the impact of fat content on total sales
   - Additional metrics: Average Sales, Number of Items, Average Rating by fat content

2. **Total Sales by Item Type**
   - Objective: Identify the performance of different item types
   - Additional metrics: Assess KPI variations across item types

3. **Fat Content by Outlet for Total Sales**
   - Objective: Compare total sales across different outlets segmented by fat content
   - Additional metrics: Cross-analysis of all KPIs

4. **Total Sales by Outlet Establishment**
   - Objective: Evaluate how the age or type of outlet establishment influences total sales

### **Chart Requirements**

5. **Sales by Outlet Size**
   - Analyze correlation between outlet size and total sales

6. **Sales by Outlet Location**
   - Assess geographic distribution of sales across different locations

7. **All Metrics by Outlet Type**
   - Comprehensive view of all KPIs broken down by outlet types

---

## 🛠️ Tools & Technologies

| Tool | Purpose |
|------|---------|
| **SQL** | Data cleaning, transformation, and analysis |
| **Power BI** | Interactive dashboard creation and data visualization |
| **DAX** | Creating calculated measures and columns |
| **Power Query** | Data extraction and transformation |
| **Microsoft Excel** | Initial data exploration and validation |

**Key Skills Demonstrated:**
- Data Cleaning & Preparation
- SQL Query Writing
- DAX Calculations
- Data Modeling
- Dashboard Design
- Business Intelligence
- Data Storytelling

---

## 📊 Dataset Information

### **Dataset Overview**
- **Source:** Blinkit Sales Data
- **Total Records:** 8,523 items
- **Total Revenue:** $1.20M
- **Time Period:** 2012 - 2022

### **Dataset Columns**

| Column Name | Description | Data Type |
|------------|-------------|-----------|
| Item Fat Content | Fat content classification (Low Fat/Regular) | Categorical |
| Item Identifier | Unique identifier for each item | Text |
| Item Type | Category of the item (e.g., Fruits, Snacks, Dairy) | Categorical |
| Outlet Establishment Year | Year when the outlet was established | Numeric |
| Outlet Identifier | Unique identifier for each outlet | Text |
| Outlet Location Type | Location tier of the outlet (Tier 1/2/3) | Categorical |
| Outlet Size | Size of the outlet (Small/Medium/High) | Categorical |
| Outlet Type | Type of outlet (Supermarket/Grocery Store) | Categorical |
| Item Visibility | Visibility percentage of the item in the outlet | Numeric |
| Item Weight | Weight of the item | Numeric |
| Sales | Sales revenue for the item | Numeric |
| Rating | Customer rating for the item | Numeric |

---

## 💻 SQL Analysis

### **Data Cleaning Operations**

```sql
-- 1. Handling missing values in Item Weight
UPDATE blinkit_data
SET Item_Weight = (SELECT AVG(Item_Weight) FROM blinkit_data WHERE Item_Weight IS NOT NULL)
WHERE Item_Weight IS NULL;

-- 2. Standardizing Fat Content values
UPDATE blinkit_data
SET Item_Fat_Content = 'Low Fat'
WHERE Item_Fat_Content IN ('LF', 'low fat');

UPDATE blinkit_data
SET Item_Fat_Content = 'Regular'
WHERE Item_Fat_Content IN ('reg');

-- 3. Removing duplicate records
WITH CTE AS (
    SELECT *, 
           ROW_NUMBER() OVER (PARTITION BY Item_Identifier, Outlet_Identifier ORDER BY Sales DESC) as rn
    FROM blinkit_data
)
DELETE FROM CTE WHERE rn > 1;
```

### **Key SQL Queries for Analysis**

```sql
-- 1. Total Sales by Fat Content
SELECT 
    Item_Fat_Content,
    ROUND(SUM(Sales), 2) as Total_Sales,
    ROUND(AVG(Sales), 2) as Avg_Sales,
    COUNT(Item_Identifier) as No_of_Items,
    ROUND(AVG(Rating), 2) as Avg_Rating
FROM blinkit_data
GROUP BY Item_Fat_Content;

-- 2. Top 5 Item Types by Sales
SELECT TOP 5
    Item_Type,
    ROUND(SUM(Sales), 2) as Total_Sales,
    COUNT(*) as Item_Count
FROM blinkit_data
GROUP BY Item_Type
ORDER BY Total_Sales DESC;

-- 3. Sales by Outlet Location Type
SELECT 
    Outlet_Location_Type,
    ROUND(SUM(Sales), 2) as Total_Sales,
    COUNT(DISTINCT Outlet_Identifier) as No_of_Outlets
FROM blinkit_data
GROUP BY Outlet_Location_Type
ORDER BY Total_Sales DESC;

-- 4. Outlet Performance by Establishment Year
SELECT 
    Outlet_Establishment_Year,
    ROUND(SUM(Sales), 2) as Total_Sales,
    COUNT(DISTINCT Outlet_Identifier) as Outlets
FROM blinkit_data
GROUP BY Outlet_Establishment_Year
ORDER BY Outlet_Establishment_Year;

-- 5. Sales Distribution by Outlet Size
SELECT 
    Outlet_Size,
    ROUND(SUM(Sales), 2) as Total_Sales,
    ROUND(100.0 * SUM(Sales) / (SELECT SUM(Sales) FROM blinkit_data), 2) as Sales_Percentage
FROM blinkit_data
WHERE Outlet_Size IS NOT NULL
GROUP BY Outlet_Size;
```

---

## 📈 Power BI Dashboard

### **Dashboard Preview**

![Dashboard Overview](path/to/dashboard_screenshot.png)
*Main dashboard showing all KPIs and visualizations*

### **DAX Measures Created**

```dax
// Total Sales
Total Sales = SUM(blinkit_data[Sales])

// Average Sales
Avg Sales = AVERAGE(blinkit_data[Sales])

// Number of Items
No of Items = COUNT(blinkit_data[Item_Identifier])

// Average Rating
Avg Rating = AVERAGE(blinkit_data[Rating])

// Sales by Previous Year (for comparison)
Sales PY = CALCULATE(
    [Total Sales],
    DATEADD(blinkit_data[Outlet_Establishment_Year], -1, YEAR)
)

// Year over Year Growth
YoY Growth = 
DIVIDE(
    [Total Sales] - [Sales PY],
    [Sales PY],
    0
)
```

### **Interactive Features**

- **Filter Panel** (Left Side)
  - Outlet Location (All/Tier 1/Tier 2/Tier 3)
  - Outlet Size (All/Small/Medium/High)
  - Item Type (All/Fruits/Snacks/Dairy/etc.)

- **KPI Cards** (Top Section)
  - Total Sales: $1.20M
  - Average Sales: $141
  - Number of Items: 8,523
  - Average Rating: 3.9 ⭐

### **Visualizations Breakdown**

#### 1. **Fat Content Analysis**
![Fat Content Chart](path/to/fat_content_chart.png)

- **Donut Chart** showing sales split between Low Fat ($776K) and Regular ($425K)
- Low Fat products contribute 64.6% of total sales
- Regular products contribute 35.4% of total sales

#### 2. **Item Type Performance**
![Item Type Analysis](path/to/item_type_chart.png)

- **Horizontal Bar Chart** displaying sales by product category
- Top performers:
  - Fruits and Vegetables: $0.18M
  - Snack Foods: $0.18M
  - Household Items: $0.14M

#### 3. **Outlet Establishment Trend**
![Outlet Establishment Trend](path/to/outlet_trend_chart.png)

- **Line Chart** showing sales growth from 2012 to 2022
- Peak sales: $205K (2018)
- Recent stabilization around $131K (2022)

#### 4. **Fat Content by Outlet Type**
![Fat by Outlet](path/to/fat_by_outlet.png)

- **Stacked Bar Chart** comparing Low Fat vs Regular sales across outlet tiers
- All tiers show preference for Low Fat products

#### 5. **Outlet Size Distribution**
![Outlet Size](path/to/outlet_size_chart.png)

- **Donut Chart** showing sales distribution:
  - Medium: $249K (42.3%)
  - Small: $445K (37.0%)
  - High: $508K (20.7%)

#### 6. **Outlet Location Performance**
![Outlet Location](path/to/outlet_location_chart.png)

- **Horizontal Bar Chart** by location tier:
  - Tier 3: $472.13K
  - Tier 2: $393.15K
  - Tier 1: $336.40K (Highest performance percentage at 71.3%)

#### 7. **Outlet Type Comparison Table**
![Outlet Type Table](path/to/outlet_type_table.png)

Comprehensive table showing all metrics by outlet type:

| Outlet Type | Total Sales | No. of Items | Avg Sales | Avg Rating | Item Visibility |
|-------------|-------------|--------------|-----------|------------|----------------|
| Supermarket Type1 | $787.55K | 5,577 | $141 | 4.0 | 0.06 |
| Grocery Store | $151.94K | 1,083 | $140 | 4.0 | 0.10 |
| Supermarket Type2 | $131.48K | 928 | $142 | 4.0 | 0.06 |
| Supermarket Type3 | $130.71K | 935 | $140 | 4.0 | 0.06 |

---

## 💡 Key Insights

### **Sales Performance**

1. **Total Revenue Achievement**
   - Generated $1.20M in total sales across all outlets
   - Average transaction value of $141 indicates healthy ticket size
   - Consistent 3.9 average rating shows good customer satisfaction

2. **Product Performance**
   - **Low Fat products dominate** with 64.6% market share ($776K)
   - Top 3 categories drive 45% of sales:
     - Fruits and Vegetables: $0.18M
     - Snack Foods: $0.18M
     - Household Items: $0.14M

3. **Outlet Performance**
   - **Tier 3 locations generate highest revenue** ($472.13K), suggesting strong performance in smaller cities
   - Tier 1 shows highest efficiency with 71.3% item visibility
   - Medium-sized outlets contribute 42% of total sales

### **Strategic Insights**

4. **Establishment Timeline Analysis**
   - Peak performance in 2018 ($205K)
   - Recent stabilization indicates market maturity
   - 2012-2014 shows steady growth phase

5. **Outlet Type Effectiveness**
   - **Supermarket Type1** is the clear winner with $787.55K (65% of total sales)
   - Grocery stores show lowest sales ($151.94K) but highest visibility (0.10)
   - All outlet types maintain consistent 4.0 rating

### **Optimization Opportunities**

6. **Product Strategy**
   - Regular fat content products underperform - potential to:
     - Reduce inventory of regular products
     - Expand low-fat product range
     - Market regular products more effectively

7. **Geographic Expansion**
   - Tier 3 cities show strong performance - opportunity for expansion
   - Tier 1 locations need revenue optimization despite good visibility

8. **Outlet Size Optimization**
   - Small outlets ($445K) outperform medium ($249K) and high ($508K)
   - Consider optimizing medium-sized outlets or expanding small format stores

---

## 🎓 Conclusion

This analysis of Blinkit's sales data reveals several critical insights for business optimization:

**Key Takeaways:**
- Low-fat products are the primary revenue driver, indicating health-conscious consumer behavior
- Tier 3 cities represent the strongest market segment with $472K in sales
- Supermarket Type1 outlets demonstrate the most successful business model
- Customer satisfaction remains consistently high (3.9 rating) across all segments

**Business Impact:**
- Data-driven insights can guide inventory management decisions
- Location strategy should prioritize Tier 3 expansion
- Product mix optimization toward low-fat items can increase revenue
- Outlet format replication of Type1 supermarkets can drive growth

**Technical Achievements:**
- Successfully cleaned and analyzed 8,523 records using SQL
- Created dynamic DAX measures for real-time KPI tracking
- Designed an interactive dashboard enabling stakeholder self-service analytics
- Demonstrated end-to-end data analytics workflow

---

## 🚀 Future Improvements

### **Technical Enhancements**
- [ ] Implement time-series forecasting for sales prediction
- [ ] Add customer segmentation analysis using clustering
- [ ] Create automated ETL pipeline using Python/Apache Airflow
- [ ] Integrate real-time data refresh capabilities
- [ ] Build mobile-responsive dashboard version

### **Analysis Extensions**
- [ ] Perform basket analysis to identify product associations
- [ ] Add seasonality analysis for demand forecasting
- [ ] Include profitability analysis with cost data
- [ ] Implement ABC analysis for inventory categorization
- [ ] Add customer lifetime value (CLV) metrics

### **Dashboard Features**
- [ ] Add drill-through pages for detailed outlet analysis
- [ ] Implement what-if parameters for scenario planning
- [ ] Create executive summary page with key highlights
- [ ] Add export functionality for automated reports
- [ ] Include comparative analysis with industry benchmarks

### **Data Quality**
- [ ] Implement data validation rules
- [ ] Add data quality scorecards
- [ ] Create automated anomaly detection
- [ ] Include data lineage documentation

---

## 📂 How to Run This Project

### **Prerequisites**
- Microsoft SQL Server / MySQL
- Power BI Desktop (latest version)
- Basic understanding of SQL and Power BI

### **Step-by-Step Instructions**

#### **1. Database Setup**
```sql
-- Create database
CREATE DATABASE Blinkit_Analysis;

-- Use database
USE Blinkit_Analysis;

-- Import CSV data into table
-- (Use SQL Server Import Wizard or LOAD DATA command)
```

#### **2. Data Cleaning**
```bash
# Run the SQL cleaning script
# Execute all queries from SQL_Analysis section above
# Verify data quality after cleaning
```

#### **3. Power BI Setup**

1. **Open Power BI Desktop**

2. **Connect to Data Source**
   - Click 'Get Data' → 'SQL Server'
   - Enter server name and database
   - Select 'blinkit_data' table

3. **Create Measures**
   - Go to 'Modeling' tab
   - Create New Measure
   - Copy DAX formulas from the DAX Measures section above

4. **Build Visualizations**
   - Use the filter panel on the left
   - Create KPI cards at the top
   - Add charts as per requirement:
     - Donut charts for Fat Content and Outlet Size
     - Bar charts for Item Type and Location
     - Line chart for Outlet Establishment
     - Table for Outlet Type comparison

5. **Apply Formatting**
   - Use Blinkit brand colors (Yellow: #FFD700, Black: #177c29)
   - Set consistent font sizes and styles
   - Add title and logos

6. **Publish Dashboard**
   - Save the .pbix file
   - Publish to Power BI Service (optional)

#### **4. Testing**
- Verify all filters work correctly
- Test cross-filtering between visuals
- Validate KPI calculations
- Check data refresh functionality

### **Files in This Repository**

```
├── README.md                          # Project documentation (this file)
├── SQL_Scripts/
│   └──Cleaning_And_Analysis_queries.sql          # SQL analysis and cleaning queries
├── PowerBI_DashBoard/
│   └── Blinkit_DashBoard.pbix        # Power BI dashboard file
├── Dataset/
│   └── blinkit_data.csv              # Raw dataset (if shareable)
├── Images/
│   ├── DashBoard_Overview.png        # Main dashboard screenshot
│   ├── KPI_Cards.png                 # KPI section
│   ├── Charts.png      # Charts Section
|   ├── After_Filter.png      # After Filter
│   └── Filter_Panel.png        # Filter Panel
└── Documentation/
    ├── Business_Requirements.pdf     # Original problem statement
    └── Blinkit_Analysis_Report.pdf         # Final Report
```

---

## 👤 Connect With Me

I'm passionate about data analytics and turning data into actionable insights. Let's connect!

[![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/aryan-kumar-sahoo)
[![GitHub](https://img.shields.io/badge/GitHub-100000?style=for-the-badge&logo=github&logoColor=white)](https://github.com/Aryan-1105)
[![Email](https://img.shields.io/badge/Email-D14836?style=for-the-badge&logo=gmail&logoColor=white)](mailto:aryankumarsahoo10@gmail.com)

---

## 📜 License

This project is licensed under the MIT License - feel free to use this project for learning and portfolio purposes.

---

## 🙏 Acknowledgments

- **Blinkit** for the business context and problem statement
- **Power BI Community** for visualization inspiration
- **SQL Community** for query optimization techniques

---

<div align="center">

### ⭐ If you found this project helpful, please give it a star!

**Made with ❤️ and ☕ by [Aryan Kumar Sahoo]**

*Last Updated: February 2026*

</div>
