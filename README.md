# py2hs

## Introduction
I find it an interesting practice to learn and reinforce a new programming language by comparing it with 
a familiar language, and adding the distinct features and concepts from the new language on top of my existing knowledge system.
This is a reference guide I created when I was learning Haskell.

This project is inspired by [py2rs](https://github.com/rochacbruno/py2rs). 

*This is:* 

- a complementary resource that helps python programmers to learn Haskell as a new language
- a place for beginners to learn and reinforce some of the core concepts in Haskell

*This is __not__:*

- an one-stop-shop that teaches *everything* about Haskell
- an encyclopedia that aims at covering *all* similarities and differences between Python and Haskell

## Table of contents
  * [General](#general)
     * [Getting started with Haskell](#getting-started-with-haskell)
  * [Show me the code](#show-me-the-code)
     * [Hello World](#hello-world)
     * [Functions](#functions)
        * [Basic function definition](#basic-function-definition)
        * [Flow control](#flow-control)
        * [Explicit typing](#explicit-typing)
        * [Lambda function](#lambda-function)
     * [List](#list)
        * [List creation](#list-creation)
        * [List concatenation](#list-concatenation)
        * [Adding element to list](#adding-element-to-list)
        * [Indexing](#indexing)
        * [List comprehension](#list-comprehension)
        * [Zipping](#zipping)
     * [Class -&gt; data](#class---data)
     * [Interface -&gt; Type class](#interface---type-class)
     * [Higher order functions](#higher-order-functions)
        * [Map](#map)
        * [Filter](#filter)
        * [Reduce](#reduce)
        * [Partial functions](#partial-functions)


## General

| Definition      | Python      | Haskell  |
| -----------    | ----------- | -------- |
| [Programming paradigm](https://en.wikipedia.org/wiki/Programming_paradigm)   | [Imperative](https://en.wikipedia.org/wiki/Imperative_programming)  | [Purely functional](https://en.wikipedia.org/wiki/Purely_functional_programming) |
| [Type System](https://en.wikipedia.org/wiki/Type_system)      | Dynamically typed | Statically typed|
| First appeared | 1989 | 1990 |
| File extensions | .py, .pyw, .pyc | .hs, .lhs | 
| Programming guidelines | [Haskell programming guidelines](https://wiki.haskell.org/Programming_guidelines#Let_or_where_expressions) | [PEP8](https://www.python.org/dev/peps/pep-0008/)

### Getting started with Haskell
Official Haskell documentation provides plenty of tutorials and books to start with.

https://www.haskell.org/documentation/

## Show me the code

All examples are either code snippet or interactions with python shell or [Haskell GHCi](https://wiki.haskell.org/GHC/GHCi). 
Python shell examples starts with `>>> `, and Haskell GHCi examples starts with `λ> `. Please note that all 
Python codes are written in [Python 3](https://docs.python.org/3/).

---------------------------

### Hello World

Python

```python
>>> print("Hello world")
Hello world
```

Haskell

```haskell
λ> print "Hello world"
"Hello world"
```

---------------------------

### Functions

#### Basic function definition
Python

```python
>>> def multiply(x, y):
...     return x * y
...
>>> multiply(2, 3)
6
```

Haskell

```haskell
λ> multiply x y = x * y
λ> multiply 2 3
6
```

#### Flow control
Python

```python
def bigger(x, y):
    if x > y:
        return x
    else:
        return y
```

Haskell

```haskell
bigger x y = if x > y
             then x
             else y
```

#### Explicit typing
Python

```python
def bigger(x: int, y: int) -> int:
    if x > y:
        return x
    else:
        return y
```

Haskell

##### Approach 1: explicitly specify all inputs and the output

```haskell
bigger :: Int -> Int -> Int
bigger x y = if x > y
             then x
             else y
```

##### Approach 2: set a type constraint `Int` for `a` 
```haskell
bigger :: (Int a) => a -> a -> a 
bigger x y = if x > y
             then x
             else y
```

#### Lambda function
Python

```python
>>> list(map(lambda x: x + 1, [1, 2, 3]))
[2, 3, 4]
```

Haskell

```haskell
λ> map (\x -> x + 1) [1, 2, 3]
[2,3,4]
```

---------------------------

### List


#### List creation

Python

```python
>>> [1, 2, 3, 4]
[1, 2, 3, 4]
>>> list(range(1, 5)) # Use range
[1, 2, 3, 4]
```

Haskell

Syntax sugar:

```haskell
λ> [1, 2, 3, 4]
[1,2,3,4]
λ> [1..4] -- Use range
[1,2,3,4]
```

Without syntax sugar:

```haskell
λ> 1:2:3:4:[]
[1,2,3,4]
```

#### List concatenation

Python

```python
>>> [1, 2] + [3, 4]
[1, 2, 3, 4]
```

Haskell
```haskell
λ> [1, 2] ++ [3, 4]
[1,2,3,4]
``` 

#### Adding element to list

##### Append

Python

```python
>>> my_list = [1, 2, 3, 4]
>>> my_list.append(5)
>>> my_list
[1, 2, 3, 4, 5]
```

Haskell
```haskell
λ> [1, 2, 3, 4] ++ [5]
[1,2,3,4,5]
``` 

##### Prepend

Python

```python
>>> my_list = [1, 2, 3, 4]
>>> my_list.insert(0, 5)
>>> my_list
[5, 1, 2, 3, 4]
```

Haskell
```haskell
λ> 5:[1, 2, 3, 4]
[5,1,2,3,4]
``` 

#### Indexing

Python

```python
>>> my_list = [1, 2, 3, 4]
>>> my_list[2]
3
```

Haskell
```haskell
λ> let myList = [1, 2, 3, 4]
λ> myList !! 2
3
```

#### List comprehension

Python

```python
>>> [i * 2 for i in range(1, 11)]
[2, 4, 6, 8, 10, 12, 14, 16, 18, 20]
```

Haskell

```haskell
λ> [x*2 | x <- [1..10]]
[2,4,6,8,10,12,14,16,18,20]
```

##### List comprehension with conditions

Python

```python
>>> [i * 2 for i in range(1, 11) if i % 2 == 0]
[4, 8, 12, 16, 20]
```

Haskell

```haskell
λ> [x*2 | x <- [1..10], even x]
[4,8,12,16,20]
```

##### More complex list comprehension

Python

```python
>>> [i * j for i in range(1, 5) for j in range(1, 5)]
[1, 2, 3, 4, 2, 4, 6, 8, 3, 6, 9, 12, 4, 8, 12, 16]

>>> [[a, b, c] for c in range(1, 11)
...     for b in range(1, c+1)
...     for a in range(1, b+1) if a**2 + b**2 == c**2]
[[3, 4, 5], [6, 8, 10]]

```

Haskell

```haskell
λ> [ x*y | x <- [1..4], y <- [1..4]]
[1,2,3,4,2,4,6,8,3,6,9,12,4,8,12,16]

λ> [ [a,b,c] | c <- [1..10], b <- [1..c], a <- [1..b], a^2 + b^2 == c^2]
[[3,4,5],[6,8,10]]
```

#### Zipping

Python

```python
>>> list(zip(range(1,5), range(2,6)))
[(1, 2), (2, 3), (3, 4), (4, 5)]

>>> list(zip(range(1, 5), [8, 9, 10]))
[(1, 8), (2, 9), (3, 10)]
```

Haskell

```haskell
λ> zip [1..4] [2..5]
[(1,2),(2,3),(3,4),(4,5)]

λ> zip [1..4] [8,9,10]
[(1,8),(2,9),(3,10)]
```

---------------------------

### Class -> data 

In Python, and most OOP languages, Classes provide a means that bundles two things:
- data
- functions that retrieve, modify, or manipulate the data

Let's start with defining a class in Python.

```python
from dataclasses import dataclass

@dataclass
class Square:
    side: float

    def area(self):
        return self.side ** 2
```

```python
>>> my_square = Square(5)
>>> my_square.area()
25
>>> my_square.side = 6
>>> my_square.area()
36
```

Class `Square` bundles a float number, `side`, and a function, `area`, which calculates the area of the square. 
Attribute `side` could be directly updated. The functions bundled with the class are stateful, because 
the outputs of the same function could be different depending on the state of the class. Being stateful is like holding 
a double-edge sword. On one hand, you can make the a class very flexible and adaptive to changes. On the other 
hand, however, the behavior of functions becomes non-deterministic with respect to their inputs, making it sometimes 
unpredictable and difficult to debug. 

Now let's see a way to define a Square and a function that calculates any Square's area in Haskell.

`data` is a keyword in Haskell. It defines a type of data structure, similar to `Class` in Python.

```haskell
data Square = Square Float -- Defining "Square" as a data structure that holds a Float

area :: Square -> Float -- Declaring function "area" as a function that takes a "Square" and returns a Float
area (Square side) = side ^ 2 -- Implement the area function
```

```haskell
λ> let mySquare = Square 5
λ> area mySquare
25.0
```

What we saw above, is a type (`Square`) that holds a float, and a function (`area`) that operates on this specific
type, while not being bundled to the data structure itself. The separation of data and function, is one of the 
most important characteristics of Haskell. 
What's good about the separation of data and function? Stateless! Instead of depending on some "internal"
state, functions are deterministic and predictable purely from their inputs.

---------------------------

### Interface -> Type class


Area does not only apply to square, but all types of shapes. Let's start with defining an interface, `area`, in Python.

```python
from abc import ABC, abstractmethod

class Shape(ABC):

    @abstractmethod
    def area(self):
        pass
```

Then we define a couple of shapes that implements `area`.

```python
import math
from dataclasses import dataclass

@dataclass
class Square(Shape):
    side: float

    def area(self):
        return self.side ** 2

@dataclass
class Rectangle(Shape):
    height: float
    width: float

    def area(self):
        return self.height * self.width

@dataclass
class Circle(Shape):
    radius: float

    def area(self):
        return math.pi * self.radius ** 2
```

```python
>>> my_shapes = [Square(5), Rectangle(5, 6), Circle(3)]
>>> [shape.area() for shape in my_shapes]
[25, 30, 28.274333882308138]
```

How do to do so in Haskell?

In Haskell, data can define more than one constructor.

```haskell
data Shape = Square Float | Rectangle Float Float | Circle Float
```


Here is one way to write our area function in Haskell:

```haskell
area :: Shape -> Float
area (Square side) = side ^ 2
area (Rectangle height width) = height * width
area (Circle radius) = pi * radius ^ 2
```

```haskell
λ> map area [Square 5, Rectangle 5 6, Circle 3]
[25.0,30.0,28.274334]
```


You might already observe a problem here: whenever a new type of "Shape" is created, we will need to add that type to  
the implementation of "area". In other words, "area" is tightly coupled with "Shape"
For instance, say we create a new type that represents continents and we want them to support calculation of 
their areas. If we continue to use the same area function, things become ugly.

First, we need to add more types to shape:

```haskell
data Shape = Square Float
    | Rectangle Float Float
    | Circle Float
    | Africa
    | Antarctica
```

Then, we need to modify area function as well

```haskell
area :: Shape -> Float
area (Square side) = side ^ 2
area (Rectangle height width) = height * width
area (Circle radius) = pi * radius ^ 2
area (Africa) = 1.117e7 -- in the unit of square miles
area (Antarctica) = 5.483e6
```

In other words, the implementation of `area` is coupled with `Shape`. 


Here is where type classes become handy. Type class is similar to the concept of interface in 
object-oriented programming. Let's try to define our "interface" with a type class.


```haskell
class MyObject a where
    area :: a -> Float
```

What we are are seeing, is a type class, `MyObject`, that declares a behavior (or function), `area`, 
which returns a Float when a type of `MyObject` is given.

Now, we can define Shapes and Continents separately and make them support area calculation at the same time.

##### Define shapes 

```haskell
data Shape = Square Float | Rectangle Float Float
instance MyObject Shape where
    area (Square side) = side ^ 2
    area (Rectangle height width) = height * width

```

```haskell
λ> let a = Square 4
λ> area a
16.0
λ> let b = Rectangle 4 5
λ> area b
20.0
```

##### Define continents

```haskell
data Continent = Africa | Antarctica
instance MyObject Continent where
    area Africa = 1.117e7
    area Antarctica = 5.483e6
```

```haskell
λ> area Africa
1.117e7
λ> area Antarctica
5483000.0
```

The codes of Shape and Continent are completely independent, but we can still apply the same function (`area`) on them. 

This concept is actually nothing new in Python. Remember [data model](https://docs.python.org/3/reference/datamodel.html) 
in python? For example, a class can implement  `__eq__` so we can apply `==` operator to it:


```python
class Rectangle:
    def __init__(self, height, width):
        self.height = height
        self.width = width

    def __eq__(self, other):
        if isinstance(other, Rectangle):
            return self.height == other.height and self.width == other.width
```

```python
>>> Rectangle(5, 6) == Rectangle(5, 6)
True
>>> Rectangle(5, 6) == Rectangle(6, 7)
False
```

Unsurprisingly, `==` belongs to type class `Eq` in Haskell.

This is the definition of `Eq`:

```haskell
class Eq a where
    (==) :: a -> a -> Bool
    (/=) :: a -> a -> Bool
    x == y = not (x /= y)  
    x /= y = not (x == y)   
```

Let's try implement `Eq` for our rectangle.

```haskell
data Rectangle = Rectangle Float Float
instance Eq Rectangle where
    Rectangle a b == Rectangle x y = a == x && b == y
```

```haskell
λ> Rectangle 5 6 == Rectangle 5 6
True
λ> Rectangle 5 6 == Rectangle 6 7
False
λ> Rectangle 5 6 /= Rectangle 6 7
True
```

Notice that we only need to implement either `==` or `/=`, and the other operator will be automatically usable. 
This is achieved by the default implementation of `Eq`, where one is the negation of the other:

```haskell
    x == y = not (x /= y)
    x /= y = not (x == y)
``` 

---------------------------

### Higher order functions

Higher order functions are functions that takes functions as inputs or return functions as outputs.
Let's start with some of the most common higher order functions: map, filter, and reduce.

#### Map
Python

```python
>>> list(map(lambda x: x*2, range(1, 5)))
[2, 4, 6, 8]
```

Haskell

```haskell
λ> map (*2) [1..4]
[2,4,6,8]
```

#### Filter
Python
```python
>>> list(filter(lambda x: x >= 3, range(1, 5)))
[3, 4]
```


Haskell
```haskell
λ> filter (>=3) [1..4]
[3,4]
```

#### Reduce
Python
```python
>>> from functools import reduce
>>> reduce(lambda x, a: x + a, range(1,5), 100)
110
```

Reduce is called "fold" in Haskell
```haskell
λ> foldl (+) 100 [1..4]
110
```

#### Partial functions

In Python, we can create a function that takes a function and return a new function that add things 
on top of it. For example, we can make a "partial" function that apply arguments partially to another 
function.

```python
def add(x, y):
    return x + y

def multiply(x, y):
    return x * y

def partial(func, *partial_args):
    def apply(*remaining_args):
        return func(*partial_args,  *remaining_args)
    return apply
```

Now, we can use a combination of `partial` and another function to construct new functions. 

```python
>>> increment = partial(add, 1)
>>> increment(2)
3
>>> increment(3)
4
>>> double = partial(multiply, 2)
>>> double(2)
4
>>> double(3)
6
```

In fact, Python standard library provides `partial` as a utility function in module `functools`.

```python
from functools import partial
>>> from functools import partial
>>> increment = partial(add, 1)
>>> increment(3)
4
>>> double = partial(multiply, 2)
>>> double(2)
4
```

Can we do something similar in Haskell? Yes, and even simpler! 

```haskell
λ> increment = (+) 1
λ> increment 3
4
λ> increment 4
5
λ> double = (*) 2
λ> double 2
4
λ> double 3
6
```

In fact, all functions could be partially applied in Haskell by default. In Haskell, we call partially 
applied functions as "curried functions", named after Haskell Curry.

