# Chapter 05

1. Using a list comprehension, give an expression that calculates the sum
   1<sup>2</sup> + 2<sup>2</sup> + ... + 100<sup>2</sup> of the first one
   hundred integer squares.

```haskell
first100squares = [x * x | x <- [1..100]]
```

2. Suppose that a *coordinate grid of size m X n* is given by the list of all
   pairs `(x,y)` of integers such that `0 <= x <= m` and `0 <= y <= n`. Using a
   list comprehension, define a function `grid :: Int -> Int -> [(Int, Int)]`
   that returns `n` coordinate grid of a given size. For example:

```haskell
> grid 1 2
[(0, 0), (0, 1), (0, 2), (1, 0), (1, 1), (1, 2)]
```

Implementation:

```haskell
grid :: Int -> Int -> [(Int,Int)]
grid m n = [(x, y) | x <- [0..m], y <- [0..n]]
```

3. Using a list comprehension and the function `grid` above, define a function
   `square :: Int -> [(Int, Int)]` that returns a coordinate square of size `n`
   excluding the diagional from `(0, 0)` to `(n, n)`. For example:

```haskell
> square 2
[(0,1),(0,2),(1,0),(1,2),(2,0),(2,1)]
```

Implementation:

```haskell
square :: Int -> [(Int,Int)]
square n = [(x, y) | (x, y) <- grid n n, x /= y]
```

Note: I thought Haskell also `!=` for inequality testing. Instead it
uses `/=`.

4. In a similar way to the function `length`, show how the library function
   `replicate :: Int -> a -> [a]` that produces a list of identical elements can
   be defined using a list comprehension. For example:

```haskell
> replicate 3 True
[True,True,True]
```

Implementation:

```
replicate :: Int -> a -> [a]
replicate n x = [x | _ <- [1..n]]
```

5. A triple `(x,y,z)` of positive integers is *Pythagorean* if it satisfies the
   equation x<sup>2</sup> + y<sup>2</sup> = z<sup>2</sup>. Using a list
   comprehension with three generators, define a funcyion `pyths :: Int ->
   [(Int,Int,Int)]` that returns the list of all such triples whose components
   are at a given limit. For example:

```haskell
> pyths 10
[(3,4,5),(4,3,5),(6,8,10),(8,6,10)]
```

Implementation:

```haskell
pyths :: Int -> [(Int,Int,Int)]
pyths n = [(x,y,z) | x <- [1..n], y <- [1..n], z <- [1..n], x * x + y * y == z * z]
```

6. A positive integer is *perfect* if it equals the sum of all of its factors,
   excluding the number itself. Using a list comprehension and the function
   `factors`, define a function `perfects :: Int -> [Int]` that returns the list
   of all perfect numbers ip to a given limit. For example:

```haskell
> perfects 500
[6,28,496]
``` 

Implementation:

```haskell
perfects :: Int -> [Int]
perfects l = [n | n <- [1..l], sum (tail (reverse (factors n))) == n]
```

7. Show how the list comprehension `[(x,y) | x <- [1,2], y <- [3,4]]` with two
   generators can be re-expressed using two comprehensions with single
   generators. Hint: nest one comprehension within the other and make use of the
   library function `concat :: [[a]] -> [a]`.

```haskell
concat [[(x, y) | x <- [1,2]] | y <- [3, 4]]
concat [[(1,3),(2,3)],[(1,4),(2,4)]]
[(1,3),(1,4),(2,3),(2,4)]
```

**Comment:** For this one I had to turn to the internet. I'm still
uncertain if this is the right solution given that if a generator
takes the form `x <- [1, 2]`, then it stands to chance that `y <- [3,4]`
is another generator. Hence this solution still has two generators.

8. Redefine the function `positions` using the function `find`.

```haskell
positions :: Eq a => a -> [a] -> [Int]
positions x xs = find x (zip xs [0..])
```

9. The *scalar product* of two lists of integers `xs` and `ys` of length `n` is
   given by the sum of the products of corresponding integers:

```math
\sum^{n-1}_{i=0}(xs_i * ys_i)
```

In a similar to `chisqr`, show how a list comprehension can be used to define a
function `scalarproduct :: [Int] -> [Int] -> Int` that returns the scalar
product of two lists. For example:

```haskell
> scalarproduct [1,2,3] [4,5,6]
```

Implementation:

```haskell
scalarproduct :: [Int] -> [Int] -> Int
scalarproduct xs ys = sum [fst ps * snd ps | ps <- zip xs ys]
```
10. Modify the Caesar cipher program to also handle upper-case letters.

```haskell
import Data.Char

-- Convert from lowercase to integer in the range [0,25].
low2int :: Char -> Int
low2int l = ord l - ord 'a'

-- Convert from uppercase to integer in the range [0,25].
upp2int :: Char -> Int
upp2int u = ord u - ord 'A'

-- Convert from integer to lowercase.
int2low :: Int -> Char
int2low n = chr (ord 'a' + n)

-- Convert from integer to uppercase.
int2upp :: Int -> Char
int2upp n = chr (ord 'A' + n)

-- Apply shift factor to a character (e.g., shift 3 'a' -> 'd').
shift :: Int -> Char -> Char
shift n c
    | isLower c = int2low ((low2int c + n) `mod` 26)
    | isUpper c = int2upp ((upp2int c + n) `mod` 26)
    | otherwise = c

-- Encode string by factor.
encode :: Int -> String -> String
encode n xs = [shift n x | x <- xs]
```

**Comment:** Since lowercase letters are already mapped to the
range `[0,25]`, I was trying to devise a non-overlapping range
for the uppercase letters in order to avoid any clashes when converting
from integers to letters. However since the `shift` uses both `isLower`
and `isUpper`, then there won't be any clashes even though both
letter cases mapped to the same integer range,
