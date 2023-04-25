# date: year-month, and year
```sql
SELECT 
    DATE_TRUNC('month', HSE_Created_Date) AS month_year, # 2022-10-01 (2022-oct)
    DATE_PART('year', HSE_Created_Date) AS year,# just 2022
    COUNT(DISTINCT PatientApsID) AS num_patients
    
    
-- 2022 Oct 
date_format(DATE_TRUNC('month', HSE_Created_Date),'yyyy MMM') AS year_month,

```
