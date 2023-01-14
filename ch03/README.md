# Chapter 03

1. What are the types of the following values?

```haskell
['a', 'b', 'c']             -- [Char]

('a', 'b', 'c')             -- (Char, Char, Char)

[(False, '0'), (True, '1')] -- [(Bool, Char)]

([False, True], ['0', '1']) -- ([Bool], [Char])

[tail, init, reverse]       -- [[a] -> [a]]
```

2. Write down the definitions that have the following types; it does not matter what the definitions actually do as long as they are type correct.

```haskell
bools :: [Bool]
bools = [True, False, True]

nums :: [[Int]]
nums = [[0], [0, 1], [0, 1, 2]]

add :: Int -> Int -> Int -> Int
add w x y z = w + x + y + z

copy :: a -> (a, a)
copy x = (x, x)

apply :: (a, b) -> a -> b
apply f x = f x
```

3. What are the types of the following functions?

```haskell
second xs = head (tail xs) -- second :: [a] -> a

swap (x, y) = (y, x) -- swap :: (a, b) -> (b, a)

pair x y = (x, y) -- pair :: a -> b -> (a, b)

double x = x * 2 -- double :: Num a => a -> a

palindrome xs = reverse xs == xs -- palindrome :: [a] -> Bool

twice f x = f (f x) -- twice :: (a -> b) -> a -> b
```

Hint: take care to include the necessary class constraints in the types if the function are defined using overloaded operators.

4. Check your answers to the preceding three questions using GHCi.
Question 1:
```haskell
ghci> :type ['a', 'b', 'c']
['a', 'b', 'c'] :: [Char]
ghci> :type ('a', 'b', 'c')
('a', 'b', 'c') :: (Char, Char, Char)
ghci> :type [(False, '0'), (True, '1')]
[(False, '0'), (True, '1')] :: [(Bool, Char)]
ghci> :type ([False, True], ['0', '1'])
([False, True], ['0', '1']) :: ([Bool], [Char])
ghci> :type [tail, init, reverse]
[tail, init, reverse] :: [[a] -> [a]]
```

Question 2:
```haskell
ghci> bools = [True, False, True]
ghci> :type bools
bools :: [Bool]
```

Question 3:
```haskell
ghci> :type second
second :: [a] -> a
ghci> :type swap
swap :: (b, a) -> (a, b)
ghci> :type pair
pair :: a -> b -> (a, b)
ghci> :type double
double :: Num a => a -> a
ghci> :type palindrome
palindrome :: Eq a => [a] -> Bool
ghci> :type twice
twice :: (t -> t) -> t -> t
```

* For `palindrome`, I forgot the class constraint `Eq` which is needed since two lists are being compared with `==`.

* For `twice`, I got the types wrong. 

5. Why is it not feasible in general for function types to be instances of the `Eq` class? When is it feasible? Hint: two functions of the same type are equal if they always return equal results for equal arguments.

In general it's not feasible to compare two functions for equality.

It's feasible if the domain of the function (the input type) is finite and the range (the output type) is an instance of the `Eq` class, at which point we can test for equality by enumerating all possible inputs. [Reddit comment](https://old.reddit.com/r/haskellquestions/comments/fi17cm/eq_instance_for_function/fkehhls/)

