# Scrape a table
```python
import pandas as pd
import requests
import lxml # needed for parsing behind the scene

url = "https://spark.apache.org/docs/latest/sql-ref-datetime-pattern.html"
r = requests.get(url,verify=False)
df_list = pd.read_html(r.text) # this parses all the tables in webpages to a list
df = df_list[0]
df.head()

# save this table
sdf = spark.createDataFrame(df)
sdf.write.mode('overwrite').option('overwriteSchema','true').saveAsTable('datascience.Spark_Datetime_Patterns')
```
