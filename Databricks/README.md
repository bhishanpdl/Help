# databricks
```python
%fs ls /databricks-datasets
%sql show tables in datascience

%jupysql duckdb:///:memory:

sdf.createOrReplaceTempView('sdf')
spark.createDataFrame(pandas_df)

sdf.write.option('overwriteSchema','true').mode('overwrite').saveAsTable('datascience.test')
```

# Spark read load a delta folder
```python
hse = spark.read.load('/mnt/databricksprod1/silver/hse/').toPandas()
```

# Read file from Azure Portal
```python
!ls /dbfs/mnt/datascience/datascience/RXLightning
%fs ls /mnt/datascience/datascience/RXLightning

ifile = "/mnt/datascience/datascience/RXLightning/Billing Matrix_V1.csv"
bill = spark.read.option("header","true").csv(ifile).toPandas()

bill.show(2)
```

# filter data
```python
sdf.filter(F.lower(F.col('A')).like('%xx%'))
sdf.filter(F.lower(F.col('A')).contains('xx'))
sdf.filter(F.col('A').rlike('(?i)xx')) # regex like ignorecase
```

# Convert to Pandas
```python
df = sdf.select([sdf[col].cast("string") for col in sdf.columns]).toPandas()
```
