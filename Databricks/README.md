# databricks
```python
%fs ls /databricks-datasets
%sql show tables in datascience

%jupysql duckdb:///:memory:

sdf.createOrReplaceTempView('sdf')
spark.createDataFrame(pandas_df)

sdf.write.option('overwriteSchema','true').mode('overwrite').saveAsTable('datascience.test')

# some settings to read very big tables
spark.conf.set("spark.sql.execution.arrow.pyspark.enabled", "false")
```

# Spark read load a delta folder
```python
s_hse = spark.read.load('/mnt/databricksprod1/silver/hse/')
s_hse_str = s_hse.select([s_hse[col].cast("string") for col in s_hse.columns])
hse = s_hse_str.toPandas()
del s_hse
del s_hse_str
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

# Display Azure Portal stored images in notebook
```python
from PIL import Image
img_path = "/dbfs/mnt/datascience/datascience/Eylea/Eylea_to_EyleaHD_Transition/eylea 01.png"
display(Image.open(img_path))
```
