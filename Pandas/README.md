# Useful Operations
```python
df.groupby(['A', 'B']).agg({'C': 'nunique'}).add_prefix("num_").reset_index() # always choose nunique/count/size carefully
df.groupby(['A', 'B']).agg(num_C=('C','count')).reset_index()
df.groupby(['A', 'B'])[['C']].nunique().add_prefix("num_").reset_index() # groupby and add prefix

df.groupby(['A','B'])['C'].apply(lambda x: x.value_counts(normalize=True)).rename_axis(index=['A', 'B', 'C']).rename('count_C').reset_index()

df.apply(lambda row: row['A'] in row['B'], axis=1) # check if substring from columnA exists in columnB
df.loc[lambda x:  x['A'].astype(str).str.len() > 100 ].head(1) # select where column have some values
df.filter(regex='time|TIME') # df.filter(like='time') # newer version also have case=True parameter.
df['A'].astype(str).str.contains(substr, regex=False) # substr = '"name": "AnnexusHealth"' works for json, but single Quote does not work.

# move values of a column to columns
df.pivot_table(index=['Site_ID', 'YearQtr'], columns='Product_Name', values='Record_ID', aggfunc='sum',fill_value=0).reset_index().rename_axis(None,axis=1)

# rename value counts
df['A'].value_counts().rename('count').rename_axis('A').reset_index() # make column name and rename_axis SAME!!

# date time operations
ser = pd.date_range(start='2020-01-01',end='2020-01-05',freq='D')
ser2 = ser - pd.DateOffset(years=1) + pd.Timedelta(days=10)

# value counts with %
df.groupby(['A','B'])['C'].value_counts(normalize=True).mul(100).round(2).astype(str).add('%').reset_index()
```

# Pandas Datetime
```python
pd.Timestamp('2020-01-01') + pd.DateOffset(years=1) + pd.Timedelta(days=5)
```

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

#========== parse date column
data = {'CreatedDate': ['2023-09-12 16:40:24.60024017', '2023-08-25 10:15:30.12345678']}
df = pd.DataFrame(data)
df['CreatedDate'] = pd.to_datetime(df['CreatedDate'], format='%Y-%m-%d %H:%M:%S.%f') # %f is microseconds with 0 padded format for 8 digits
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
df = pd.DataFrame(columns=['a','b','c_min','d_max','e_min_x'])
# use negative lookahead to filter columns
df.filter(regex='^(?!.*(_min|_max)).*$') # only a and b (anything without _min and _max in columns)
```

# groupby year+month
```python
df = pd.DataFrame({
    "Country": ["US", "UK", "US"],
    "Date": pd.to_datetime(["2023-01-01", "2023-02-01", "2023-01-02"])
})

(df.groupby('Country')
 .apply(lambda row: row['Date'].dt.strftime('%Y-%m')
        .value_counts()).unstack(0).sort_index()
)

# using pivot_table
(df
 .assign(Date_ym = lambda x: x['Date'].dt.strftime('%Y-%m'))
 .pivot_table(index='Date_ym', columns='Country', aggfunc='size', fill_value=0).sort_index()
)

Country  UK  US     
2023-01   0   2
2023-02   1   0
```

# replace multiple values
```python
na_values = ['na', 'NaN', ' ', '', 'nan' ]
na_values_map = {i: 'null' for i in na_values}

final2 = final.copy(deep=True)

for col in final2.columns:
    if final2[col].dtype == 'O':
        final2[col] = final2[col].astype(str).map(na_values_map).fillna(final2[col])

final2.head(2)
```

# Creating rank 1,2,3 from duplicate elements of a column
```python
col = 'Id'
df = pd.DataFrame({col: [1,1,2,3], 'Value': [10,20,30,40]})
df[f'{col}_Rank'] = df.groupby(col).cumcount() + 1

dups = df.loc[lambda x: x[f'{col}_Rank']>1]
dup_ids = dups[col].unique()
df.loc[lambda x: x[col].isin(dup_ids)][[df.columns[-1]] + list(df.columns[:-1])]

   Id  Value  Id_Rank
0   1     10        1
1   1     20        2
2   2     30        1
3   3     40        1
```

# Get column names where values are null
```python
df[['A', 'B', 'C']].apply(lambda x: ','.join(x.index[x.eq('null')]), axis=1)
```

# Pivot and pivot_table: move values of a column to the columns
```python
df2 = df.pivot_table(index=['Site_ID', 'YearQtr'], columns='Product_Name', values='Record_ID', aggfunc='sum',fill_value=0).reset_index().rename_axis(None,axis=1).sort_values('YearQtr')
```
