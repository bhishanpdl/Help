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
