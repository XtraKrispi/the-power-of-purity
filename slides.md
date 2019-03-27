---
title: The Power of Purity
theme: solarized
highlightTheme: obsidian
revealOptions:
  width: 100%
  height: 100%
  margin: 0
  minScale: 1
  maxScale: 1
---

# About me

<img src="images/goldie-pic.jpg" style="margin:0;background:0;border:0;box-shadow:none;" />
<ul>
  <li>Michael Gold, Lead Software Engineer Analytics</li>
  <li>Work out of Toronto, live in Vaughan, Ontario <div style="text-align: center"><img style="margin:0;background:0;border:0;box-shadow:none;height: 300px;" src="images/wonderland.jpg"></div></li>
</ul>
    
---

# About me

<ul>
  <li>Two kids, Don (4) and Myles (2), and wife Lisa
      <div style="text-align: center">
        <img style="margin:0;background:0;border:0;box-shadow:none;height: 300px;" src="images/don-pic.jpg"><img style="margin:0;background:0;border:0;box-shadow:none;height:300px;" src="images/myles-pic.jpg">
      </div>
  </li>
  <li>Avid board gamer, video gamer, technology lover</li>
  <li>Love functional programming including Haskell, Purescript, Elm, F#</li>
</ul>

---

# The Power of Purity

<img src="images/Haskell-Logo.svg" style="margin:0;background:0;border:0;box-shadow:none;" />
---

# What does this function do?

```cs
  public int Increment(int n)
```

----

# Think again

```cs
  public int Increment(int n) {
    var toReturn = n + globalValue;

    globalValue += 10;

    _someService.fireZeMissiles();

    return toReturn;
  }
```

---

# What is purity?

A pure function:
  * Does not alter state <!-- .element: class="fragment" -->
  * Performs no mutation <!-- .element: class="fragment" -->
  * Does no side effects <!-- .element: class="fragment" -->
  * Same input = Same output <!-- .element: class="fragment" -->
  
---
# Haskell

* Turing complete, general purpose, purely functional programming language <!-- .element: class="fragment" -->

* No side effects, every operation is pure <!-- .element: class="fragment" -->

* Completely immutable, once a value is set, it can't be changed <!-- .element: class="fragment" -->

**For the same input, Haskell will always return the same output** <!-- .element: class="fragment" -->

Note: Talk about how side effects are possible, just not in the way you think

----

# Haskell

## Some syntax

Function signatures are written like this:
```haskell
  add :: Int -> Int -> Int
```

<ul>
  <li class="fragment fade-in">
    **`::`** *has the type*
  </li>
  <li class="fragment fade-in">
    **`->`** Argument separator
  </li>
  <li class="fragment fade-in">
    Final data type is the return type of the function
  </li>
  <li class="fragment fade-in">
    Functions must have *lowercase* names, types must be *uppercase*
  </li>
</ul>
  
Note:  Explain the different parts of a function signature.

Also explain that most of the time function signatures are optional, but are useful documentation

----

# Haskell

## Back to our example

The Haskell version of the function we saw at the beginning looks like:

```haskell
  increment :: Int -> Int
```

<ul>
  <li class="fragment fade-in">
    Impossible to alter state, so it can only return an `Int` of some kind
  </li>
  <li class="fragment fade-in">
    The function is only able to operate on the `Int` argument to produce the `Int` result because of *compiler enforced purity*
  </li>
  <li class="fragment fade-in">
    *Cognitive load* is drastically reduced: there is less to keep in your head when dealing with pure functions
  </li>
</ul>
  
Note: Ask "What does this function do?"

---
# Type Polymorphism

## Purity shines

* Type polymorphism means more generalized functions
* Polymorphic types have lowercase names in type signatures:
  
```haskell
  foo :: a -> a
```
<!-- .element: class="fragment fade-in" -->

<p>
  For **every** type `a`, foo is a function that takes an `a` as an argument and returns an `a`
</p>
<!-- .element: class="fragment fade-in" -->

----

# Type Polymorphism

```haskell
  foo :: a -> a
```
What does this function do?

----

# Type Polymorphism

This function has only one possible implementation.

<p>This function **must** return its argument</p>
<!-- .element: class="fragment fade-in" -->

Why?
<!-- .element: class="fragment fade-in" -->

