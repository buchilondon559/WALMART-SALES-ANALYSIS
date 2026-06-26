Use Projects 
go 

--1. What is the total revenue generated across all 45 stores for the entire period?
SELECT FORMAT(SUM(Weekly_Sales), 'N2') AS Total_Revenue FROM Walmart_Sales

--2. What is the average weekly sales figure across all stores and all weeks?
Select Format(AVG(weekly_sales), 'N2') As Avg_Weekly_Sales from Walmart_Sales

--3. What is the highest single-week sales ever recorded — which store, which date?
SELECT TOP 1 Store_ID, Store, [Date], Holiday_Name, FORMAT(Weekly_Sales, 'N2') AS Highest_Weekly_Sales FROM Walmart_Sales
ORDER BY Weekly_Sales DESC

--4. What is the lowest single-week sales ever recorded — which store, which date?
SELECT TOP 1 Store_ID, Store, [Date], Holiday_Name, FORMAT(Weekly_Sales, 'N2') AS Lowest_Weekly_Sales FROM Walmart_Sales
ORDER BY Weekly_Sales asc 

--5. What is the total number of weeks tracked per store?
select store_id, store, count(distinct(weekly_sales)) As Number_Of_weeks from Walmart_Sales
group by Store_ID, store 
order by Store_ID asc

--6. Which store generates the highest total revenue over the full period?
SELECT TOP 5 store_ID, Store, FORMAT(SUM(Weekly_Sales), 'N2') AS Total_Revenue FROM Walmart_Sales
GROUP BY Store_ID, Store
ORDER BY SUM(Weekly_Sales) DESC

--7. Which store generates the lowest total revenue over the full period?
SELECT TOP 5 store_ID, Store, FORMAT(SUM(Weekly_Sales), 'N2') AS Total_Revenue FROM Walmart_Sales
GROUP BY Store_ID, Store
ORDER BY SUM(Weekly_Sales) asc

--8. What is the full ranking of all 45 stores by total sales?
SELECT RANK() OVER (ORDER BY SUM(Weekly_Sales) DESC) AS Rank, Store_ID, Store, FORMAT(SUM(Weekly_Sales), 'N2') AS Total_Revenue
FROM Walmart_Sales
GROUP BY Store_ID, Store
ORDER BY Rank

--9. Which store has the highest average weekly sales?
SELECT TOP 1 Store_ID, Store, FORMAT(AVG(Weekly_Sales), 'N2') AS Avg_Weekly_Sales FROM Walmart_Sales
GROUP BY Store_ID, Store
ORDER BY AVG(Weekly_Sales) DESC

--10. Which store has the lowest average weekly sales?
SELECT TOP 1 Store_ID, Store, FORMAT(AVG(Weekly_Sales), 'N2') AS Avg_Weekly_Sales FROM Walmart_Sales
GROUP BY Store_ID, Store
ORDER BY AVG(Weekly_Sales) asc

--11. Which stores consistently perform above the overall weekly sales average?
WITH Network_Avg AS (
    SELECT AVG(Weekly_Sales) AS Overall_Avg
    FROM Walmart_Sales
),
Store_Avgs AS (
    SELECT
        Store_ID,
        Store,
        AVG(Weekly_Sales) AS Avg_Weekly_Sales
    FROM Walmart_Sales
    GROUP BY Store_ID, Store
)
SELECT
    s.Store_ID,
    s.Store,
    FORMAT(s.Avg_Weekly_Sales, 'N2') AS Avg_Weekly_Sales,
    FORMAT(n.Overall_Avg, 'N2') AS Network_Avg
FROM Store_Avgs s
CROSS JOIN Network_Avg n
WHERE s.Avg_Weekly_Sales > n.Overall_Avg
ORDER BY s.Avg_Weekly_Sales DESC

