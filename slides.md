---
title: The Power of Purity
theme: solarized
highlightTheme: obsidian
---

# About me

  <img src="images/goldie.jpg" style="margin:0;background:0;border:0;box-shadow:none;" />
  <ul>
    <li>Michael Gold, Lead Software Engineer Analytics</li>
    <li>Work out of Toronto, live in Vaughan, Ontario <div style="text-align: center"><img style="margin:0;background:0;border:0;box-shadow:none;height: 300px;" src="images/wonderland.jpg"></div></li>
  </ul>
    
---

# About me

<ul>
  <li>Two kids, Don (4) and Myles (1.66) and wife Lisa
      <div style="text-align: center">
        <img style="margin:0;background:0;border:0;box-shadow:none;height: 300px;" src="images/don.jpg"><img style="margin:0;background:0;border:0;box-shadow:none;height:300px;" src="images/myles.jpg">
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

# Does it do what you thought?

```cs
  public int Increment(int n) {
    var toReturn = n + globalValue;

    globalValue += 10;

    _someService.fireZeMissiles();
  }
```

---

# What is purity?

A function is considered pure if it does not alter state outside of the function itself.

This means no global mutation, no side effects, nothing that affects anything but the inputs and the outputs.

---
# Haskell

* Haskell is a purely functional programming language.  

* Because Haskell is a pure language, there are no such things as side effects, and every function is pure.

* Haskell functions are more akin to mathematical functions as opposed to typical functions in most programming languages:

**For the same input, every single function in Haskell returns the same output.**

----

# Haskell

## Some syntax

Functions signatures are declared like this:
```haskell
  add :: Int -> Int -> Int
```

This means that `add` *has the type (::)* `Int -> Int -> Int`

The `->` separates arguments to the function, and the final datatype is the return type of the function.

All functions must start with a lowercase letter, and all datatypes must start with an uppercase.

----

# Haskell

## Back to our example

In Haskell, the function we saw before looks like this:

```haskell
  increment :: Int -> Int
```

What does this function do?

We're not really sure the business logic, but we do know that it is impossible for this function
to alter any state, so our *cognitive load* is drastically reduced.

---
# Type Polymorphism

## Purity shines

Haskell has polymorphism in types, allowing for more generalized functions to be written.

We denote polymorphic types by using lowercase names within function signatures:

```haskell
  foo :: a -> a
```

This is exactly the same as writing:

```haskell
  foo :: forall a. a -> a
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

It is **impossible**, because of *functional purity*, for this function to do anything except return its argument.

Why?

Since we have no knowledge of what the type of `a` is, and the function works for any type conceivable, the function implementation
cannot possibly do anything to the datatype, and thus must return it.

----

# Type Polymorphism

```haskell
  map :: (a -> b) -> [a] -> [b]
```

`map` is a function that itself takes as an argument a function `(a -> b)`, as well as a *list* of `a`

Knowing that list has ways of traversing the list, there is only one thing the function can logically do (there are variations on exactly the logic, but the cognitive overhead is the same).

The only thing this function can do is transform each element using the function provided, since we know nothing about the types of `a` and `b`.

----

# Type Polymorphism

Abstracting type information away reduces the amount of operations that are possible within a function, making it easier to *reason* about what the function can possibly do.  

Oftentimes, it is unnecessary to look at the implementation of a function when the type signature provides most of the information required.

---

# Typeclasses
## Expanding our types

We want to keep functions as general as possible, so we don't have to think about their implementations.

This reduces our *cognitive load* and allows us to focus on the logic and flow of an application.

Restricting functions to polymorphic types only gets us so far though, since we have no knowledge about the types at all.

Let's expand that with *typeclasses*

----

# Typeclasses

Typeclasses are a mechanism for classifying types into specific buckets of functionality.

Let's take a look at the `Eq` typeclass

```haskell
  class Eq a where
    (==) :: a -> a -> Bool
```

This is saying that the `Eq` typeclass defines an `==` function that takes two arguments of the **same** type (since the two letters are the same), and returns a `Bool`

Any type that implements the `Eq` typeclass can then use the `==` function.

----

# Typeclasses

Having the idea of a typeclass, let's see it in action:

```haskell
  foo :: Eq a => [a] -> [a]