<ul>
  <li class="fragment fade-in">
    Don't know what `a` is
  </li>
  <li class="fragment fade-in">
    Function must work for any type
  </li>
  <li class="fragment fade-in">
    Not possible to use the data of `a` in any way
  </li>
  <li class="fragment fade-in">
    Guaranteed because of compiler enforced functional purity
  </li>
</ul>

----

# Type Polymorphism

```haskell
  map :: (a -> b) -> [a] -> [b]
```
<p>`map`:</p>
<!-- .element: class="fragment fade-in" -->
<ul>
  <li class="fragment fade-in">
    Is called a ***higher order*** function
  </li>
  <li class="fragment fade-in">
    Takes a function as an argument `(a -> b)`, and list of `a`, and returns a list of `b`
  </li>
  <li class="fragment fade-in">
    `a` and `b` represent different types (but they don't have to be)
  </li>
  <li class="fragment fade-in">
    We don't know anything about `a` and `b`
  </li>
  <li class="fragment fade-in">
    The only way to get the return value is by applying the function to the elements of the list
  </li>
  <li class="fragment fade-in">
    Cognitive load is reduced because there's less to think of  
  </li>
</ul>

----

# Type Polymorphism

Compare that to this implementation:

```haskell
  map :: (Int -> Int) -> [Int] -> [Int]
```
<!-- .element: class="fragment fade-in" -->

<p>This is the exact same function, only operating on `Int`s</p>
<!-- .element: class="fragment fade-in" -->

<p>Because we can use any `Int` functions, it makes it a lot harder to figure out exactly what is happening inside the function</p>
<!-- .element: class="fragment fade-in" -->

<p>Cognitive load is increased when we specify types concretely</p>
<!-- .element: class="fragment fade-in" -->

----

# Type Polymorphism

* Abstracting types reduces what a function can do <!-- .element: class="fragment fade-in" -->
* This makes it easier to reason about functions <!-- .element: class="fragment fade-in" -->
* Sometimes only looking at a function signature is enough to understand what's happening <!-- .element: class="fragment fade-in" -->

---

# Expanding our types

* Abstracted functions are very limited <!-- .element: class="fragment fade-in" -->
* Polymorphic types reduce cognitive load <!-- .element: class="fragment fade-in" -->
* Need a way to get more functionality from polymorphic functions <!-- .element: class="fragment fade-in" -->

Enter typeclasses <!-- .element: class="fragment fade-in" -->

----

# Typeclasses

* A way to classify characteristics of a type <!-- .element: class="fragment fade-in" -->
* Think of typeclasses as interfaces on steroids <!-- .element: class="fragment fade-in" -->
  
<div class="fragment fade-in">
Example, the `Eq` typeclass: 
</div>

<div>
Haskell:

```haskell
  class Eq a where
    (==) :: a -> a -> Bool
```
C#:

```cs
  interface Eq<T> {
    bool Equals(T a, T b);
  }
```
</div>
<!-- .element: class="fragment fade-in" -->

<ul>
  <li class="fragment fade-in">
    Any type that has the classification `Eq` must define an `==` function
  </li>
</ul>

----

# Typeclasses

* Typeclasses can be "inherited" from other typeclasses:
<!-- .element: class="fragment fade-in" -->

```haskell
  class Eq a => Ord a where
    compare :: a -> a -> Ordering
```
<!-- .element: class="fragment fade-in" -->
  
* <p>`Ord` *implies* `Eq`</p>
<!-- .element: class="fragment fade-in" -->
* <p>Any type that implements `Ord` **must** also implement `Eq`</p>
<!-- .element: class="fragment fade-in" -->

----

# Typeclasses

Having the idea of a typeclass, let's see it in action:

```haskell
  foo :: Eq a => [a] -> [a]
```

<ul>
  <li class="fragment fade-in">
    We added `Eq a =>`
    <ul>
      <li class="fragment fade-in">
        `a` must have the `Eq` classification
      </li>
    </ul>
  </li>
  <li class="fragment fade-in">
    Broadens the scope of the function, now it can check equality on it's data
  </li>
  <li class="fragment fade-in">
    Cognitive load is higher, but still low because we know exactly what the function is capable of doing  
  </li>
</ul>

----

# Typeclasses

** Introducing constraints *expands* the scope of a function, but it also *narrows* the types that can call it ** <!-- .element: class="fragment fade-in" -->

<div class="fragment fade-in">
  Only `Eq`s can call this function, and not every type implements `Eq` so the domain of the function is reduced.
</div>

<div class="fragment fade-in">
  Again, this reduces some more of our *cognitive load*.
</div>

----

# Typeclasses

## Some common typeclasses



* Show - for converting to a string <!-- .element: class="fragment fade-in" -->
* Read - for parsing from a string <!-- .element: class="fragment fade-in" -->
* Eq - for equality <!-- .element: class="fragment fade-in" -->
* Ord - for ordering <!-- .element: class="fragment fade-in" -->
* Num - for numeric processing, allows for +, -, * and others <!-- .element: class="fragment fade-in" -->
  * The more functions a typeclass exposes, the more a function that uses it can do <!-- .element: class="fragment fade-in" -->

Using typeclasses makes it really easy to generalize a function while still dictating what can happen within it. <!-- .element: class="fragment fade-in" -->

Take a look at this example:
<!-- .element: class="fragment fade-in" -->

```haskell
  foo :: (Eq a, Show b) => [a] -> b -> String
```
<!-- .element: class="fragment fade-in" -->

<p>**`a`** and **`b`** are different types, one has **`Eq`** and one has **`Show`**, so we know exactly what the function is able to do with the data!</p>
<!-- .element: class="fragment fade-in" -->

---

## Even more abstraction

<p>Haskell allows us to have *higher kinded types*, which are types that themselves take types as arguments.</p>
<!-- .element: class="fragment fade-in" -->

<p>Think of higher order functions (functions that take other functions as arguments, remember `map`)</p>
<!-- .element: class="fragment fade-in" -->

<div class="fragment fade-in">
  Think of C# or Java's generic types
</div>

<div class="fragment fade-in">
  A Haskell type's *kind* is like a function (`*` is a type):
  <br>
  <ul>
    <li class="fragment fade-in">
      `Int` is `*`
    </li>
    <li class="fragment fade-in">
      `[]` is `* -> *`
    </li>
</div>

----

# Examples of higher kinded types

<div>
Haskell:

```haskell
  data Identity a = Identity a -- Kind is * -> *

  data List a = Cons a (List a) | Nil -- Kind is * -> *
```
C#:

```cs
  class Identity<T> {}
  class List<T> {}
```
</div>
<!-- .element: class="fragment fade-in" -->

* Both of these data types require a type in order to make it concrete
<!-- .element: class="fragment fade-in" -->
* <p>Because of the parameterized type, it makes the data type *higher kinded*</p>
<!-- .element: class="fragment fade-in" -->

Here is one that has two parameterized types:
<!-- .element: class="fragment fade-in" -->

<div>
Haskell:

```haskell
  data Either a b = Left a | Right b -- Kind is * -> * -> * 
``` 

C#:

```cs
  class Either<T, U> {}
```
</div>
<!-- .element: class="fragment fade-in" -->

----

# Higher kinded types

This is where the similarities end for Haskell and C#/Java generics. <!-- .element: class="fragment fade-in" -->

Haskell allows you to be polymorphic on the higher kinded types as well.<!-- .element: class="fragment fade-in" -->  

This is not possible in a language like C# or Java.<!-- .element: class="fragment fade-in" -->

----
# Higher kinded types
## An example

```haskell
  class Functor f where
    fmap :: (a -> b) -> f a -> f b
```

<ul>
  <li class="fragment fade-in">
    Notice the **`f a`** and **`f b`**
  </li>
  <li class="fragment fade-in">
    **`fmap`** is polymorphic on three types: **`f`**, **`a`**, and **`b`**!
  </li>
  <li class="fragment fade-in">
    Allows for even more expression in our abstractions
  </li>
  <li class="fragment fade-in">
    Still allows us to reason, **`fmap`** is the only function available to **`Functor`**
  </li>
</ul>

----
# Higher kinded types

Let's take a look at an example:

```haskell
  data Identity a = Identity a                -- this is a built in data type that wraps any data
```
<!-- .element: class="fragment fade-in" -->

```haskell
  data Maybe a = Just a | Nothing             -- this is a built in data type that contains two constructors, one that has a value, and one that doesn't
                                              -- this means that any data of type Maybe could have two possible states, Just or Nothing
```
<!-- .element: class="fragment fade-in" -->

```haskell
  data List a = Cons a (List a) | Nil         -- this is a rough outline for how lists work in Haskell, they are made of two constructors, cons and empty
```
<!-- .element: class="fragment fade-in" -->


```haskell
  data MyDataType = MyDataType Int  -- this is a custom type that wraps an int

  toString :: MyDataType -> String -- let's create a toString function for our type

  mapToString :: Functor f => f MyDataType -> f String -- We don't care what the functor is, just that we have a functor
  mapToString data = fmap toString data  
```
<!-- .element: class="fragment fade-in" -->

Note: Go through this example, make sure to note syntax and lambdas

And mapToString

----
# Higher kinded types

These are some ways we can use this function:

```haskell

  x :: Maybe MyDataType
  x = Just (MyDataType 1)

  y :: [MyDataType]
  y = [MyDataType 1, MyDataType 2]

  z :: Identity MyDataType
  z = Identity (MyDataType 2)

  x' = foo x -- x' == Just "1"
  y' = foo y -- y' == ["1", "2"]
  z' = foo z -- z' == Identity "2"

```
<div class="fragment fade-in">
  **Purity allows us to reason about what a function is capable of doing, and polymorphism and higher kinded types allow us to expand the scope of what's capable**
</div>

Note: Go through the example
---

# Purity and Implementation

Purity can make implementations easier to reason about as well! <!-- .element: class="fragment fade-in" -->

```haskell
  data Identity a = Identity a

  instance Functor Identity where
    fmap :: (a -> b) -> Identity a -> Identity b
    fmap = ???
```
<!-- .element: class="fragment fade-in" -->


```haskell
    -- There is only one possible implementation here:
    fmap fn (Identity a) = Identity (fn a) 
    -- This is the only way the compiler will typecheck
```
<!-- .element: class="fragment fade-in" -->

<p class="fragment fade-in">
  Since we know nothing about `a` and `b`, we must use the function provided to convert them.
</p>

----
# Purity and Implementation

Knowing the typeclass can make implementations easier as well. <!-- .element: class="fragment fade-in" -->

<p class="fragment fade-in">Take the `void` function:</p>

```haskell
    void :: Functor f => f a -> f ()
```
<!-- .element: class="fragment fade-in" -->

<p class="fragment fade-in">`f` must be a Functor</p>

Since we don't know what f is, there is only one implementation of this function:
<!-- .element: class="fragment fade-in" -->

```haskell
  void fa = fmap (\a -> ()) fa
```
<!-- .element: class="fragment fade-in" -->

If we had to implement concrete versions of this for different datatypes, the number of implementations goes up:
<!-- .element: class="fragment fade-in" -->

```haskell
  void :: [a] -> [()] -- How do we implement this?

  void as = [()]
  void as = [(), ()]
  void as = []
  void as = [(), (), ()]
  ... 
```
<!-- .element: class="fragment fade-in" -->

For lists, there are an infinite number of possibilities for this implementation.
<!-- .element: class="fragment fade-in" -->

**Narrowing the types means we don't have to think about as much in terms of implementation**
<!-- .element: class="fragment fade-in" -->

---

# To conclude

* Purity allows us to more easily reason about what is happening in our code <!-- .element: class="fragment fade-in" -->
* Don't have to worry about state changing or mutation <!-- .element: class="fragment fade-in" -->
* Abstractions over types allow us to manipulate data without knowing concrete data types <!-- .element: class="fragment fade-in" -->
* The more abstracted a function, the easier it is to reason about <!-- .element: class="fragment fade-in" -->
* Higher kinded types let us abstract this even further <!-- .element: class="fragment fade-in" -->

**Cognitive load is drastically reduced in a purely functional language, because purity and strong typing enforce restrictions on what is possible, and Haskell is the only language out there that does it** <!-- .element: class="fragment fade-in" -->

---
# Questions?

---

# Thanks!

## Some resources

* https://www.haskell.org/
* https://github.com/data61/fp-course
* http://haskellbook.com/


Come see me if you want to know more about Haskell, or if you want any training, I'd be happy to provide!