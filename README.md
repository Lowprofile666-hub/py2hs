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
 * [Functor](#functor)
 * [Applicative](#applicative)
 * [Monoid](#monoid)


## General

| Definition      | Python      | Haskell  |
| -----------    | ----------- | -------- |
| [Programming paradigm](https://en.wikipedia.org/wiki/Programming_paradigm)   | [Imperative](https://en.wikipedia.org/wiki/Imperative_programming)  | [Purely functional](https://en.wikipedia.org/wiki/Purely_functional_programming) |
| [Type System](https://en.wikipedia.org/wiki/Type_system)      | Dynamically typed | Statically typed|
| First appeared | 1989 | 1990 |
| File extensions | .py, .pyw, .pyc | .hs, .lhs | 
| Programming guidelines | [PEP8](https://www.python.org/dev/peps/pep-0008/) | [Haskell programming guidelines](https://wiki.haskell.org/Programming_guidelines#Let_or_where_expressions) |

### Getting started with Haskell
Official Haskell documentation provides plenty of tutorials and books to start with.

https://www.haskell.org/documentation/

### Code format

All examples are either code snippet or interactions with python shell or [Haskell GHCi](https://wiki.haskell.org/GHC/GHCi). 
Python shell examples starts with `>>> `, and Haskell GHCi examples starts with `λ> `. Please note that all 
Python codes are written in [Python 3](https://docs.python.org/3/).

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
a double-edge sword. On one hand, you can make the a class very flexible and adaptive to new changes. On the other 
hand, however, the behavior of functions is non-deterministic with respect to their inputs, making it 
unpredictable, and therefore, difficult to debug. 

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
the implementation of "area". In other words, "area" is tightly coupled with "Shape".

For instance, if we create a new type that represents continents and we want them to support calculation of 
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
object-oriented programming. Let's try to define our "interface" using a type class.


```haskell
class MyObject a where
    area :: a -> Float
```

What we are are seeing, is a type class, `MyObject`, that declares a behavior (or interface), `area`, 
which returns a Float when a type of `MyObject` is given. Notice that the `class` here is not the same as the 
"class" in Python.

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

This concept is actually nothing new in Python. 
Remember [data model](https://docs.python.org/3/reference/datamodel.html)? Yes, type class is a more power version of 
data model.

In python, a class can implement  `__eq__` so we can apply `==` operator to it:


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

In Python, a function can operate on another function. 
For example, we can make a "partial" function that apply arguments partially to another function.

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
applied functions as "curried functions", named after [Haskell Curry](https://en.wikipedia.org/wiki/Haskell_Curry).

---------------------------

### Functor

In Python, [`map`](https://docs.python.org/3/library/functions.html#map) is very useful when we want to transform 
an iterable.

For example, we can transform a list of integers to a list of strings with `map`.

```python
>>> list(map(str, [1, 2, 3, 4, 5]))
['1', '2', '3', '4', '5']
```

Here, list is a "container" that holds the elements, each of which is mapped to a new element.

A limitation of `map` is that the container has to be an iterable. 


Say we have a tree structure like below:

```python
class Tree:
    def __init__(self, left=None, right=None, val=0):
        self.left = left
        self.right = right
        self.val = val

    def __repr__(self):
        return f'Tree(val={self.val}, left={self.left}, right={self.right})'
```


```python
>>> my_tree = Tree(left=Tree(val=1), right=Tree(val=2))
>>> my_tree
Tree(val=0, left=Tree(val=1, left=None, right=None), right=Tree(val=2, left=None, right=None))

```

Clearly, `map` can't transform the values in this tree. In this case, we need to implement out own `map`.

```python
def map_tree(my_func, tree):
    if tree:
        return Tree(left=map_tree(my_func, tree.left), 
                    right=map_tree(my_func, tree.right),
                    val=my_func(tree.val))
``` 

```python
>>> map_tree(lambda x: x*2, my_tree)
Tree(val=0, left=Tree(val=2, left=None, right=None), right=Tree(val=4, left=None, right=None))

>>> map_tree(lambda x: 'transformed!', my_tree)
Tree(val=transformed!, left=Tree(val=transformed!, left=None, right=None), right=Tree(val=transformed!, left=None, right=None))
```

Now, we have a default `map` function from Python standard library, and we also have our own implementation of 
`map_tree`, implemented for `Tree` structure. 
Unfortunately, there is not a straightforward way to unify these two functions in Python. By 
unify, I meant a universal `map` interface that apply both on list, Tree, and essentially any types.

Thanks to the robustness of type class, this universal interface is achievable in Haskell, and it is provided as part of 
Haskell's standard library. This interface is called "Functor".

Here is its definition:

```haskell
class Functor f where
    fmap :: (a -> b) -> f a -> f b
```

`f` is a type of container that "contains" the data. It could be a list, a Tree, or anything that holds values. 
`(a -> b)` means a function that transform a value of type `a` to a value of type `b`. `f a` is a concrete type that 
means container `f` is holding values of type `b`. `f b` means container `f` is holding values of type `b`.

All together, the definition of Functor in plain words, is any types that implements a function `fmap`, that, 
given a function (`(a -> b)`) and a container `f` that holds values of type `a`, returns a container `f` that holds 
values of type `b`. `fmap` is the universal map function we've been looking for.

Let's look at the implementation of Functor for list.

```haskell
instance Functor [] where  
    fmap = map  
``` 
 
It is really straightforward.The Functor implementation of list is simply `map`. 

```haskell
λ> fmap (*2) [1,2,3]
[2,4,6]

``` 

Now, let's create a Tree type and see how Functor works. 

```haskell
λ> data Tree a = EmptyTree | Node a (Tree a) (Tree a) deriving (Show, Read, Eq)
λ> tree = Node 0 (Node 1 EmptyTree EmptyTree) (Node 2 EmptyTree EmptyTree)
λ> tree
Node 0 (Node 1 EmptyTree EmptyTree) (Node 2 EmptyTree EmptyTree)
```

Implementing `fmap` for `Tree`:

```haskell
instance Functor Tree where  
    fmap f EmptyTree = EmptyTree  
    fmap f (Node x left right) = Node (f x) (fmap f left) (fmap f right)  
```

```haskell
λ> fmap (*2) tree
Node 0 (Node 2 EmptyTree EmptyTree) (Node 4 EmptyTree EmptyTree)
λ> fmap (\x -> "Transformed!") tree
Node "Transformed!" (Node "Transformed!" EmptyTree EmptyTree) (Node "Transformed!" EmptyTree EmptyTree)
```

We can see that `fmap` could be applied to any type of "container" as long as it is an instance of Functor.

---------------------------

### Applicative

Applicative is a more powerful type of Functor.

In the section of Functor, we learn how to apply a function to a Tree with `fmap`.

What if we have two trees and we want to overlay the two to generate a new Tree? 

```
  Tree 1          Tree 2              New Tree

    1       +       5       --->         6                
   / \             /                    /
  2   3           6                    8
```

What if we want to apply three trees to a function?
```
 
   Tree 1          Tree 2         Tree 2              New Tree
 _                        _
|    1               5     |        5                    30                
|   / \      +      /      |   *   /        --->        /
|  2   3           6       |      6                   48
 -                        - 
```

First, let's see how to implement this in python.

We can't use `map_tree` we created previously, because it only operates on one tree. We can create a 
functions that can operates on any number of trees:

```python
def map_trees(trees, func):
    if all(trees):
        return Tree(val=func(*[tree.val for tree in trees]), 
                    left=map_trees([tree.left for tree in trees], func), 
                    right=map_trees([tree.right for tree in trees], func))
    else:
        return None
```

```python
>>> tree1 = Tree(val=1, left=Tree(val=2), right=Tree(val=3))
>>> tree2 = Tree(val=5, left=Tree(val=6))
>>> map_trees([tree1, tree2], add)
Tree(val=6, left=Tree(val=8, left=None, right=None), right=None)
>>> map_trees([tree1, tree2], pow)
Tree(val=1, left=Tree(val=64, left=None, right=None), right=None)
>>> map_trees([tree1, tree2, tree2], lambda a, b, c: (a + b) * c)
Tree(val=30, left=Tree(val=48, left=None, right=None), right=None)
```

Intuitively, `map_trees` is a more powerful version of `map_tree`, in a way that it accepts any 
number of trees as inputs. Similarly, applicative is a more powerful version of functor, in a way that 
it could be passed to a function that has more than one input. In Haskell, applicative is defined as below:

```haskell
class (Functor f) => Applicative f where  
    pure :: a -> f a  
    (<*>) :: f (a -> b) -> f a -> f b  
```

First, the type constraint tells us that applicative has to be a Functor in the first place. Second, applicative has two 
functions. `pure` is basically wrapping a value in a functor (or "container"). `<*>` is similar to `fmap`. A 
quick reminder of `fmap`:

```haskell
fmap :: Functor f => (a -> b) -> f a -> f b
```
The only difference between `fmap` and `<*>` lies in their first argument, where functor is looking for a function, 
`(a -> b)`, while applicative is looking for a function that is wrapped in a functor, `f (a -> b)`.

Seems weird? Let's look how we can implement `map_trees` with applicative in Haskell.

```haskell
instance Applicative Tree where
    pure x = Node x (pure x) (pure x)
    Node a aLeft aRight <*> Node b bLeft bRight = Node (a b) (aLeft <*> bLeft) (aRight <*> bRight) 
    _ <*> _ = EmptyTree
```

What `pure` does is simply placing a value inside a container (a tree in the case above).
For `<*>`, the values of two Trees are combined by applying `b` to `a` recursively when both inputs are not empty.
In the rest of cases, simply return an `EmptyTree`.

```haskell
λ> tree1 = Node 1 (Node 2 EmptyTree EmptyTree) (Node 3 EmptyTree EmptyTree)
λ> tree2 = Node 5 (Node 6 EmptyTree EmptyTree) EmptyTree
λ> pure (+) <*> tree1 <*> tree2
Node 6 (Node 8 EmptyTree EmptyTree) EmptyTree
λ> my_func a b c = (a + b) * c
λ> pure my_func <*> tree1 <*> tree2 <*> tree2
Node 30 (Node 48 EmptyTree EmptyTree) EmptyTree
λ> my_func a b c d = a * b + c * d
λ> pure my_func <*> tree1 <*> tree2 <*> tree2 <*> tree2
Node 30 (Node 48 EmptyTree EmptyTree) EmptyTree
```

---------------------------

### Monoid

In the section of Applicative, we learnt how a series of functors could be passed as arguments to a regular function. 
Now, let's consider a similar but slightly different problem: 
how can we apply a function to reduce or fold a tree recursively?
e.g. calculating the sum or product of values in a tree. 


There are many of ways to implement this in python. Here is one way:

```python
def reduce_tree(tree, func, default=1):
    if tree:
        reduced_left = func(reduce_tree(tree.left, func, default=default), tree.val)
        reduced = func(reduced_left, reduce_tree(tree.right, func, default=default))
        return reduced
    else:
        return default
```

```python
>>> tree = Tree(val=1, left=Tree(val=2), right=Tree(val=4))
>>> reduce_tree(tree, lambda a,b: a+b, default=0)
7
>>> reduce_tree(tree, lambda a,b: a*b, default=1)
8
>>> reduce_tree(tree, min, default=float("inf"))
1
>>> reduce_tree(tree, max, default=-float("inf"))
4
```

We can also make the `Tree` iterable, so we can directly use `reduce` and make our code a bit more functional.

```python
class Tree:
    def __init__(self, left=None, right=None, val=0):
        self.left = left
        self.right = right
        self.val = val

    def __repr__(self):
        return f'Tree(val={self.val}, left={self.left}, right={self.right})'

    def __iter__(self):
        if self.left:
            yield from self.left
        yield self.val
        if self.right:
            yield from self.right
```

```python
>>> tree = Tree(val=1, left=Tree(val=2), right=Tree(val=4))
>>> list(tree)
[2, 1, 4]
>>> from functools import reduce
>>> reduce(lambda a,b: a+b, tree, 0)
7
>>> reduce(lambda a,b: a*b, tree, 1)
8
>>> reduce(min, tree, float("inf"))
1
>>> reduce(max, tree, -float("inf"))
4
```

There is an important assumption in implementations above. That is, all functions being applied to the tree has 
to be [associative](https://en.wikipedia.org/wiki/Associative_property). Associativity means that the order of function 
application doesn't determine the final output value. To define it more strictly, 
a binary function `f` is associative if the following condition satisfy for all `x`, `y`, `z` in S, 
where S is a set of values. 
 
```
f(x, f(y, z)) = f(f(x,y), z)
```

It is not hard to tell that `+`, `*`, `min`, and `max` are all associative. That's why we can apply them to a Tree 
without worrying about the order in which they are applied internally.

An example of non-associative function is average. e.g. `average(average(a, b), c)` is not equal to 
`average(a, average(b, c))`.
If we apply average to a tree, the final value will be different depending on whether left child or right child is 
reduced with the root first.

```python
def reduce_tree(tree, func, default=1):
    if tree:
        reduced_left = func(reduce_tree(tree.left, func, default=default), tree.val)
        reduced = func(reduced_left, reduce_tree(tree.right, func, default=default))
        return reduced
    else:
        return default

def reduce_tree2(tree, func, default=1):
    if tree:
        reduced_right = func(tree.val, reduce_tree2(tree.right, func, default=default))
        reduced = func(reduce_tree2(tree.left, func, default=default), reduced_right)
        return reduced
    else:
        return default
``` 

```python
>>> tree
Tree(val=1, left=Tree(val=2, left=None, right=None), right=Tree(val=4, left=None, right=None))
# Addition is not determined by the order in which values are applied
>>> reduce_tree(tree, lambda a,b: a+b, default=0)
7
>>> reduce_tree2(tree, lambda a,b: a+b, default=0)
7
# Average is determined by the order in which values are applied
>>> reduce_tree(tree, lambda a,b: (a+b)/2, default=0)
0.875
>>> reduce_tree2(tree, lambda a,b: (a+b)/2, default=0)
0.75
```

Why are we talking about associativity? Because it is the essence of monoids.
In Haskell, Monoid is a type class that has an associative binary function and a identity value, `A`, which, when applied 
the binary function along with any value, `B`, always yields `B` as the output.

For example, `0` is the identity value with respect to binary function `+`, and `1` is the identity value with respect 
to binary function `*`.

Here is the formal definition of Monoid type class:

```haskell
class Monoid m where  
    mempty :: m  
    mappend :: m -> m -> m  
    mconcat :: [m] -> m  
    mconcat = foldr mappend mempty  
```

`mempty` is the identity value, and `mappend` is the associative binary function.  
`mconcat` is a function that "concatenates" a list of monoids into one, and we get it for free with a default 
implementation.

Now, let's first see how to make a foldable tree in haskell *without* using monoids.

```haskell
import qualified Data.Foldable as F  

data Tree a = EmptyTree | Node a (Tree a) (Tree a) deriving (Show, Read, Eq)

instance F.Foldable Tree where
    foldr f z EmptyTree = z
    foldr f z (Node val left right) = f (f val rightResult) leftResult
	    where
	    rightResult = foldr f right z
	    leftResult = foldr f left z
```

The code seems fine in the first look. We pass the reduced results from right and left to function `f`, and we get the 
final reduced result . 
However, GHC complains!
```
    • Couldn't match expected type ‘a’ with actual type ‘b’

  |
7 |     foldr f z (Node val left right) = f (f val rightResult) leftResult
  |                                          ^^^^^^^^^^^^^^^^^
``` 
Well, it turns that function `f` (whose type is `a -> b -> b`) is expecting its first argument to be type `a`, but it got 
type `b`, which is the output of `f val rightResult`. In fact, we don't really have a way of turning type `b` into 
type `a`. We are stuck here.

Fortunately, `Foldable` provides a second interface, `foldMap`, which is easier to implement:
```haskell
foldMap :: (Monoid m, Foldable t) => (a -> m) -> t a -> m  
```

Now, instead of taking a function of type `a -> b -> b`, `foldMap` takes a function of type `a -> m`, which transforms 
a type, `a`, to a monoid, `m`.

Here is the implementation of foldMap for Tree:

```haskell
instance F.Foldable Tree where  
    foldMap f EmptyTree = mempty  
    foldMap f (Node x left right) = mconcat [foldMap f l, f x, foldMap f r]
```

Now we can see the advantage of using monoid here. 
`F.foldMap f left`, `f x`, and `F.foldMap f right` all return a monoid as outputs, which could be "concatenated" into  
a single monoid with `mconcat`. Now, we just need to find a function that turns a value of a Node to a monoid.

A simple way is to create a newtype as a monoid for numbers.  

```haskell
newtype Product a =  Product { getProduct :: a }  
    deriving (Eq, Ord, Read, Show, Bounded)  

instance Num a => Monoid (Product a) where  
    mempty = Product 1  
    Product x `mappend` Product y = Product (x * y)  
``` 

```haskell
λ> getProduct $ Product 3 `mappend` Product 4
12
```

Now, we can use `Product` as a monoid for foldMap.
```haskell
λ> tree = Node 2 (Node 1 EmptyTree EmptyTree) (Node 4 EmptyTree EmptyTree)
λ> getProduct $ foldMap Product tree
8
```

Not surprisingly, `Product` is defined as one of the monoids in standard module `Data.Monoid`, along with Sum, Min, Max, 
and many more.

```haskell
λ> getSum $ foldMap Sum tree
7
```
