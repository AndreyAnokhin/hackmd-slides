---
slideOptions:
  transition: none
  progress: false
  slideNumber: true
  theme: league

title: functools
---

## Functools
#### Higher-order functions and operations on callable objects.

---

<p style='text-align: justify; font-size: 20pt'>
The functools module is for higher-order functions: functions that act on or return other functions. In general, any callable object can be treated as a function for the purposes of this module.<br>
The functools module defines the following functions:
</p>


```python
@lru_cache
```
```python
@total_ordering
```
```python
partial(func, /, *args, **keywords)
```
```python
reduce(function, iterable)
```
```python
class functools.partialmethod(func, /, *args, **keywords)
```
```python
class functools.singledispatchmethod(func)
```

---

### Partial functions

Python functools partial functions are used to:
- Replicate existing functions with some arguments already passed in.
- Creating new version of the function in a well-documented manner.

---

```python
def multiplier(x, y):
    return x * y

```
```python
def doubleIt(x):
    return multiplier(x, 2)

def tripleIt(x):
    return multiplier(x, 3)
```
```python

from functools import partial

def multiplier(x, y):
    return x * y

double = partial(multiplier, y=2)
triple = partial(multiplier, y=3)

print('Double of 2 is {}'.format(double(5)))
```
```python
Double of 2 is 10
```

---

#### Partial functions are self-documented:
```python

from functools import partial

def multiplier(x, y):
    return x * y

double = partial(multiplier, y=2)
triple = partial(multiplier, y=3)

print(f'Function powering double is {double.func}')
print(f'Default keywords for double is {double.keywords}')

```
```python
Function powering double is 
<function multiplier at 0x7fee34db0bf8>

Default keywords for double is {'y': 2}
```

---

#### update_wrapper()
```python
def multiplier(x, y):
    """Multiplier doc string."""
    return x * y

double = functools.partial(multiplier, y=2)
```
```python
double.__name__
>> AttributeError: 'functools.partial' object 
'has no attribute '__name__'

double.__doc__
>> 'partial(func, *args, **keywords) - new function 
'with partial application of the given arguments and keywords'
```
```python
functools.update_wrapper(double, multiplier)
```
```python
double.__name__
>> 'multiplier'

double.__doc__
>> 'Multiplier doc string.'
```

---

####  @wraps decorator
```python
import functools

def do_twice(func):
    @functools.wraps(func)
    def wrapper_do_twice(*args, **kwargs):
        func(*args, **kwargs)
        return func(*args, **kwargs)
    return wrapper_do_twice
```
```python
>>> say_whee
<function say_whee at 0x7ff79a60f2f0>

>>> say_whee.__name__
'say_whee'

>>> help(say_whee)
Help on function say_whee in module whee:

say_whee()
```

---

#### partialmethod()

```python
from functools import partialmethod
class Cell(object):
    def __init__(self):
        self._alive = False
    @property
    def alive(self):
        return self._alive
    def set_state(self, state):
        self._alive = bool(state)
    set_alive = partialmethod(set_state, True)
    set_dead = partialmethod(set_state, False)
```
```python
>>> c = Cell()
>>> c.alive
False
>>> c.set_alive()
>>> c.alive
True
```

---

#### @total_ordering
<p style='text-align: justify; font-size: 20pt'>
Provide a way to provide automatic comparison functions. There are 2 conditions we needs:
Definition of at least one comparison function is a must like `le`, `lt`, `gt` or `ge`.
Definition of `eq` function is mandatory.
</p>

```python
from functools import total_ordering

@total_ordering
class Number:
    def __init__(self, value):
        self.value = value
    def __lt__(self, other):
        return self.value < other.value
    def __eq__(self, other):
        return self.value == other.value
```

---

```python
print(Number(1) < Number(2))
print(Number(10) > Number(21))
print(Number(10) <= Number(2))
print(Number(10) >= Number(20))
print(Number(2) <= Number(2))
print(Number(2) >= Number(2))
print(Number(2) == Number(2))
print(Number(2) == Number(3))
```
```
True
False
False
False
True
True
True
False
```

---

#### Fibonacci sequence