--12. Which stores consistently perform below the overall weekly sales average?
WITH Network_Avg AS (
    SELECT AVG(Weekly_Sales) AS Overall_Avg
    FROM Walmart_Sales
),
Store_Avgs AS (
    SELECT
        Store_ID,
        Store,
        AVG(Weekly_Sales) AS Avg_Weekly_Sales
    FROM Walmart_Sales
    GROUP BY Store_ID, Store
)
SELECT
    s.Store_ID,
    s.Store,
    FORMAT(s.Avg_Weekly_Sales, 'N2') AS Avg_Weekly_Sales,
    FORMAT(n.Overall_Avg, 'N2') AS Network_Avg
FROM Store_Avgs s
CROSS JOIN Network_Avg n
WHERE s.Avg_Weekly_Sales < n.Overall_Avg
ORDER BY s.Avg_Weekly_Sales ASC

--13. What is the revenue gap between the best and worst performing store?
WITH Store_Totals AS (
    SELECT
        Store_ID,
        SUM(Weekly_Sales) AS Total_Sales
    FROM Walmart_Sales
    GROUP BY Store_ID
)
SELECT
    FORMAT(MAX(Total_Sales), 'N2') AS Highest_Store_Revenue,
    FORMAT(MIN(Total_Sales), 'N2') AS Lowest_Store_Revenue,
    FORMAT(MAX(Total_Sales) - MIN(Total_Sales), 'N2') AS Revenue_Gap
FROM Store_Totals

--14. What percentage of total revenue comes from the top 5 stores?
WITH Store_Totals AS (
    SELECT
        Store_ID,
        Store,
        SUM(Weekly_Sales) AS Total_Sales
    FROM Walmart_Sales
    GROUP BY Store_ID, Store
),
Network_Total AS (
    SELECT SUM(Weekly_Sales) AS Grand_Total
    FROM Walmart_Sales
)
SELECT TOP 5
    s.Store_ID,
    s.Store,
    FORMAT(s.Total_Sales, 'N2') AS Store_Revenue,
    FORMAT((s.Total_Sales / n.Grand_Total) * 100, 'N2') AS Pct_Of_Total
FROM Store_Totals s
CROSS JOIN Network_Total n
ORDER BY s.Total_Sales DESC

--15. What percentage of total revenue comes from the bottom 5 stores?
WITH Store_Totals AS (
    SELECT
        Store_ID,
        Store,
        SUM(Weekly_Sales) AS Total_Sales
    FROM Walmart_Sales
    GROUP BY Store_ID, Store
),
Network_Total AS (
    SELECT SUM(Weekly_Sales) AS Grand_Total
    FROM Walmart_Sales
)
SELECT TOP 5
    s.Store_ID,
    s.Store,
    FORMAT(s.Total_Sales, 'N2') AS Store_Revenue,
    FORMAT((s.Total_Sales / n.Grand_Total) * 100, 'N2') AS Pct_Of_Total
FROM Store_Totals s
CROSS JOIN Network_Total n
ORDER BY s.Total_Sales ASC

--16. Which store has the most inconsistent/volatile sales week over week - Most Volatile Store (Highest Sales Fluctuation)?
SELECT TOP 1 Store_ID, Store, FORMAT(STDEV(Weekly_Sales), 'N2') AS Sales_Std_Dev, FORMAT(AVG(Weekly_Sales), 'N2') AS Avg_Weekly_Sales
FROM Walmart_Sales
GROUP BY Store_ID, Store
ORDER BY STDEV(Weekly_Sales) DESC

--17. Which store has the most stable, consistent sales - Most Stable Store (Lowest Sales Fluctuation)?
SELECT TOP 1 Store_ID, Store, FORMAT(STDEV(Weekly_Sales), 'N2') AS Sales_Std_Dev, FORMAT(AVG(Weekly_Sales), 'N2') AS Avg_Weekly_Sales
FROM Walmart_Sales
GROUP BY Store_ID, Store
ORDER BY STDEV(Weekly_Sales) ASC

--18. How does total revenue trend across the full period — is the business growing, flat, or declining?
SELECT Year, Month, Month_Name, FORMAT(SUM(Weekly_Sales), 'N2') AS Monthly_Revenue FROM Walmart_Sales
GROUP BY Year, Month, Month_Name
ORDER BY Year, Month

