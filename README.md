# Sales, Customer & Inventory Analytics – Power BI

A 5-page Power BI report built from 16,282 orders ($448.47M revenue, Jul 2012 – Oct 2023). It covers revenue performance, geographic trends, employee and customer insights, inventory risk, and an executive summary.

📄 **[View the full report as PDF](Power_BI_Sales_Report.pdf)**  ·  📊 [Download the .pbix](Project.pbix)

> Shared as a PDF and screenshots because Power BI's "Publish to web" needs Power BI Service.

## Report pages

### 1. Revenue and Sales Performance
![Revenue and Sales Performance](screenshots/page1_revenue_sales_performance.png)

### 2. Geographic and Regional Trends
![Geographic and Regional Trends](screenshots/page2_geographic_regional_trends.png)

### 3. Stakeholder Engagement Insights
![Stakeholder Engagement Insights](screenshots/page3_stakeholder_engagement.png)

### 4. Inventory and Product Intelligence
![Inventory and Product Intelligence](screenshots/page4_inventory_product_intelligence.png)

### 5. Executive Summary and Key Insights
![Executive Summary and Key Insights](screenshots/page5_executive_summary.png)

## Key insights
- **Markets are concentrated:** USA, Germany, France, Brazil and UK generate 56.8% of revenue.
- **Revenue is flat:** roughly $38–41M a year since 2013.
- **Stock risk on a top seller:** Thüringer Rostbratwurst, the #2 revenue product ($24.6M), has zero stock. In total, 5 products are out of stock and 12 hold fewer than 10 units.
- **Small markets are the weakest:** Ireland, Norway and Poland have 1 customer each and the lowest revenue ($4.5M–$6.0M).
- **Balanced team:** each employee produced $48.3M–$51.5M; Margaret Peacock leads with 1,908 orders.

## Data model
| Table | Role |
|---|---|
| Order_Details | Fact table (quantity, discount, final selling price) |
| Orders | Order date, ship country, links to customers and employees |
| Customers, Employees, Products | Dimension tables |

Relationships (many-to-one): Order_Details → Orders, Order_Details → Products, Orders → Customers, Orders → Employees.

## DAX measures
```DAX
Total Revenue = SUM(Order_Details[FinalSellingPrice])
Total Orders  = DISTINCTCOUNT(Order_Details[OrderID])
Total Units Sold = SUM(Order_Details[Quantity])
Average Discount % = AVERAGE(Order_Details[Discount]) * 100
Total Asset Value = SUM(Products[InventoryValue])
Total Units in_Stock = SUM(Products[UnitsInStock])
Total Product Catalog = DISTINCTCOUNT(Products[ProductName])

Top Performer (Orders) =
MAXX ( TOPN ( 1, Employees, [Total Orders], DESC ), Employees[FullName] )

Core Client Hub =
MAXX (
    TOPN ( 1, VALUES ( Customers[Country] ),
           CALCULATE ( DISTINCTCOUNT ( Customers[CustomerID] ) ), DESC ),
    Customers[Country]
)

Lead Job Role =
MAXX ( TOPN ( 1, Employees, [Total Orders], DESC ), Employees[Title] )

Country With Least Customers =
VAR CountryTable =
    ADDCOLUMNS ( VALUES ( Customers[Country] ),
        "@Cust", CALCULATE ( DISTINCTCOUNT ( Customers[CustomerID] ) ) )
VAR MinCust = MINX ( CountryTable, [@Cust] )
RETURN
    CONCATENATEX ( FILTER ( CountryTable, [@Cust] = MinCust ), Customers[Country], ", " )
```

## Tools
Power BI Desktop · DAX · Data modeling · Data storytelling


