---
slideOptions:
  transition: none
  progress: false
  slideNumber: true
  theme: league

title: Python Decorators
---

## Python Decorators

---

### Definition
<p style='text-align: justify; font-size: 20pt'>
A decorator is a function that takes another function and extends the behavior of the latter function without explicitly modifying it
</p>

```python
@uppercase
def greet():
    return 'Hello!'
```
```python
>>> greet()
'HELLO!'
```
<p style='text-align: justify; font-size: 20pt'>
Decorators provide a simple syntax for calling higher-order functions.
</p>

---

### Functions are objects

```python
def yell(text):
    return text.upper() + '!'
```
```python
>>> yell('hello')
HELLO!
```


```python
>>> bark = yell
```
```python
>>> bark('woof')
WOOF!
```
```python
>>> del yell

>>> yell('hello?')
NameError: "name 'yell' is not defined"

>>> bark('hey')
'HEY!'
```
```python
>>> bark.__name__
'yell'

---

#### Functions can be stored in data structures
```python
>>> funcs = [bark, str.lower, str.capitalize]
>>> funcs
[<function yell at 0x10ff96510>,
<method 'lower' of 'str' objects>,
<method 'capitalize' of 'str' objects>]
```
```python
>>> for f in funcs:
...     print(f, f('hey there'))
<function yell at 0x10ff96510> 'HEY THERE!'
<method 'lower' of 'str' objects> 'hey there'
<method 'capitalize' of 'str' objects> 'Hey there'
```
<p style='text-align: justify; font-size: 20pt'>
You can even call a function object stored in the list without first assigning it to a variable:
</p>

```python
>>> funcs[0]('heyho')
'HEYHO!'
```

---

#### Functions can be passed to other functions:

```python
def greet(func):
    greeting = func('Hi, I am a Python program')
    print(greeting)
```
<p style='text-align: justify; font-size: 20pt'>
You can influence the resulting greeting by passing in different functions:
</p>

```python
>>> greet(bark)
'HI, I AM A PYTHON PROGRAM!'
```

---

### Inner functions

```python
def speak(text):
    def whisper(t):
        return t.lower() + '...'
    return whisper(text)
```
```sh
>>> speak('Hello, World')
'hello, world...'
```
<br>

```python
>>> whisper('Yo')
# ???




```

---

### Inner functions

```python
def speak(text):
    def whisper(t):
        return t.lower() + '...'
    return whisper(text)
```
```sh
>>> speak('Hello, World')
'hello, world...'
```
<br>

```python
>>> whisper('Yo')
NameError:
"name 'whisper' is not defined"



```

---

### Inner functions

```python
def speak(text):
    def whisper(t):
        return t.lower() + '...'
    return whisper(text)
```
```sh
>>> speak('Hello, World')
'hello, world...'
```
<br>

```python
>>> whisper('Yo')
NameError:
"name 'whisper' is not defined"
>>> speak.whisper
# ???

```

---

### Inner functions

```python
def speak(text):
    def whisper(t):
        return t.lower() + '...'
    return whisper(text)
```
```sh
>>> speak('Hello, World')
'hello, world...'
```
<p style='text-align: justify; font-size: 20pt'>
But inner function 'whisper' does not exist outside 'speak':
</p>

```python
>>> whisper('Yo')
NameError:
"name 'whisper' is not defined"
>>> speak.whisper
AttributeError:
"'function' object has no attribute 'whisper'"
```

---


### Return functions
<p style='text-align: justify; font-size: 20pt'>
Functions are objects — you can return the inner function to the caller of the parent function:
</p>

```python
def get_speak_func(volume):
    def whisper(text):
        return text.lower() + '...'
    def yell(text):
        return text.upper() + '!'
    if volume > 0.5:
        return yell
    else:
        return whisper
```

---

<p style='text-align: justify; font-size: 20pt'>
get_speak_func doesn’t call any of its inner functions - it simply returns the function object:
</p>

```python
>>> get_speak_func(0.3)
<function get_speak_func.<locals>.whisper at 0x10ae18>
>>> get_speak_func(0.7)
<function get_speak_func.<locals>.yell at 0x1008c8>
```
<p style='text-align: justify; font-size: 20pt'>
You can call the returned function:
</p>

```python
>>> speak_func = get_speak_func(0.7)
>>> speak_func('Hello')
'HELLO!'
```

---

#### Functions can capture local state

```python
def make_adder(n):
    def add(x):
        return x + n
    return add