--19. Which year generated the highest total revenue — 2010, 2011, or 2012?
select  Year, format(sum(weekly_sales), 'N2') as Total_Revenue, COUNT(DISTINCT Date) AS Weeks_In_Year from Walmart_Sales
group by Year
order by Total_Revenue desc

--20. How do average weekly sales compare year over year?
WITH YoY AS (
    SELECT
        Year,
        FORMAT(AVG(Weekly_Sales), 'N2') AS Avg_Weekly_Sales,
        COUNT(DISTINCT Date) AS Weeks_In_Year
    FROM Walmart_Sales
    GROUP BY Year
)
SELECT
    Year,
    Avg_Weekly_Sales,
    Weeks_In_Year
FROM YoY
ORDER BY Year

--21. Which month generates the highest average weekly sales across all stores?
SELECT TOP 1 Month, Month_Name, FORMAT(AVG(Weekly_Sales), 'N2') AS Avg_Weekly_Sales FROM Walmart_Sales
GROUP BY Month, Month_Name
ORDER BY AVG(Weekly_Sales) DESC

--22. Which month generates the lowest average weekly sales?
SELECT TOP 1 Month, Month_Name, FORMAT(AVG(Weekly_Sales), 'N2') AS Avg_Weekly_Sales FROM Walmart_Sales
GROUP BY Month, Month_Name
ORDER BY AVG(Weekly_Sales) ASC

--23. Which quarter is the strongest revenue quarter overall?
select Quarter, format(sum(weekly_sales), 'N2') as Quarter_Revenue, FORMAT(AVG(Weekly_Sales), 'N2') as Avg_weekly_Sales from Walmart_Sales
group by quarter
order by Avg_weekly_Sales desc

--24. Which quarter is the weakest?
select Quarter, format(sum(weekly_sales), 'N2') as Quarter_Revenue, FORMAT(AVG(Weekly_Sales), 'N2') as Avg_weekly_Sales from Walmart_Sales
group by quarter
order by Avg_weekly_Sales asc

--25. Which specific week (date) had the single highest total sales across all stores combined?
SELECT TOP 5 Date, Holiday_Name, FORMAT(SUM(Weekly_Sales), 'N2') AS Total_Network_Sales FROM Walmart_Sales
GROUP BY Date, Holiday_Name
ORDER BY SUM(Weekly_Sales) DESC

--26. Do holiday weeks generate higher average sales than non-holiday weeks? 
select Holiday_Label, format(avg(weekly_sales), 'N2') as Avg_Holiday_sales,  COUNT(*) AS Week_Count from Walmart_Sales 
group by Holiday_Label
order by Avg_Holiday_sales desc

--27. By exactly what percentage do holiday weeks outperform non-holiday weeks?
WITH Averages AS (
    SELECT
        MAX(CASE WHEN Holiday_Label = 'Holiday' THEN AvgSales END) AS Holiday_Avg,
        MAX(CASE WHEN Holiday_Label = 'No Holiday' THEN AvgSales END) AS Non_Holiday_Avg
    FROM (
        SELECT
            Holiday_Label,
            AVG(Weekly_Sales) AS AvgSales
        FROM Walmart_Sales
        GROUP BY Holiday_Label
    ) AS Sub
)
SELECT
    FORMAT(Holiday_Avg, 'N2') AS Holiday_Avg,
    FORMAT(Non_Holiday_Avg, 'N2') AS Non_Holiday_Avg,
    FORMAT(((Holiday_Avg - Non_Holiday_Avg) / Non_Holiday_Avg) * 100, 'N2') AS Pct_Outperformance
FROM Averages

--28. What is the total revenue from holiday weeks versus non-holiday weeks?
SELECT Holiday_Label, FORMAT(SUM(Weekly_Sales), 'N2') AS Total_Revenue, FORMAT((SUM(Weekly_Sales) / SUM(SUM(Weekly_Sales)) OVER()) * 100, 'N2') AS Pct_Of_Total
FROM Walmart_Sales
GROUP BY Holiday_Label

--29. How many holiday weeks appear in the dataset versus non-holiday weeks?
SELECT Holiday_Label, Holiday_Name, COUNT(*) AS Week_Count FROM Walmart_Sales
GROUP BY Holiday_Label, Holiday_Name
ORDER BY Holiday_Label DESC, Week_Count DESC

