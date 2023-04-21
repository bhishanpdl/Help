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
