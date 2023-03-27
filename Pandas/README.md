# agg with filters
```python
df = pd.DataFrame({'Year': [2021, 2022, 2023, 2023],'Month': [1,2,1,2],'Sales': [100, 120, 130, 90]})

df.agg(
    SalesForYear2023 = ('Sales', lambda ser: ser[df.Year==2023].sum()),
    SalesForMonth01 = ('Sales', lambda ser: ser[df.Month==1].sum())
)
```
