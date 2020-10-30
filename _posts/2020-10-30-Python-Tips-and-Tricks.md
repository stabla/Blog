---
layout: post
title: "Python Tips & Tricks"
description: "Python Tips & Tricks"
date: 2020-10-29
tags: [article, tips, tricks, python, blog]
comments: false
share: true
---


# Python 3 Tips & Tricks

Here's a succinct list of tips and tricks in Python.  

Python is a powerful and simple language. There's some tricks that exists that we should use instead of reinventing the wheel.

<img src="https://cdn.dribbble.com/users/74401/screenshots/3022594/d-snake1.png" style=" display: block; margin: 0 auto; width: 70%;" alt="Python Tips and Tricks">

_________________

1.  **Understand your scope**
As you should know, scoping in Python is respecting the LEGB rule, which is abbreviation for **L**ocal, **E**nclosing, **G**lobal, Built-in

To make an assignment to a variable in a scope, it has to become local. Therefore, you won't be able to write a variable if it isn't global 

```python
a = 2

def f():
    global a 
    # global make possible to inner scope to access variable in global scope
    a += 2
    print(a)
    # 4

f()
```

Note bene: using _nonlocal_ keyboard instead of _global_ would work as well.

2.  **Enum (Python 3.4+)**
Instead of over-using constants, you use Enum class.

```python
from enum import Enu

class Color(Enum):
    RED = 1,
    GREEN = 2,
    BLUE = 3

print(repr(Color.RED))
# <Color.RED: (1,)>

print(Color.GREEN)
# Color.GREEN
```

3. **Data Classes (Python 3.7+)**

DRY (Don't repeat yourself). Use data classes, Python will generate spcial methods like __init__ and __repr__ reducing repetition in your code.

```python
from dataclasses import dataclass

# Compare this class, using the conventional implementation
class Rectangle1:
    def __init__(
        self,
        color: str,
        width: float,
        height: float) -> None:

        self.color = color
        self.width = width
        self.height = height

    def area(self) -> float:
        return self.width * self.height

# with this class, using the @dataclass decorator
#dataclass
class Rectangle2:
    color: str
    width: float
    height: float

    def area(self) -> float:
        return self.width * self.height

# we can use borth instances the same way
rectangle1 = Rectangle1("Blue", 2, 3)
rectangle2 = Rectangle2("Blue, 2, 3)

```
   
4.  **Pathlib (Python 3.4+)**
   
The module pathlib is a clean way to intereact with the file system, far better than os.path or the glob module

```python

from pathlib import Path

# This is a folder
path = Path('threads')
print(path)

# Let's make ou rpath a litle bit more complexe
path = path / 'sub' / 'sub-sub'
print(path)
# threads/sub/sub-sub
```

5.  **Type Hints (Python 3.5+)**
   
Typing is not necessary in Python, but it is useful for develpers after you, or you in few years. It makes your code cleaner and more understable.


```python
    class House:
        def __init__(self, width: float, height: float) -> None:
            self.width = width
            self.height = height

        def area(self) -> float:
            return self.width * self.height 
```

6.  **f-strings (3.6+)**

Don't use .fomrat() to print your strings, f-strings is much more convinient and easier to maintain.

```python

x = 2
y = 3

# Compare the statement
print("x = {} and y = {}.".format(x,y))
# x = 2 and y = 3

# with the f-strings
print(f"x = {x} and y = {y}.")
# x = 2 and y = 3

```

7.  **Extendede iterable unpacking (Python 3.0+)**

Using this trick, while unpacking an iterable, you can specify a "catch-all" variable that will be assigned a list of the items not assigned to a regular variable.

```python
items = [1, 2, 3, 4, 5]
head, *body, tail = items

print(head, body, tail)
# 1, [2, 3, 4], 5

```

8. **Async IO 5Python 3.4+**

The asyncio is a good module to write asynchronous code. 
```python
import asyncio

async def say():
    print("Hello")
    await asyncio.sleep(2)
    print("World")

async def hello():
    await asyncio.gathe(say(), say())

asyncio.un(hello())
# Hello
# Hello
# World
# World
```

9.  **Underrscoores in Numeric Literals (Python 3.6+)**

```python

x = 1_000_000
y = 1000000
printf(x, y, x == y)
# 1000000 1000000 True

```

10. **Swapping Two Variables**

First idea when you need to swipe variables is to use a temporary variable, and blabla. But there's a better method without use a temporary variable, as such:

```python
a, b = 50, 60
a = a + b # a = 110
b = a - b # b = 50
a = a - b # a = 60
```

Cool, heh? But there's even something cooler.

```python
a, b = 50, 60
print(a, b)
a, b = b, a
# a = 60, b = 50
```

11. **Reversing a string**
   
In addition of swapping quickly a variable you need (not often, ok) to reverse a string, and it can be done extremely quickly.

```python
my_string = "TOR"
rev_string = my_string[::-1]
# ROT
```

12. **Splitting words in a line**

Of course, the old method of using .split() is a good way to do it.

```python
string = "This is a sentence"
splitted = string.split(' ')
# ['This', 'is', 'a', 'sentence']
```

13. **List of words into a line**
   
Imagine you have ['This', 'is', 'a', 'sentence'], what to do?

Use the ```python "".join(string)```

14. **Joining two string using addition operator**

```python
a = " I think "
b = "Python is great"
print(a+b)
# I think Python is great
```

15. **More than one conditionnal operators**

In contrario of thers programming langage, using mathematically borned notation is accepted in Python :)   

