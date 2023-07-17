# Some Python Features
```python
# attributes that takes multiple values
x = [i for i in lst if i.endswith((".csv", ".xls", ".xlsx"))]
assert isinstance(7, (float, int))

# transpose a list of lists
transposed_list = zip(*mylists)

# importing local modules
# https://docs.python.org/3/faq/programming.html#what-are-the-best-practices-for-using-import-in-a-module
from mymodule import myfunction # from xxx may have circular refrence
import mymodule # import mymodule is recommended way to deal with circular references

# using timer function
start_time = timeit.default_timer() # time only goes forward using this method
start_time = time.time() # it can go backward for leap seconds, clock adjustments, daylight savings

# string formatting
assert f"{pct:.1%}" == "12.3%"  # The new way

# convert number to currency
import locale
print(locale.locale_alias)
locale.setlocale(locale.LC_ALL, 'en_US.UTF-8')
assert locale.currency(1234, grouping=True) == "$1,234.00"

# functools
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

# Pathlib
```python
# all csv files not starting with meta_
path = Path(r"C:\Users\a126291\OneDrive - AmerisourceBergen(ABC)\GPS\p_982_Infinitus_Projections\output")
files = [file for file in path.glob('*.csv') if not re.match(r'meta_', file.name)]
len(files), files[0]

# aliter
files = glob.glob(r"C:\Users\a126291\OneDrive - AmerisourceBergen(ABC)\GPS\p_982_Infinitus_Projections\output\*.csv")
files = [i for i in files if os.path.basename(i)[0:4]!='meta']
```

# Importlib
```python
# these are local scripts we have at PWD (note, there is pd.py helper file, not the original module pandas)
__all__ = ["data", "mpl", "pd", "pl", "pt", "sk", "vega"]

import importlib

def __getattr__(name):
    if name in __all__:
        return importlib.import_module(f".{name}", __name__)

    raise AttributeError(f"module {__name__!r} has no attribute {name!r}")

# Example usage
import data # instead of import .data we use import data
import mpl
import pd
```

# Unpack zip/tar files
```python
import shutil
shutil.unpack_archive("my_archive.tar.gz", extract_dir="unpacked")
```

# itertools
```python
#========= creating grid
import itertools

grid = [divmod(x, 3) for x in range(2 * 3)]

assert grid == list(itertools.product(range(2), range(3)))
assert grid == [(row, col) for row in range(2) for col in range(3)]
assert grid == [
    (0, 0),
    (0, 1),
    (0, 2),
    (1, 0),
    (1, 1),
    (1, 2),
]
```

# Datetime
1. Sort calendar days
```python
import calendar

# A list of days you would like to have sorted
days = ["Tuesday", "Monday", "Saturday", "Monday"]

day_names = list(calendar.day_name)
days.sort(key=day_names.index)

assert days == ['Monday', 'Monday', 'Tuesday', 'Saturday']
```

# Python Basics
1. Functions can have attributes
```python
def multiply(a, b):
    return a * b

multiply.test_cases = [
    ((2, 3), 6),
    ((0, 1), 1),
    ((4, 4), 16),
]
```

# Deepcopy (copies list of list, not references)
```python
from copy import deepcopy

my_list = [{"one": 1}]

# Create two types of copies
shallow_copy = my_list.copy()
deep_copy = deepcopy(my_list)

# Change the original
my_list.append({"two": 2})
my_list[0]["one"] = 77 

# Look at the changes
assert my_list == [{"one": 77}, {"two": 2}]
assert shallow_copy == [{"one": 77}]  # Mutated!
assert deep_copy == [{"one": 1}]
```

# Typing
1. typing Literal
```python
from typing import Literal

def do_something(color: Literal["Red", "Blue", "Green"]):
    ...
```

2. typing Final (python 3.8)
```python
from typing import Final
MAX_SIZE: Final = 9000
MAX_SIZE += 1  # Error reported by type checker but does not fail in runtime
```

# numpy
1. numpy integers are not 'int' but 'np.int64' or others
```python
from numbers import Integral
import numpy as np

for item in np.array([1, 2, 3]):
    assert isinstance(item, Integral)
    assert not isinstance(item, int)  # These aren't really ints!
```