--30. Which of the four holidays — Super Bowl, Labour Day, Thanksgiving, Christmas — drives the highest average sales?
SELECT Holiday_Name, FORMAT(AVG(Weekly_Sales), 'N2') AS Avg_Weekly_Sales, FORMAT(SUM(Weekly_Sales), 'N2') AS Total_Sales, COUNT(*) AS Weeks
FROM Walmart_Sales
WHERE Holiday_Label = 'Holiday'
GROUP BY Holiday_Name
ORDER BY AVG(Weekly_Sales) DESC

--31. Which holiday drives the lowest average sales?
SELECT TOP 1 Holiday_Name, FORMAT(AVG(Weekly_Sales), 'N2') AS Avg_Weekly_Sales FROM Walmart_Sales
WHERE Holiday_Label = 'Holiday'
GROUP BY Holiday_Name
ORDER BY AVG(Weekly_Sales) ASC

--32. Which store benefits the most from holiday weeks relative to its own non-holiday average?
WITH Store_Avgs AS (
    SELECT
        Store_ID,
        Store,
        AVG(CASE WHEN Holiday_Label = 'Holiday' THEN Weekly_Sales END) AS Holiday_Avg,
        AVG(CASE WHEN Holiday_Label = 'No Holiday' THEN Weekly_Sales END) AS Non_Holiday_Avg
    FROM Walmart_Sales
    GROUP BY Store_ID, Store
)
SELECT TOP 1
    Store_ID,
    Store,
    FORMAT(Holiday_Avg, 'N2') AS Holiday_Avg,
    FORMAT(Non_Holiday_Avg, 'N2') AS Non_Holiday_Avg,
    FORMAT(((Holiday_Avg - Non_Holiday_Avg) / Non_Holiday_Avg) * 100, 'N2') AS Holiday_Lift_Pct
FROM Store_Avgs
ORDER BY ((Holiday_Avg - Non_Holiday_Avg) / Non_Holiday_Avg) DESC

--33. Which store shows the least lift during holiday weeks?
WITH Store_Avgs AS (
    SELECT
        Store_ID,
        Store,
        AVG(CASE WHEN Holiday_Label = 'Holiday' THEN Weekly_Sales END) AS Holiday_Avg,
        AVG(CASE WHEN Holiday_Label = 'No Holiday' THEN Weekly_Sales END) AS Non_Holiday_Avg
    FROM Walmart_Sales
    GROUP BY Store_ID, Store
)
SELECT TOP 1
    Store_ID,
    Store,
    FORMAT(Holiday_Avg, 'N2') AS Holiday_Avg,
    FORMAT(Non_Holiday_Avg, 'N2') AS Non_Holiday_Avg,
    FORMAT(((Holiday_Avg - Non_Holiday_Avg) / Non_Holiday_Avg) * 100, 'N2') AS Holiday_Lift_Pct
FROM Store_Avgs
ORDER BY ((Holiday_Avg - Non_Holiday_Avg) / Non_Holiday_Avg) ASC

--34. Are there stores that actually underperform during holiday weeks compared to their own baseline?
WITH Store_Avgs AS (
    SELECT
        Store_ID,
        Store,
        AVG(CASE WHEN Holiday_Label = 'Holiday' THEN Weekly_Sales END) AS Holiday_Avg,
        AVG(CASE WHEN Holiday_Label = 'No Holiday' THEN Weekly_Sales END) AS Non_Holiday_Avg
    FROM Walmart_Sales
    GROUP BY Store_ID, Store
)
SELECT
    Store_ID,
    Store,
    FORMAT(Holiday_Avg, 'N2') AS Holiday_Avg,
    FORMAT(Non_Holiday_Avg, 'N2') AS Non_Holiday_Avg,
    FORMAT(((Holiday_Avg - Non_Holiday_Avg) / Non_Holiday_Avg) * 100, 'N2') AS Holiday_Lift_Pct
FROM Store_Avgs
WHERE Holiday_Avg < Non_Holiday_Avg
ORDER BY ((Holiday_Avg - Non_Holiday_Avg) / Non_Holiday_Avg) ASC