```python
if ( 1 < a < 20):
```

16. **Find most frequent element in a list**

```python
    list = [1, 2, 3, 2, 2, 1, 1]
    frequent = max(set(list), key=list.count)
    # 1
```

17. **Count occurence of elements in a list**

```python
    from collections import Counter
    print(Counter(list))
```

18. **Repeating the element multiple times**

```python
my_list = [3]
my_list = my_list*5
# [3, 3, 3, 3, 3]
```

19. **Using Ternary Operator**

```python
print("Eligible") if age>20 else print("Not Eligible")
# Eligible
```

20. **Rounding with Floor and Ceil**
   
```python
import math
my_number = 18.7
print(math.floor(my_number))
print(math.ceil(my_number))
```

21. **Function in one line**

```python
x = lambda a,b,c : a+b+c
print(x(10, 20, 30))
# 60
```

22. **Apply function for each element in a list**
   
```python
l = ["a", "b"]
l = map(str.capitalize,l)
print(list(l))
# ['a', 'b']
```

23. **Lambda functon on each element in a list**

```python
l = [1, 2, 3, 4, 5]
nl = map(lambda x: x*x, l)
print(list(nl))
# [1, 4, 9, 16, 25]
```

24. **Return multiple values from a function**
```python
def function(n):
    return 1,2,3,4
a,b,c,d = function(5)
print(a,b,c,d)
# 1 2 3 4
```

25. **Filtering the values using filter function**

```python
def eligibility(age):
    return age>=24
list_of_age = [10, 24, 27, 33, 30, 18, 17, 21, 26, 25]
age = filter(eligibility, list_of_age)print(list(age))
# [24, 27, 33, 30, 26, 25]
```

26. **Merging two dictionnaries**

```python
d1 = {'One':1, 'Two':2}
d2 = {'Two':2, 'Three':3}
dicti = {**d1, **d2}
print(dicti)
# {'One': 1, 'Two': 2, 'Three': 3}
```

27. **Getting size of an object**

```python
import sys
a = 5
print(sys.getsizeof(a))
# 28
```

28. **Two lists into diictionnary**
   
```python
l1 = ["One","Two","Three"]
l2 = [1,2,3]
dicti = dict(zip(1, l2))
print(dicti)
# {'Two': 2, 'One': 1, 'Three': 3}
```

29. **Calculating execution time for a program**

```python
import time
start = time.clock()
for x in range(1000):
    pass
end = time.clock()
total = end - start
print(total)
# 0.00011900000000000105
```

30. **Removing duplicate elements in list**
   
```python
l = [1,4,1,8,2,8,4,5]
l = list(set(l))
print(l)
# [8, 1, 2, 4, 5]
```

31. **Multiple assignment per variables**

```python
a, b = 50, 60
# a = 50, b = 60
a, *b = 50, 60, 70
# 50, [60, 70]
a = b = c = 50
# a = 50, b = 50, c = 50
```
_________________

Python is ❤️
