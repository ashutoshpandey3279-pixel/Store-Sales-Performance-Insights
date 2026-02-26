# Store-Sales-Performance-Insights
This Power BI dashboard analyzes retail store data to track revenue growth, product line performance, and the impact of various promotional campaigns.

Store Data & Sales Analysis Dashboard

 <img width="1356" height="822" alt="Image" src="https://github.com/user-attachments/assets/e97c704b-bc46-4161-a0a2-c96e5c62dea9" />

 Problem Statement

This dashboard helps a retail store understand customer purchasing patterns and product performance. It identifies which product lines are driving revenue and the effectiveness of specific marketing promotions. By analyzing "Units Sold" and "Price Per Unit," the store can pinpoint high-value categories and optimize inventory.

The analysis also tracks regional customer distribution and provides insights into which states contribute most to the bottom line, allowing for targeted regional marketing strategies.

 Steps Followed

 [1] Data Loading & Transformation


Step 1: Load data into Power BI Desktop from the Excel file containing `Sheet3` (Sales), `Dim Product`, `Dim Customers`, and `Dim Promotion`.

Step 2: Open **Power Query Editor** and use "Column Quality" and "Column Distribution" to check for nulls in `Price Per Unit` or `Customer ID`.

Step 3: Perform data cleaning by ensuring the `Date` column is in the correct `dd/mm/yyyy` format.

[2] Data Modeling (Star Schema)

 Relationships were established between the Fact table (`Sheet3`) and Dimension tables:
* `Product ID` linked to `Dim Product`.
* `CustomerID` linked to `Dim Customers`.
* `PromotionID` linked to `Dim Promotion`.

[3] Advanced DAX Implementation

To drive the dashboard's logic, the following DAX expressions were utilized:
A. Calculated Column (Logical IF)
Used to group customers into segments based on purchase volume or units:
```dax
Purchase Segment = 
IF('Sheet3'[Units Sold] <= 1, "Low Volume",
IF('Sheet3'[Units Sold] <= 3, "Medium Volume", "High Volume"))

```
B. Core Measures (Basic Aggregates)
Created to track the fundamental count of transactions and volume:
```dax
Total Transactions = COUNT('Sheet3'[CustomerID])

Total Units Sold = SUM('Sheet3'[Units Sold])
```
C. Iterator Functions (SUMX & RELATED)
Used to calculate revenue by pulling the price from the Product dimension for every row in the Sales table:
```dax
Total Revenue = 
SUMX(
    'Sheet3', 
    'Sheet3'[Units Sold] * RELATED('Dim Product'[Price (INR)])
)
```

D. Filter Context (CALCULATE & ALL)
Used to find the percentage of sales contribution for each product line:
```dax
% of Total Sales = 
DIVIDE(
    [Total Revenue], 
    CALCULATE([Total Revenue], ALL('Dim Product'))
)
```

E. Trend Analysis:
Used to compare current sales against the previous period:
```dax
Sales Last Year = CALCULATE([Total Revenue], SAMEPERIODLASTYEAR('Date'[Date]))

```

---

 Visualizations & Insights

[1] Key Performance Indicators (KPIs):
Total Revenue:Represented by a Card visual showing the sum of all sales.

Total Customers: A card showing the unique count of `CustomerID`.


Average Units per Sale:Calculated to understand basket size.

[2] Customer & Product Insights:

Top Product Line: Analysis shows which category (e.g., Electronics vs. Clothing) dominates the revenue share.

Regional Sales: A Map or Bar Chart segregating customers by **State** (e.g., Maharashtra, Uttar Pradesh).

Promotion Impact: A comparison visual showing sales performance when a `PromotionID` (like "PR001 - Summer Sale") was active versus standard days.

 [3] Performance Trends :

Monthly Growth: A line chart showing revenue fluctuations over the dates provided in the dataset.

Conclusion:

To conclude, this project transforms raw retail data into a high-performance analytical tool that empowers stakeholders to optimize product lines and promotional strategies through data-driven insights.