--35. What temperature range corresponds to the highest average weekly sales?
SELECT
    CASE
        WHEN Temperature < 20  THEN 'Below 20F (Extreme Cold)'
        WHEN Temperature < 40  THEN '20-40F (Cold)'
        WHEN Temperature < 60  THEN '40-60F (Cool)'
        WHEN Temperature < 80  THEN '60-80F (Warm)'
        WHEN Temperature < 100 THEN '80-100F (Hot)'
        ELSE                        'Above 100F (Extreme Heat)'
    END AS Temp_Range,
    FORMAT(AVG(Weekly_Sales), 'N2') AS Avg_Weekly_Sales,
    COUNT(*) AS Week_Count
FROM Walmart_Sales
GROUP BY
    CASE
        WHEN Temperature < 20  THEN 'Below 20F (Extreme Cold)'
        WHEN Temperature < 40  THEN '20-40F (Cold)'
        WHEN Temperature < 60  THEN '40-60F (Cool)'
        WHEN Temperature < 80  THEN '60-80F (Warm)'
        WHEN Temperature < 100 THEN '80-100F (Hot)'
        ELSE                        'Above 100F (Extreme Heat)'
    END
ORDER BY AVG(Weekly_Sales) DESC

--36. Which store experiences the widest temperature variation across the dataset?
SELECT
    Store_ID,
    Store,
    FORMAT(MIN(Temperature), 'N2') AS Min_Temp,
    FORMAT(MAX(Temperature), 'N2') AS Max_Temp,
    FORMAT(MAX(Temperature) - MIN(Temperature), 'N2') AS Temp_Range,
    FORMAT(STDEV(Temperature), 'N2') AS Temp_Std_Dev
FROM Walmart_Sales
GROUP BY Store_ID, Store
ORDER BY MAX(Temperature) - MIN(Temperature) DESC

--37. At what fuel price Level did average weekly sales peak?
SELECT
    CASE
        WHEN Fuel_Price < 2.50 THEN 'Below $2.50'
        WHEN Fuel_Price < 3.00 THEN '$2.50 - $3.00'
        WHEN Fuel_Price < 3.50 THEN '$3.00 - $3.50'
        WHEN Fuel_Price < 4.00 THEN '$3.50 - $4.00'
        ELSE                        '$4.00 - $4.50'
    END AS Fuel_Price_Level,
    FORMAT(AVG(Weekly_Sales), 'N2') AS Avg_Weekly_Sales,
    COUNT(*) AS Week_Count
FROM Walmart_Sales
GROUP BY
    CASE
        WHEN Fuel_Price < 2.50 THEN 'Below $2.50'
        WHEN Fuel_Price < 3.00 THEN '$2.50 - $3.00'
        WHEN Fuel_Price < 3.50 THEN '$3.00 - $3.50'
        WHEN Fuel_Price < 4.00 THEN '$3.50 - $4.00'
        ELSE                        '$4.00 - $4.50'
    END
ORDER BY AVG(Weekly_Sales) DESC

--38. Do holiday weeks tend to coincide with higher or lower fuel prices?
SELECT
    Holiday_Name,
    FORMAT(AVG(Fuel_Price), 'N2') AS Avg_Fuel_Price,
    FORMAT(MIN(Fuel_Price), 'N2') AS Min_Fuel_Price,
    FORMAT(MAX(Fuel_Price), 'N2') AS Max_Fuel_Price,
    COUNT(*) AS Week_Count
FROM Walmart_Sales
GROUP BY Holiday_Name
ORDER BY AVG(Fuel_Price) DESC

--39. What is the CPI range across all stores and years?
-- Overall CPI range
SELECT
    FORMAT(MIN(CPI), 'N2')  AS Min_CPI,
    FORMAT(MAX(CPI), 'N2')  AS Max_CPI,
    FORMAT(AVG(CPI), 'N2')  AS Avg_CPI,
    FORMAT(STDEV(CPI), 'N2') AS Std_Dev
