# databricks
```python
%fs ls /databricks-datasets
show tables in datascience

%jupysql duckdb:///:memory:

sdf.createOrReplaceTempView('sdf')
spark.createDataFrame(pandas_df)

sdf.write.option('overwriteSchema','true').mode('overwrite').saveAsTable('datascience.test')
```
