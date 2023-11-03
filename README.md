# Helpful Codes
```python
pd.Timestamp('2020-01-01') + pd.DateOffset(years=1) + pd.Timedelta(days=5)

df = pd.DataFrame({'A': list('aaaa'), 'B':list('wwwx'), 'C': [1,2,1,1] })
df.groupby(['A', 'B'])[['C']].nunique().add_prefix("num_").reset_index()
df.groupby(['A','B'])['C'].apply(lambda x: x.value_counts(normalize=False)).rename_axis(index=['A', 'B', 'C']).rename('count_C').reset_index()
```
