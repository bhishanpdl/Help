# kwargs
```python
def another_function(a=5,b=6):
    return a-b

def func(arg1, **kw):
    print(f"arg1: {arg1}")
    res = another_function(**kw)
    print(f'output of another function: {res}')
    return res

func('hello',b=2,a=6)
```
