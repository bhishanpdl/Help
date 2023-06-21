# Pandas Functions
```python
pd.crosstab(df['A'],df['B'], margins=True, margins_name="Total")
df['A'].apply([min,max])
df.groupby('A')['B'].apply(lambda x: [x.min(), x.max()])

# group by column and get value counts for categorical values
cols_vc = ['C1','C2','C3']
df.groupby('A').apply(lambda group: group[cols_vc].apply(pd.Series.value_counts)).unstack()

# group by country and get totals for number columns
cols_sum = ['F1', 'F2']
df.groupby('A').apply(lambda group: group[cols_sum].sum(axis=0))
```

# Parse date format (converters, dtype, parse_dates, dayfirst)
```python
def date_parser(date_string):
    return pd.to_datetime(date_string, format='%d/%m/%Y')
df = pd.read_excel(ifile, dtype={'A': str}, converters={col_date: date_parser}) # using converters
df = pd.read_csv(ifile,parse_dates=[col_date],dayfirst=False, date_format='%d/%m/%Y') # using date_format

# example of converters
import functools
def func(age,param): 
    return age+param
converters = {'Age': functools.partial(func, param=10)}
df = pd.read_excel(ifile,converters=converters)
```

# Column dtype str vs float
```python
df = pd.read_excel(ifile, dtype={'A': str}, converters={col_date: date_parser})
df['A Float'] = pd.to_numeric(df['A'], errors='coerce')
df['A Str'] = df['A'].apply(lambda x: '{:08.2f}'.format(x))
```

# applymap
```python
df = pd.DataFrame({ 'col1': [(1, 2), (3, 4), (5, 6)], 'col2': [(7, 8), (9, 10), (11, 12)] })
df = df.applymap( lambda x: x[1]) # apply function to all the columns of dataframe
```

# agg with filters
```python
df = pd.DataFrame({'Year': [2021, 2022, 2023, 2023],'Month': [1,2,1,2],'Sales': [100, 120, 130, 90]})

df.agg(
    SalesForYear2023 = ('Sales', lambda ser: ser[df.Year==2023].sum()),
    SalesForMonth01 = ('Sales', lambda ser: ser[df.Month==1].sum())
)
```

# groupby
```python
df.groupby('A').agg(  
    is_begin_same = ('BeginDate', lambda x: x.min()==x.max()),    
    is_end_same = ('EndDate', lambda x: x.min()==x.max()),
).reset_index()
```

# filter
```python
# use negative lookahead to filter columns
df.filter(regex='^(?!.*(_min|_max)).*$')
```