FROM Walmart_Sales;
-- CPI trend by year
SELECT
    Year,
    FORMAT(AVG(CPI), 'N2') AS Avg_CPI,
    FORMAT(MIN(CPI), 'N2') AS Min_CPI,
    FORMAT(MAX(CPI), 'N2') AS Max_CPI
FROM Walmart_Sales
GROUP BY Year
ORDER BY Year

--40. Which stores operate in the highest CPI environments?
SELECT TOP 10 Store_ID, Store, FORMAT(AVG(CPI), 'N2') AS Avg_CPI FROM Walmart_Sales
GROUP BY Store_ID, Store
ORDER BY AVG(CPI) DESC

--41. Which stores operate in the lowest CPI environments?
SELECT TOP 10 Store_ID, Store, FORMAT(AVG(CPI), 'N2') AS Avg_CPI FROM Walmart_Sales
GROUP BY Store_ID, Store
ORDER BY AVG(CPI) asc

--42. What is the unemployment rate range across all stores?
-- Overall range
SELECT
    FORMAT(MIN(Unemployment), 'N2') AS Min_Unemployment,
    FORMAT(MAX(Unemployment), 'N2') AS Max_Unemployment,
    FORMAT(AVG(Unemployment), 'N2') AS Avg_Unemployment,
    FORMAT(STDEV(Unemployment), 'N2') AS Std_Dev
FROM Walmart_Sales

--43. How has the average unemployment rate across all stores trended from 2010 to 2012?
SELECT
    Year,
    FORMAT(AVG(Unemployment), 'N2') AS Avg_Unemployment,
    FORMAT(MIN(Unemployment), 'N2') AS Min_Unemployment,
    FORMAT(MAX(Unemployment), 'N2') AS Max_Unemployment
FROM Walmart_Sales
GROUP BY Year
ORDER BY Year

--44. Which stores operate in the highest unemployment environments?
SELECT TOP 10 Store_ID, Store, FORMAT(AVG(Unemployment), 'N2') AS Avg_Unemployment FROM Walmart_Sales
GROUP BY Store_ID, Store
ORDER BY AVG(Unemployment) DESC

--45. Which stores operate in the lowest unemployment environments?
SELECT TOP 10 Store_ID, Store, FORMAT(AVG(Unemployment), 'N2') AS Avg_Unemployment FROM Walmart_Sales
GROUP BY Store_ID, Store
ORDER BY AVG(Unemployment) asc

--46. Do stores in high-unemployment regions perform better or worse than low-unemployment stores?
WITH Store_Stats AS (
    SELECT
        Store_ID,
        Store,
        AVG(Unemployment)  AS Avg_Unemployment,
        AVG(Weekly_Sales)  AS Avg_Sales
    FROM Walmart_Sales
    GROUP BY Store_ID, Store
),
Ranked AS (
    SELECT
        Store_ID,
        Store,
        Avg_Unemployment,
        Avg_Sales,
        RANK() OVER (ORDER BY Avg_Unemployment DESC) AS Unemp_Rank,
        RANK() OVER (ORDER BY Avg_Sales DESC)        AS Sales_Rank,
        CASE
            WHEN Avg_Unemployment >= 9 THEN 'High Unemployment (9%+)'
            WHEN Avg_Unemployment >= 7 THEN 'Medium Unemployment (7-9%)'
            ELSE                            'Low Unemployment (Below 7%)'
        END AS Unemployment_Tier
    FROM Store_Stats
)
SELECT
    Unemployment_Tier,
    COUNT(*) AS Store_Count,
    FORMAT(AVG(Avg_Unemployment), 'N3') AS Avg_Unemployment,
    FORMAT(AVG(Avg_Sales), 'N2')        AS Avg_Weekly_Sales
FROM Ranked
GROUP BY Unemployment_Tier
ORDER BY AVG(Avg_Unemployment) DESC

