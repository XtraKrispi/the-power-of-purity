---
title: The Power of Purity
theme: solarized
highlightTheme: obsidian
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
  <li>Two kids, Don (4) and Myles (1.66) and wife Lisa
      <div style="text-align: center">
        <img style="margin:0;background:0;border:0;box-shadow:none;height: 300px;" src="images/don-pic.jpg"><img style="margin:0;background:0;border:0;box-shadow:none;height:300px;" src="images/myles-pic.jpg">
      </div>
  </li>
  <li>Avid board gamer, video gamer, techology lover</li>
  <li>Love functional programming including Haskell, Elm, F#</li>
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

* Turing complete general purpose purely functional programming language <!-- .element: class="fragment" -->

* No side effects, every operation is pure <!-- .element: class="fragment" -->

**For the same input, Haskell will always return the same output** <!-- .element: class="fragment" -->

Note: Talk about how side effects are possible, just not in the way you think

----

# Haskell

## Some syntax

Functions signatures are written like this:
```haskell
  add :: Int -> Int -> Int
```

<ul>
  <li class="fragment fade-in">
    `::` *has the type*
  </li>
  <li class="fragment fade-in">
    `->` Argument separator
  </li>
  <li class="fragment fade-in">
    Final data type is the return type of the function
  </li>
  <li class="fragment fade-in">
    Functions have *lowercase* names, types must be *uppercase*
  </li>
</ul>
  
Note:  Explain the different parts of a function signature.

Also explain that most of the time function signatures are optional, but are useful documentation

----

# Haskell

## Back to our example

The Haskell version of the function we saw before looks like:

```haskell
  increment :: Int -> Int
```

<ul>
  <li class="fragment fade-in">
    Impossible to alter state, so it can only return an `Int` of some kind
  </li>
  <li class="fragment fade-in">
    *Cognitive load* is drastically reduced because of purity
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

For **every** type `a` foo is a function that takes an `a` as an argument and returns an `a`

----

# Type Polymorphism

```haskell
  foo :: a -> a
```
What does this function do?

----

# Type Polymorphism

This function **must** return its argument

Why?

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
    Guaranteed because of functional purity
  </li>
</ul>

----

# Type Polymorphism

```haskell
  map :: (a -> b) -> [a] -> [b]
```
`map`:
<ul>
  <li class="fragment fade-in">
    Takes a function as an argument, and list of `a`
  </li>
  <li class="fragment fade-in">
    `a` and `b` represent different types
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

* Abstracting types reduces what a function can do <!-- .element: class="fragment fade-in" -->
* This makes it easier to reason about functions <!-- .element: class="fragment fade-in" -->
* Sometimes only looking at a function signature is enough to understand basically what's happening <!-- .element: class="fragment fade-in" -->

---

# Expanding our types

* Abstracted functions are very limited <!-- .element: class="fragment fade-in" -->
* Polymorphic types reduce cognitive load <!-- .element: class="fragment fade-in" -->
* Need a way to get more functionality from polymorphic functions <!-- .element: class="fragment fade-in" -->

Enter typeclasses <!-- .element: class="fragment fade-in" -->

----

# Typeclasses

* A way to classify characteristics of a type <!-- .element: class="fragment fade-in" -->

<div class="fragment fade-in">
Example, the `Eq` typeclass: 
</div>
<pre class="fragment fade-in">
  <code class="lang-haskell">
    class Eq a where
      (==) :: a -> a -> Bool
  </code>
</pre>


<ul>
  <li class="fragment fade-in">
    Any type that has the classification `Eq` must define an `==` function
  </li>
</ul>

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
    Cognitive load is still low because we know exactly what the function is capable of doing  
  </li>
</ul>

----

# Typeclasses

### Introducing constraints expands the scope of a function, but it also narrows the types that can call it <!-- .element: class="fragment fade-in" -->

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

---

## Even more abstraction

<div class="fragment fade-in">
  Haskell allows us to have *higher kinded types*, which are types that themselves take types as arguments. 
</div>

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
    Notice the `f a` and `f b`
  </li>
  <li class="fragment fade-in">
    `fmap` is polymorphic on three types: `f`, `a`, and `b`!
  </li>
  <li class="fragment fade-in">
    Allows for even more expression in our abstractions
  </li>
  <li class="fragment fade-in">
    Still allows us to reason, because only `fmap` is available for `Functor`
  </li>
</ul>

----
# Higher kinded types

Let's take a look at an example:

```haskell
  data Identity a = Identity a      -- this is a built in data type that wraps any data

  data Maybe a = Just a | Nothing   -- this is a built in data type that contains two constructors, one that has a value, and one that doesn't
                                    -- this means that any data of type Maybe could have two possible states, Just or Nothing

  data [a] = (:) a [a] | []         -- this is a rough outline for how lists work in Haskell, they are made of two constructors, cons and empty

  -- ALL OF THE ABOVE ARE INSTANCES OF FUNCTOR

  data MyDataType = MyDataType Int  -- this is a custom type that wraps an int

  toString :: MyDataType -> String

  foo :: Functor f => f MyDataType -> f String -- We don't care what the functor is, just that we have a functor
  foo data = fmap (\mdt -> toString mdt) data  
```

Note: Go through this example, make sure to note syntax and lambdas

And foo

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

<pre class="fragment fade-in">
  <code class="lang-haskell">
    data Identity a = Identity a

    instance Functor Identity where
      fmap :: (a -> b) -> Identity a -> Identity b
      fmap = ???
  </code>
</pre>
<pre class="fragment fade-in">
  <code class="lang-haskell">
    -- There is only one possible implementation here:
    fmap fn (Identity a) = Identity (fn a) 
    -- This is the only way the compiler will typecheck
  </code>
</pre>

<div class="fragment fade-in">
  Since we know nothing about `a` and `b`, we must use the function provided to convert them.
</div>

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