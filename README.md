# Haskell Notes and Examples

_Brushing up my Haskell skills._

## Intro

Haskell is a *purely function programming language*.

We express what things are in terms of functions.

Functions have no side effects, as everything is immutable. As a result we get _referential transparency_.

Haskell is _lazy_ and _statically typed_.

To load a file into `ghci` use the command `:l filename.hs`. You can reload it with `:r`.

## Basics

Numeric operations are as usual, with the common precedence rules and parenthesis to make it explicit or to change it. Negative numbers should be enclosed in parenthesis.

Boolean algebra and comparisons are straightforward as well.

```haskell
ghci> True && not (42 == 42)
False
```

Functions are called by writing their name, a space and then a list of space separated parameters.

```haskell
ghci> min 9 10
9
ghci> succ 8
9
```

Function application have the highest precedence:

```haskell
ghci> succ 9 + max 5 4 + 1
16
ghci> (succ 9) + (max 5 4) + 1
16
ghci> succ 9 * 10
100
ghci> succ (9 * 10)
91
```

We can infix funcitons of two parameters by using backtics: ```92 `div` 10```.

Characters go in between simple quotes, and strings are just lists of characters.

## Functions

```haskell
ghci> doubleThis x = x + x
ghci> doubleThis 42
84
ghci> superThing x = doubleThis x + doubleThis x
ghci> superThing 42
168
```

Conditionals always require an `else` clause. Because of this, they always return a value, hence conditions are _expressions_.

```haskell
absoluteValue x = if >= 0 
                    then x
                    else x * (-1)
```

Functions can't start with capital letters and if they take no parameters they are _definitions_.

## Lists

Lists are _homogenous_, all elements must be of the same type.

Nested lists can have different lengths but not different typs.

```haskell
ghci> let lostNumbers = [4,8,15,16,23,42]
ghci> lostNumbers
[4,8,15,16,23,42]
```

Concatenation of lists is done with the `++` operator. Beware of doing this with large lists -it's costly-. It works with two lists, so to add a single element, it must be enclosed in brackets.

`[1,2,3]` is syntactic sugar for `1:2:3:[]`.

Appending at the beginning is instantaneous. Use the `cons` operator `:`.

```haskell
ghci> 1:[2,3,4,5]
[1,2,3,4,5]
```

To retrieve an element we use `!!` and its index (starting at 0).

Lists are compared in lexicographical order.

Other simple functions for lists: head, tail, last, init, length, null, reverse, take, drop, maximum, minimum, sum, product, elem.

```haskell
ghci> head [1,2,3,4,5]
1
ghci> tail [1,2,3,4,5]
[2,3,4,5]
ghci> last [1,2,3,4,5]
5
ghci> init [1,2,3,4,5]
[1,2,3,4]
ghci> length [1,2,3,4,5]
5
ghci> null [1,2,3,4,5]
False
ghci> null []
True
ghci> reverse [1,2,3,4,5]
[5,4,3,2,1]
ghci> take 3 [1,2,3,4,5]
[1,2,3]
ghci> take 0 [1,2,3,4,5]
[]
ghci> drop 3 [1,2,3,4,5]
[4,5]
ghci> drop 0 [1,2,3,4,5]
[1,2,3,4,5]
ghci> drop 100 [1,2,3,4,5]
[]
ghci> maximum [1,2,3,4,5]
5
ghci> minimum [1,2,3,4,5]
1
ghci> sum [1,2,3,4,5]
15
ghci> product [1,2,3,4,5]
120
ghci> 4 `elem` [1,2,3,4,5]
True
ghci> 6 `elem` [1,2,3,4,5]
False
```

Exceptions will be raised if we try to retrieve an element that doesn't exist (as the head of an empty list).

## Ranges

Generating a list including all numbers between a and b is done with: `[a..b]`.

It also works with chars: `['a'..'b']`.

A step can also be specified. Get even numbers in a range with `[0,2..10]`

For reverse, you have to specified the reversed step: `[10,9..1]`.

Ranges can be infinite! `[1,3..]` odd numbers to infinite.

To get the first n odd numbers in a range well... `take n [1,3..]`

`cycle` takes a list and cycles it to infinite.

`repeat` takes an element and makes an infinit list of that element.

`replcate a b` gives a list of a b's.

## List comprehensions

`[x * 2 | x <- [1..10]]`

x is drawn from the range and then multiplied by 2. We can also add conditions (more than one, a returned element will have to satisfy all predicates):

```[x * 2 | x <- [1..10], x `mod` 2 == 0]```

Print odd or even according to the elements of a list

`oddOrEven xs = [ if odd x then "ODD" else "EVEN" | x <- xs]`

When drawing from multiple lists comprehensions will produce all combinations of the given lists and join them:

`[x * y | x <- [1..10], y <- [11..20]]`

Possible implementation of `length`:

```length' xs = sum [1 | _ <- xs]```

Get even numbers from nested lists, without flattening them:

`nestedEvens xxs = [ [ x | x <- xs, even x ] | xs <- xxs ]`

