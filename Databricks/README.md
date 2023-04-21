# databricks
```python
%fs ls /databricks-datasets
show tables in datascience

%jupysql duckdb:///:memory:

sdf.createOrReplaceTempView('sdf')
spark.createDataFrame(pandas_df)

sdf.write.option('overwriteSchema','true').mode('overwrite').saveAsTable('datascience.test')
```
# filter data
```python
sdf.filter(F.lower(F.col('A')).like('%xx%'))
sdf.filter(F.lower(F.col('A')).contains('xx'))
sdf.filter(F.col('A').rlike('(?i)xx')) # regex like ignorecase
```

