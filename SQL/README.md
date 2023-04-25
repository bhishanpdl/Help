# date: year-month, and year
```sql
SELECT 
    DATE_TRUNC('month', HSE_Created_Date) AS month_year, # 2022-10-01 (2022-oct)
    DATE_PART('year', HSE_Created_Date) AS year,# just 2022
    COUNT(DISTINCT PatientApsID) AS num_patients
    
    
-- M = 1 MM = 01 MMM = Jan  MMMM = January
date_format(DATE_TRUNC('month', HSE_Created_Date),'yyyy MMM') AS year_month # 2022 Jan

```
