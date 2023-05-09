
# Non duplicated combined list with order kept same
```python
from collections import OrderedDict
from itertools import chain
from functools import reduce
import numpy as np

out = list(OrderedDict.fromkeys(list1 + list2 + list3)) # we can use + only for lists
out = list(OrderedDict.fromkeys(chain.from_iterable([list1, list2, list3]))) # this works for all iterables

out = reduce(lambda x, y: x + [i for i in y if i not in x], list_of_lists, [])
out = np.unique(np.concatenate(list_of_lists), return_index=True)[0]
```

# System
```python
sys.getsizeof(myvar)
cwd = os.getcwd()
parent = os.path.dirname(cwd)
```

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
