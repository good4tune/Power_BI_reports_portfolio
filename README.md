# Power_BI_reports_portfolio

This Power BI report is created for a headquarter office of grocery store chain with 17 stores across the states. The power BI report is attached to a data mart built on AWS instance. The data mart is created by ETL pipeline that copies sales data from POS databases in 17 branch stores everyday.

Followings are description on DAX formulas used for creating Year-over-Year Sales trend report.

Year-over-Year Sales Trends – DAX Documentation
1. Project Overview
This Power BI visualization compares weekly sales performance across multiple years. It allows users to analyze sales trends, identify seasonal patterns, and compare performance between corresponding weeks of different years.
The visualization uses three DAX calculations to handle department sales reclassification, ISO week numbering, and year adjustments.
The objective is to provide a consistent year-over-year comparison while ensuring that sales are attributed to the appropriate department and reporting year.
________________________________________
2. DAX Measures and Calculations
A. Adjusted Value – Department Sales Reclassification
Purpose:
This calculation adjusts department sales values by consolidating selected department categories into the Grocery department.
Three department categories are excluded from appearing independently:
•	96-LOYALTY CPN
•	99-BAG CREDIT
•	95-PAPER BAG CHARGE
Instead, their sales values are added to the Grocery department (5-GROCERY).
How it works:
1.	Identifies the currently selected department using SELECTEDVALUE().
2.	Retrieves the original sales amount using the [Total Value] measure.
3.	Calculates the combined sales value of the three selected department categories using CALCULATE().
4.	Uses SWITCH(TRUE()) to apply the reclassification logic:
o	Returns BLANK() for the three selected department categories to prevent them from appearing separately.
o	Adds their combined sales value to Grocery.
o	Leaves all other department sales values unchanged.
Business value:
This approach prevents selected transaction categories from being reported as separate departments and ensures their values are included in the Grocery department's overall sales performance.
________________________________________
B. YearAdjusted – ISO Calendar Year Alignment
Purpose:
This calculated year adjustment ensures that sales are assigned to the correct reporting year when comparing weekly sales trends across multiple years.
The calculation uses ISO 8601 week numbering through:
WEEKNUM(CurrentDate, 21)
How it works:
1.	Retrieves the transaction date.
2.	Determines the ISO week number.
3.	Extracts the calendar year and calendar month.
4.	Applies adjustments around the year boundary:
o	If a date falls in January but belongs to ISO week 52 or later, it is assigned to the previous year.
o	If a date falls in December but belongs to ISO week 1, it is assigned to the following year.
o	Otherwise, the original calendar year is retained.
Business value:
This adjustment prevents inconsistencies when comparing weekly sales across calendar years. It ensures that weeks spanning December and January are grouped according to their ISO reporting year rather than their calendar year.
________________________________________
C. WeekNumAdjusted – ISO Week Number
Purpose:
This calculation generates the ISO week number for each transaction date.
How it works:
1.	Retrieves the transaction date from the dataset.
2.	Uses WEEKNUM(CurrentDate, 21) to calculate the ISO 8601 week number.
3.	Returns the resulting week number, ranging from 1 to 52 or 53 depending on the year.
Business value:
Using ISO week numbering provides a consistent weekly reporting structure, allowing sales performance to be compared across equivalent reporting weeks in different years.
________________________________________
3. How the DAX Calculations Work Together
The three calculations work together to produce the Year-over-Year Sales Trends visualization.
Calculation	Purpose
Adjusted Value	Reclassifies selected department sales into Grocery
YearAdjusted	Aligns reporting years using ISO week conventions
WeekNumAdjusted	Establishes consistent weekly groupings
Visualization Configuration
The Power BI line chart uses the following configuration:
•	X-axis:WeekNumAdjusted
•	Y-axis: Adjusted Value
•	Legend:YearAdjusted
Each line represents a different reporting year, allowing users to compare weekly sales performance across multiple years.
________________________________________
4. Business Insights and Analytical Value
This visualization supports several business analysis activities:
•	Year-over-Year Comparison: Evaluates sales performance across corresponding weeks of different years.
•	Seasonality Analysis: Identifies recurring weekly sales patterns and seasonal fluctuations.
•	Sales Performance Monitoring: Highlights periods of increased or decreased sales activity.
•	Department Sales Accuracy: Ensures selected department categories are consolidated into Grocery rather than reported separately.
•	Consistent Reporting: Uses ISO week numbering to maintain consistency across year-end reporting periods.
________________________________________
5. Technical Skills Demonstrated
This Power BI implementation demonstrates practical experience with:
•	Advanced DAX calculations and variables (VAR).
•	Conditional logic using SWITCH() and IF().
•	Filter context manipulation using CALCULATE().
•	Department sales reclassification.
•	ISO 8601 calendar and week-based reporting.
•	Year-over-Year performance analysis.
•	Dynamic data visualization using Power BI line charts.
________________________________________
