# Hackerrank-SQL-query-advance
### / Weather Analysis
There is a table with daily weather data over the last 6 months of 2020, including the maximum, minimum, and average temperatures.

Write a query that gives month, monthly maximum, monthly minimum, monthly average temperatures for the six months.

Note: Round the average to the nearest integer.

``` 
WITH MaxTemp AS (
    SELECT 
        MONTH(record_date) AS R, 
        MAX(data_value) AS D
    FROM temperature_records 
    WHERE data_type = 'max' 
    GROUP BY MONTH(record_date)
),
MinTemp AS (
    SELECT 
        MONTH(record_date) AS R, 
        MIN(data_value) AS D
    FROM temperature_records 
    WHERE data_type = 'min' 
    GROUP BY MONTH(record_date)
),
AvgTemp AS (
    SELECT 
        MONTH(record_date) AS R, 
        ROUND(AVG(data_value), 0) AS D
    FROM temperature_records 
    WHERE data_type = 'avg' 
    GROUP BY MONTH(record_date)
)
SELECT 
    M.R AS Month, 
    M.D AS Max_Temperature, 
    N.D AS Min_Temperature, 
    A.D AS Avg_Temperature
FROM MaxTemp M
JOIN MinTemp N ON M.R = N.R
JOIN AvgTemp A ON M.R = A.R;
```