```
```python
>>> plus_3 = make_adder(3)
>>> plus_5 = make_adder(5)
>>> plus_3(4)
7
>>> plus_5(4)
9
```
Functions that do this are called lexical closures.

---

### Simple Decorators
```python
def my_decorator(func):
    def wrapper():
        print("Do something before the function is called.")
        func()
        print("Do something after the function is called.")
    return wrapper
```
```python
def say_whee():
    print("Whee!")
```
```python
say_whee = my_decorator(say_whee)
```
```sh
>>> say_whee()
Do something before the function is called.
Whee!
Do something after the function is called.
```
<p style='text-align: justify; font-size: 20pt'>
The name say_whee now points to the wrapper() inner function:
</p>

```python
>>> say_whee
<function my_decorator.<locals>.wrapper at 0x7f3c5dfd42f0>
```

---

### Syntactic Sugar
<p style='text-align: justify; font-size: 20pt'>
Using the @ syntax is just syntactic sugar and a shortcut for this commonly used pattern.
</p>

```python
@my_decorator
def say_whee():
    print("Whee!")
```
<p style='text-align: justify; font-size: 20pt'>
@my_decorator is just an easier way of saying:</p>
<p style='text-align: center; font-size: 20pt'>say_whee = my_decorator(say_whee)</p>
<p style='text-align: justify; font-size: 20pt'>
It’s how you apply a decorator to a function.
</p>

---

#### Applying multiple decorators to a function
```python
def strong(func):
    def wrapper():
        return '<strong>' + func() + '</strong>'
    return wrapper
def emphasis(func):
    def wrapper():
        return '<em>' + func() + '</em>'
    return wrapper
```
```python
@strong
@emphasis
def greet():
    return 'Hello!'
```
```python
>>> greet()
# ???
```

---

#### Applying multiple decorators to a function
```python
def strong(func):
    def wrapper():
        return '<strong>' + func() + '</strong>'
    return wrapper
def emphasis(func):
    def wrapper():
        return '<em>' + func() + '</em>'
    return wrapper
```
```python
@strong
@emphasis
def greet():
    return 'Hello!'
```
```python
>>> greet()
'<strong><em>Hello!</em></strong>'
```

---

#### Applying multiple decorators to a function
```python
def strong(func):
    def wrapper():
        return '<strong>' + func() + '</strong>'
    return wrapper
def emphasis(func):
    def wrapper():
        return '<em>' + func() + '</em>'
    return wrapper
```
```python
@strong
@emphasis
def greet():
    return 'Hello!'
```
```python
>>> greet()
'<strong><em>Hello!</em></strong>'
```

<p style='text-align: justify; font-size: 20pt'>
The decorators were applied from bottom to top.
The chain of decorator function calls looks like this:
</p>

```python
decorated_greet = strong(emphasis(greet))
```

---

#### Decorating functions with arguments
```python
def do_twice(func):
    def wrapper_do_twice():
        func()
        func()
    return wrapper_do_twice
```
```python
@do_twice
def greet(name):
    print(f"Hello {name}")
```
```python
>>> greet("World")
# ???



```

---

#### Decorating functions with arguments
```python
def do_twice(func):
    def wrapper_do_twice():
        func()
        func()
    return wrapper_do_twice
```
```python
@do_twice
def greet(name):
    print(f"Hello {name}")
```
```python
>>> greet("World")
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: wrapper_do_twice() takes 0 positional arguments 
but 1 was given
```

---

#### Decorating functions with arguments
<p style='text-align: justify; font-size: 20pt'>
The solution is to use *args and **kwargs in the inner wrapper function. Then it will accept an arbitrary number of positional and keyword arguments:
</p>

```python
def do_twice(func):
    def wrapper_do_twice(*args, **kwargs):
        func(*args, **kwargs)
        func(*args, **kwargs)
    return wrapper_do_twice
```
```python
@do_twice
def greet(name):
    print(f"Hello {name}")
```
```python
>>> greet("World")
Hello World
Hello World
```

---

#### Returning values from decorated functions
```python
@do_twice
def return_greeting(name):
    print("Creating greeting")
    return f"Hi {name}"
```
```python
>>> hi_adam = return_greeting("Adam")
Creating greeting
Creating greeting
>>> print(hi_adam)
# ???
```

<br>

---

#### Returning values from decorated functions
```python
@do_twice
def return_greeting(name):
    print("Creating greeting")
    return f"Hi {name}"
