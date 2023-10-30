# Helpful Codes
```python
df.groupby(['A', 'B'])[['C']].nunique().add_prefix("num_").reset_index()
```