--47. Are the worst-performing stores also the highest unemployment stores — or is the relationship more complex?
WITH Store_Stats AS (
    SELECT
        Store_ID,
        Store,
        AVG(Weekly_Sales)  AS Avg_Sales,
        AVG(Unemployment)  AS Avg_Unemployment
    FROM Walmart_Sales
    GROUP BY Store_ID, Store
)
SELECT
    Store_ID,
    Store,
    FORMAT(Avg_Sales, 'N2')        AS Avg_Weekly_Sales,
    RANK() OVER (ORDER BY Avg_Sales DESC)        AS Sales_Rank,
    FORMAT(Avg_Unemployment, 'N3') AS Avg_Unemployment,
    RANK() OVER (ORDER BY Avg_Unemployment DESC) AS Unemployment_Rank
FROM Store_Stats
ORDER BY Sales_Rank DESC
OFFSET 0 ROWS FETCH NEXT 10 ROWS ONLY

--48. Do holiday weeks occur during periods of high or low unemployment on average?
SELECT
    Holiday_Name,
    FORMAT(AVG(Unemployment), 'N2') AS Avg_Unemployment,
    FORMAT(MIN(Unemployment), 'N2') AS Min_Unemployment,
    FORMAT(MAX(Unemployment), 'N2') AS Max_Unemployment,
    COUNT(*)                         AS Week_Count
FROM Walmart_Sales
GROUP BY Holiday_Name
ORDER BY AVG(Unemployment) DESC

--49. Do high CPI periods align with high or low fuel prices?
WITH Store_Factors AS (
    SELECT
        Store_ID,
        AVG(CPI)        AS Avg_CPI,
        AVG(Fuel_Price) AS Avg_Fuel_Price
    FROM Walmart_Sales
    GROUP BY Store_ID
)
SELECT
    CASE
        WHEN Avg_CPI < 150 THEN '120-150 (Low CPI)'
        WHEN Avg_CPI < 170 THEN '150-170 (Low-Mid CPI)'
        WHEN Avg_CPI < 190 THEN '170-190 (Mid CPI)'
        WHEN Avg_CPI < 210 THEN '190-210 (High-Mid CPI)'
        ELSE                    '210-230 (High CPI)'
    END AS CPI_Level,
    COUNT(*)                             AS Store_Count,
    FORMAT(AVG(Avg_CPI), 'N2')          AS Avg_CPI,
    FORMAT(AVG(Avg_Fuel_Price), 'N2')   AS Avg_Fuel_Price
FROM Store_Factors
GROUP BY
    CASE
        WHEN Avg_CPI < 150 THEN '120-150 (Low CPI)'
        WHEN Avg_CPI < 170 THEN '150-170 (Low-Mid CPI)'
        WHEN Avg_CPI < 190 THEN '170-190 (Mid CPI)'
        WHEN Avg_CPI < 210 THEN '190-210 (High-Mid CPI)'
        ELSE                    '210-230 (High CPI)'
    END
ORDER BY AVG(Avg_CPI)

--50. Which Store Has the Worst Combined External Conditions?
WITH Store_Avgs AS (
    SELECT
        Store_ID,
        Store,
        AVG(Unemployment)  AS Avg_Unemployment,
        AVG(CPI)           AS Avg_CPI,
        AVG(Fuel_Price)    AS Avg_Fuel,
        AVG(Weekly_Sales)  AS Avg_Sales
    FROM Walmart_Sales
    GROUP BY Store_ID, Store
),
Factor_Ranges AS (
    SELECT
        MAX(Avg_Unemployment) - MIN(Avg_Unemployment) AS Unemp_Range,
        MAX(Avg_CPI)          - MIN(Avg_CPI)          AS CPI_Range,
        MAX(Avg_Fuel)         - MIN(Avg_Fuel)         AS Fuel_Range,
        MIN(Avg_Unemployment)                          AS Unemp_Min,
        MIN(Avg_CPI)                                   AS CPI_Min,
        MIN(Avg_Fuel)                                  AS Fuel_Min
    FROM Store_Avgs
),
Scored AS (
    SELECT
        s.Store_ID,
        s.Store,
        s.Avg_Unemployment,
        s.Avg_CPI,
        s.Avg_Fuel,
        s.Avg_Sales,
        ((s.Avg_Unemployment - r.Unemp_Min) / r.Unemp_Range * 100 +
         (s.Avg_CPI          - r.CPI_Min)   / r.CPI_Range   * 100 +
         (s.Avg_Fuel         - r.Fuel_Min)  / r.Fuel_Range  * 100) / 3
        AS Adverse_Score
    FROM Store_Avgs s
    CROSS JOIN Factor_Ranges r
)
SELECT TOP 10
    Store_ID,
    Store,
    FORMAT(Avg_Unemployment, 'N2') AS Avg_Unemployment,
    FORMAT(Avg_CPI, 'N2')          AS Avg_CPI,
    FORMAT(Avg_Fuel, 'N2')         AS Avg_Fuel_Price,
    FORMAT(Adverse_Score, 'N1')    AS Adverse_Score_Pct,
    FORMAT(Avg_Sales, 'N2')        AS Avg_Weekly_Sales