<p style='text-align: justify; font-size: 20pt'>
The Fibonacci sequence is a sequence of numbers such that any number, except for the first and second, is the sum of the previous two:</p>
<p style='text-align: center; font-size: 20pt'>
0, 1, 1, 2, 3, 5, 8, 13, 21...
</p>
<p style='text-align: center; font-size: 20pt'>
Fibonacci number formula:</br>
fib(n) = fib(n - 1) + fib(n - 2)
</p>

```python
def fib(n):
    if n < 2: # base case 
        return n
    return fib(n - 2) + fib(n - 1) # recursive case
```

---

```python
%%timeit
fib(35)
'4.75 s ± 122 ms per loop'
```
![fib_tree](https://i.imgur.com/OBkKc6t.png)

---

#### Memoization
```python
memo = {0: 0, 1: 1} # our base cases
def fib(n):
    if n not in memo:
        memo[n] = fib(n - 1) + fib(n - 2) # memoization
    return memo[n]
```
```python
%%timeit
fib(35)
'221 ns ± 3.58 ns per loop'
```

---

#### Least recently used (LRU) Cache

```python
from functools import lru_cache

@lru_cache(maxsize=None)
def fib(n):
    if n < 2: # base case 
        return n
    return fib(n - 2) + fib(n - 1) # recursive case
```
```python
%%timeit
fib(35)
'136 ns ± 1.33 ns per loop'
```

---

#### @singledispatch
<p style='text-align: justify; font-size: 20pt'>
Transform a function into a single-dispatch generic function.
</p>

```python
from functools import singledispatch

@singledispatch
def fprint(data):
    print(f'({type(data).__name__}) {data}')
```
```python
@fprint.register(list)
def _(data):
    formatted_header = f'{type(data).__name__} -> index : value'
    print(formatted_header)
    print('-' * len(formatted_header))
    for index, value in enumerate(data):
        print(f'{index} : ({type(value).__name__}) {value}')
```

---

```python
@fprint.register(dict)
def _(data):
    formatted_header = f'{type(data).__name__} -> key : value'
    print(formatted_header)
    print('-' * len(formatted_header))
    for key, value in data.items():
        print(f'({type(key).__name__}) {key}: ({type(value).__name__}) {value}')
```
```shell
> fprint(42)
(int) 42

> fprint([1, 2, '3'])
list -> index : value
---------------------
0 : (int) 1
1 : (int) 2
2 : (str) 3

> fprint({'name': 'John Doe', 'age': 32, 'location': 'New York'})
dict -> key : value
-------------------
(str) name: (str) John Doe
(str) age: (int) 32
(str) location: (str) New York
```

---

### cmp_to_key(func)
<p style='text-align: justify; font-size: 20pt'>
Transform an old-style comparison function to a key function.
Old comparison functions used to take two values and return -1, 0 or +1 if the first argument is small, equal or greater than the second argument respectively.
</p>


```python
def cmp(a, b):
    return (a > b) - (a < b) 
```
```python
sorted(iterable, key=cmp_to_key(cmp))
```

---

### sorted() function
```python
>>> sorted([5, 2, 3, 1, 4])
[1, 2, 3, 4, 5]
```
```python
>>> strs = ['aa', 'BB', 'zz', 'CC']
>>> sorted(strs)
['BB', 'CC', 'aa', 'zz']
```
```python
>>> sorted(strs, key=str.lower)
['aa', 'BB', 'CC', 'zz']
```
```python
>>> strs = ['ccc', 'aaaa', 'd', 'bb']
>>> sorted(strs, key=len)
['d', 'bb', 'ccc', 'aaaa']
```

---

### reduce() function
<p style='text-align: justify; font-size: 20pt'>
Apply function of two arguments cumulatively to the items of sequence, from left to right, so as to reduce the sequence to a single value.
</p>

![](https://i.imgur.com/YgCQKCX.png)

```python
from functools import reduce

reduce(lambda x, y: x+y, [1, 2, 3, 4, 5]) 
```
```python
((((1+2)+3)+4)+5)
15
```

---

#### @cached_property (Python 3.8)

```python
class DataSet:
    def __init__(self, sequence_of_numbers):
        self._data = sequence_of_numbers

    @cached_property
    def stdev(self):
        return statistics.stdev(self._data)

    @cached_property
    def variance(self):
        return statistics.variance(self._data)
```

---

### Thank you
