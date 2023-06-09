# Summary
```python
functools.reduce(lambda s, r: s.replace(*r), rep, text.title()) # rep = [('from1','to1'),('frm2','to2')]
unq_ordered_list = list(OrderedDict.fromkeys(chain(*[list1, list2, list3]))) 
```

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

# Flat list
```python
import itertools

flat = list(itertools.chain(*list_of_lists))
flat = list(itertools.chain.from_iterable(list_of_lists))
flat = [i for sublist in list_of_lists for i in sublist] # (for i in sublist) is second for loop. (i for sublist in list_of_lists) is first one.
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

# Generalize multiple replaces
```python
import functools

def func(text):
    replacements = [('_', ' '), ('Json', 'JSON'), ('Sql', 'SQL')]
    return functools.reduce(lambda s, r: s.replace(*r), replacements, text.title())

# Example usage
print(func('my_json_file_name')) # My JSON File Name
#out = text.title().replace('_', ' ').replace('Json','JSON').replace('Sql','SQL')
```

# Paths
```python
# all csv files not starting with meta_
path = Path(r"C:\Users\a126291\OneDrive - AmerisourceBergen(ABC)\GPS\p_982_Infinitus_Projections\output")
files = [file for file in path.glob('*.csv') if not re.match(r'meta_', file.name)]
len(files), files[0]

# aliter
files = glob.glob(r"C:\Users\a126291\OneDrive - AmerisourceBergen(ABC)\GPS\p_982_Infinitus_Projections\output\*.csv")
files = [i for i in files if os.path.basename(i)[0:4]!='meta']
```
