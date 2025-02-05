# Hackerrank-SQL-query-advance
### 1./ Weather Analysis
There is a table with daily weather data over the last 6 months of 2020, including the maximum, minimum, and average temperatures.

![image](https://github.com/user-attachments/assets/b2f73c33-0671-4090-a76c-deccd4cd654a)
![image](https://github.com/user-attachments/assets/d0e715a5-437a-401d-a6e1-6b364a78382e)

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

### 2./ Crypto Market Transactions Monitoring
Crypto Market Transaction Monitoring Solution
![image](https://github.com/user-attachments/assets/3268cace-0d27-4e45-a7ef-5c5f1a90e397)
![image](https://github.com/user-attachments/assets/eb735d66-1d3e-45e9-878a-ec88c23c9ed9)

```
WITH df AS (
  SELECT *,
         TIMESTAMPDIFF(MINUTE, LAG(dt) OVER (PARTITION BY sender ORDER BY dt), dt) AS df_minute
  FROM transactions
),
rn AS (
  SELECT *,
         SUM(CASE WHEN df_minute IS NULL OR df_minute >= 60 THEN 1 ELSE 0 END) 
         OVER (PARTITION BY sender ORDER BY dt) AS sequence_id
  FROM df
),
ss AS (
  SELECT sender,
         sequence_id,
         MIN(dt) AS sequence_start,
         MAX(dt) AS sequence_end,
         COUNT(*) AS transactions_count,
         ROUND(SUM(amount), 6) AS transactions_sum
  FROM rn
  GROUP BY sender, sequence_id
  HAVING transactions_sum >= 150
)
SELECT sender, sequence_start, sequence_end, transactions_count, transactions_sum
FROM ss
ORDER BY sender, sequence_start, sequence_end;
```
