# Plotly Express
```python
# vertical rectangles
fig = px.line(df[col_val])
valid_end_date   = inp.index[-1].strftime("%Y-%m-%d")
valid_start_date = (df.index[-1] - pd.Timedelta(days=60-1)).strftime("%Y-%m-%d")
_ = fig.add_vrect(x0=start_date, x1=end_date, fillcolor="grey", opacity=0.25, line_width=0)
fig.show()

```