FROM Scored
ORDER BY Adverse_Score DESC

--51. Which store showed the strongest sales growth from 2010 to 2012?
WITH Store_Year_Avg AS (
    SELECT
        Store_ID,
        Store,
        Year,
        AVG(Weekly_Sales) AS Avg_Weekly_Sales
    FROM Walmart_Sales
    WHERE Year IN (2010, 2012)
    GROUP BY Store_ID, Store, Year
),
Pivoted AS (
    SELECT
        Store_ID,
        Store,
        MAX(CASE WHEN Year = 2010 THEN Avg_Weekly_Sales END) AS Avg_2010,
        MAX(CASE WHEN Year = 2012 THEN Avg_Weekly_Sales END) AS Avg_2012
    FROM Store_Year_Avg
    GROUP BY Store_ID, Store
)
SELECT TOP 5
    Store_ID,
    Store,
    FORMAT(Avg_2010, 'N2') AS Avg_Weekly_2010,
    FORMAT(Avg_2012, 'N2') AS Avg_Weekly_2012,
    FORMAT(((Avg_2012 - Avg_2010) / Avg_2010) * 100, 'N2') AS Growth_Pct
FROM Pivoted
ORDER BY ((Avg_2012 - Avg_2010) / Avg_2010) DESC

--52. Which store showed the steepest sales decline from 2010 to 2012?
WITH Store_Year_Avg AS (
    SELECT
        Store_ID,
        Store,
        Year,
        AVG(Weekly_Sales) AS Avg_Weekly_Sales
    FROM Walmart_Sales
    WHERE Year IN (2010, 2012)
    GROUP BY Store_ID, Store, Year
),
Pivoted AS (
    SELECT
        Store_ID,
        Store,
        MAX(CASE WHEN Year = 2010 THEN Avg_Weekly_Sales END) AS Avg_2010,
        MAX(CASE WHEN Year = 2012 THEN Avg_Weekly_Sales END) AS Avg_2012
    FROM Store_Year_Avg
    GROUP BY Store_ID, Store
)
SELECT TOP 5
    Store_ID,
    Store,
    FORMAT(Avg_2010, 'N2') AS Avg_Weekly_2010,
    FORMAT(Avg_2012, 'N2') AS Avg_Weekly_2012,
    FORMAT(((Avg_2012 - Avg_2010) / Avg_2010) * 100, 'N2') AS Growth_Pct
FROM Pivoted
ORDER BY ((Avg_2012 - Avg_2010) / Avg_2010) ASC

--53. Did the overall business grow between 2010 and 2011? And between 2011 and 2012?
WITH Yearly_Avg AS (
    SELECT
        Year,
        AVG(Weekly_Sales) AS Avg_Weekly_Sales,
        COUNT(DISTINCT Date) AS Weeks_In_Year
    FROM Walmart_Sales
    GROUP BY Year
)
SELECT
    Year,
    FORMAT(Avg_Weekly_Sales, 'N2') AS Avg_Weekly_Sales,
    Weeks_In_Year,
    FORMAT(
        (Avg_Weekly_Sales - LAG(Avg_Weekly_Sales) OVER (ORDER BY Year))
        / LAG(Avg_Weekly_Sales) OVER (ORDER BY Year) * 100, 'N2'
    ) AS YoY_Growth_Pct
FROM Yearly_Avg
ORDER BY Year