```
```python
>>> hi_adam = return_greeting("Adam")
Creating greeting
Creating greeting
>>> print(hi_adam)
None
```
<p style='text-align: justify; font-size: 20pt'>
Because the do_twice_wrapper() doesn’t explicitly return a value, the call return_greeting("Adam") ended up returning None.
</p>

---

#### Returning values from decorated functions
```python
def do_twice(func):
    def wrapper_do_twice(*args, **kwargs):
        func(*args, **kwargs)
        return func(*args, **kwargs)
    return wrapper_do_twice
```
```python
@do_twice
def return_greeting(name):
    print("Creating greeting")
    return f"Hi {name}"
```
```python
>>> return_greeting("Adam")
Creating greeting
Creating greeting
'Hi Adam'
```

---

### Introspection
```python
>>> print
<built-in function print>

>>> print.__name__
'print'
```
```python
>>> say_whee
<function do_twice.<locals>.wrapper_do_twice
at 0x7f43700e52f0>

>>> say_whee.__name__
'wrapper_do_twice'
```

---

####  @functools.wraps decorator
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

### Decorator template
```python
import functools

def decorator(func):
    @functools.wraps(func)
    def wrapper_decorator(*args, **kwargs):
        # Do something before
        value = func(*args, **kwargs)
        # Do something after
        return value
    return wrapper_decorator
```

---

### Decorators with arguments
```python
def repeat(num_times):
    def decorator_repeat(func):
        @functools.wraps(func)
        def wrapper_repeat(*args, **kwargs):
            for _ in range(num_times):
                value = func(*args, **kwargs)
            return value
        return wrapper_repeat
    return decorator_repeat
```
```python
@repeat(num_times=3)
def greet(name):
    print(f"Hello {name}")
```
```python
>>> greet("World")
Hello World
Hello World
Hello World
```

---

#### Decorators with or without arguments
```python
def repeat(_func=None, *, num_times=2):
    def decorator_repeat(func):
        @functools.wraps(func)
        def wrapper_repeat(*args, **kwargs):
            for _ in range(num_times):
                value = func(*args, **kwargs)
            return value
        return wrapper_repeat

    if _func is None:
        return decorator_repeat
    else:
        return decorator_repeat(_func)
```

---

```python
@repeat
def say_whee():
    print("Whee!")

@repeat(num_times=3)
def greet(name):
    print(f"Hello {name}")
```
```python
>>> say_whee()
Whee!
Whee!

>>> greet("Penny")
Hello Penny
Hello Penny
Hello Penny
```

---

### Stateful Decorators
```python
import functools

def count_calls(func):
    @functools.wraps(func)
    def wrapper_count_calls(*args, **kwargs):
        wrapper_count_calls.num_calls += 1
        print(
            f"Call {wrapper_count_calls.num_calls}"
            " of {func.__name__!r}"
        )
        return func(*args, **kwargs)
    wrapper_count_calls.num_calls = 0
    return wrapper_count_calls
```
```python
@count_calls
def say_whee():
    print("Whee!")
```

---

<p style='text-align: justify; font-size: 20pt'>
The state - the number of calls to the function - is stored in the function attribute .num_calls on the wrapper function. Here is the effect of using it:
</p>

```python
>>> say_whee()
Call 1 of 'say_whee'
Whee!

>>> say_whee()
Call 2 of 'say_whee'
Whee!

>>> say_whee.num_calls
2
```

---

### @ The end

---

---

### Classes as Decorators
```python
import functools

class CountCalls:
    def __init__(self, func):
        functools.update_wrapper(self, func)
        self.func = func
        self.num_calls = 0

    def __call__(self, *args, **kwargs):
        self.num_calls += 1
        print(
            f"Call {self.num_calls}"
            " of {self.func.__name__!r}")
        return self.func(*args, **kwargs)
```
```python
@CountCalls
def say_whee():
    print("Whee!")
```

---

### Creating Singletons
```python
import functools

def singleton(cls):
    """Make a class a Singleton class (only one instance)"""
    @functools.wraps(cls)
    def wrapper_singleton(*args, **kwargs):
        if not wrapper_singleton.instance:
            wrapper_singleton.instance = cls(*args, **kwargs)
        return wrapper_singleton.instance
    wrapper_singleton.instance = None
    return wrapper_singleton

@singleton
class TheOne:
    pass
```

---

```python
>>> first_one = TheOne()
>>> another_one = TheOne()

>>> id(first_one)
140094218762280

>>> id(another_one)
140094218762280

>>> first_one is another_one
True
```

---

### Thank you