```

Notice how we added `Eq a =>`.  This means that now we are saying that `a` must be an instance of `Eq` in order to use this function.

It is not very clear what this function does, but because of the Eq statement, it likely to do some checking between it's elements, either checking equality or non-equality.

Notice that now we've broadened what this function can do by introducing the typeclass constraint.  

But also notice that because we've introduced the constraint, and because Haskell has purity, we know everything that we're capable of doing within
the function, just by it's type signature.  

We know that **all** we are able to do with the data is compare the elements, and we know that because we have a list we can traverse it.

----

# Typeclasses

## Introducing constraints expands the scope of a function, but it also narrows the types that can call it

Only `Eq`s can call this function, and not every type implements `Eq` so the domain of the function is reduced.

Again, this reduces some more of our *cognitive load*.

----

# Typeclasses

## Some common typeclasses

Some typeclasses include:

* Show - for converting to a string
* Read - for parsing from a string
* Eq - for equality
* Ord - for ordering
  * `foo :: Ord a => [a] -> [a]` is likely to do one of two things, sort ascending, or sort descending.
* Num - for numeric processing, allows for `+, -, *` and others
  * The more functions a typeclass exposes, the more a function that uses it can do

Using typeclasses makes it really easy to generalize a function while still dictating what can happen within it.

---

# Higher kinded types
## Even more abstraction

Haskell allows us to have *higher kinded types*, which are types that themselves take types as arguments.

Think of *generics* in C# and Java: `List<T>` or `Foo<T>` are types that take another type as an argument.

In Haskell, the *kind* of a type is denoted almost like a function: 
  *The *kind* of `Int` is `*`
  *The *kind* of `[]` is `* -> *`

The `*` basically denotes a type, so list is a type that takes as an argument another type.

----

# Higher kinded types

This is where the similarities end for Haskell and C#/Java generics.

Haskell allows you to be polymorphic on the higher kinded types as well.  This is not possible in a language like C# or Java.

----
# Higher kinded types
## Functor

```haskell
  class Functor f where
    fmap :: (a -> b) -> f a -> f b
```

Notice that we are defining a typeclass that operates on a higher kinded type (you can tell because of the `f a` and `f b` within `fmap`)

This means that `fmap` is polymorphic not only on the *generic* type, but also the *container* type as well!

This introduces even more possibilities for abstraction, since we no longer need to know what the vessel is for holding the data, or what the data is itself!

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

Because we are using `Functor` in our function signature, we know that we will be `fmap`ping over the data within it.  We can know this because purity tells us that there's nothing else we can do regarding the polymorphic type.  

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

Notice how we call the function identically each time?  This is the power of higher kinded types and pure type polymorphism.

Our custom function `foo` doesn't care how it's being called, as long as it's within a functor it just works.

** Purity allows us to reason about what a function is capable of doing, and polymorphism and higher kinded types allow us to expand the scope of what's capable **

---

# Purity and Implementation

Purity allows us to make implementing functions easier to reason about as well. Let's implement Functor for Identity

```haskell
  data Identity a = Identity a

  instance Functor Identity where
    fmap :: (a -> b) -> Identity a -> Identity b
    fmap = ???
```

```haskell
  fmap fn (Identity a) = Identity a -- This is expecting a result of Identity b, so one thing we could try is returning a...
                -- This doesn't typecheck, since a is of type Identity a, so this implementation is impossible

  -- There is only one possible implementation here:
  fmap fn (Identity a) = Identity (fn a) -- This is the only way the compiler will typecheck
```

Since we know nothing about `a` and `b`, we must use the function provided to convert them.

This kind of reasoning about implementations and meaning behind functions is everywhere in Haskell, and allows the developer to focus on 
logic as opposed to syntax and other useless details.

---

# Questions?

---

# Thanks!

## Some resources

* https://www.haskell.org/
* https://github.com/data61/fp-course
* http://haskellbook.com/


Come see me if you want to know more about Haskell, or if you want any training, I'd be happy to provide!