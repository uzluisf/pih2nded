# Chapter 06: Recursive functions

1. How does the recursive version of the factorial function if applied to a
   negative argument, such as `(-1)`? Modify the definition to prohibit
   negative arguments by adding a guard to the recursive case.

    In the chapter factorial is defined recursively as follows:
    
    ```haskell
    fac :: Int -> Int
    fac 0 = 1
    fac n = n * fac (n-1)
    ```
    
    If applied to a negative argument, the function will theoretically recurse indefinitely, however the computer will run out of memory before that happen.
    
    ```haskell
    fact :: Int -> Int
    fac 0 = 1
    fact n
       | n < 0     = 0
       | otherwise = n * fac (n-1)
    ``` 
    
    Here `fac` will simply return `0` for negative arguments.

2. Define a recursive function `sumdown :: Int -> Int` that returns the sum of
   the non-negative integers from a given value down to zero. For example,
   `sumdown 3` should the reult `3+2+1+0 = 6`.

   ```haskell
   sumdown :: Int -> Int
   sumdown 0 = 0
   sumdown n = n + sumdown (n-1)
   ```

3. Define the exponentiation operator `^` for non-negative integers using the
   same pattern of recursion as the multiplication operator `*`, and show how
   the expression `2 ^ 3` is evaluated using your definition.

   ```haskell
   (^) :: Int -> Int -> Int
   b ^ 0 = 1
   b ^ n = b * (b ^ (n-1)) 
   ```
   
   Evaluation:
   
   ```haskell
   2 ^ 3
   = 2 * (2 ^ (3-1))
   = 2 * (2 * (2 ^ (2-1)))
   = 2 * (2 * (2 * (2 ^ (1-1))))
   = 2 * (2 * (2 * (2 ^ 0)))
   = 2 * (2 * (2 * 1))
   = 8
   ```

4. Define a recursive function `euclid :: Int -> Int -> Int` that implements
   *Euclid's algorithm* for calculating the greatest common divisor for two
   non-negative integers: if the two numbers are equal, this number is the
   result; otherwise, the smaller number is substracted from the larger, and
   the same process is then repeated. For example:

   ```haskell
   > euclid 6 27
   3
   ```
   
   Implementation:
   
   ```haskell
   euclid :: Int -> Int -> Int
   euclid a b
   	| a == b    = a
   	| a < b     = euclid a (b-a)
   	| otherwise = euclid (a-b) b
   ```
   
   Evaluation:
   
   ```haskell
   euclid 6 27
   = euclid 6 (27 - 6)
   = euclid 6 (21 - 6)
   = euclid 6 (15 - 6)
   = euclid 6 9
   = euclid 6 (9 - 6)
   = euclid (6 - 3) 3
   = 3
   ```

5. Using the recursive definitions given in this chapter, show how `length [1,2,3]`, `drop 3 [1,2,3,4,5]`, and `init [1,2,3]` are evaluated.

   ```haskell
   length [1,2,3]
   = 1 + length [2,3]
   = 1 + (1 + length [3])
   = 1 + (1 + (1 + length [0]))
   = 1 + (1 + (1 + 0))
   = 3
   
   drop 3 [1,2,3,4,5]
   = drop 2 [2,3,4,5]
   = drop 1 [3,4,5]
   = [4,5]
   
   init [1,2,3]
   = 1 : init [2,3]
   = 1 : 2 : init [3]
   = 1 : 2 : []
   = [1,2]
   ```

6. Without looking at the definitions from the standard prelude, define the
   following library functions on lists using recursion.

   a. Decide if all logical values in a list are `True`:
   
   ```haskell
   and :: [Bool] -> Bool
   and (x:xs)
   	| null xs   = x
   	| otherwise = x && and xs  
   ```
   
   b. Concatenate a list of lists:
   
   ```haskell
   concat :: [[a]] -> [a]
   concat (xs:[])  = xs
   concat (xs:xss) = xs ++ concat xss
   ```
   
   c. Concatenate a list with `n` identical elements:
   
   ```haskell
   replicate :: Int -> a -> [a]
   replicate 0 x = []
   replicate n x = x : replicate (n-1) x
   ```
   
   d. Select the `nth` element of a list:
   
   ```haskell
   (!!) :: [a] -> Int -> a
   (x:_) !! 0  = x
   (_:xs) !! n = xs !! (n-1)
   ```
   
   e. Decide if a value is an element of a list:
   
   ```haskell
   elem :: Eq a => a -> [a] -> Bool
   elem x [] = False
   elem x (y:ys)
   	| x == y    = True
   	| otherwise = elem x ys
   ```

   Note: most of these functions are defined in the prelude using other library
   functions rather than using explicit recursion, and are generic functions
   rather than being specific to the type of lists.

7. Define a recursive function `merge :: Ord a => [a] -> [a] -> [a]` that
   merges two sorted lists to give a single sorted list. For example:

   ```haskell
   > merge [2,5,6] [1,3,4]
   [1,2,3,4,5,6]
   ```

   Note: your definition should not use other functions on sorted lists such as
   `insert` or `isort`, but should be defined using explicit recursion.

   Implementation:

   ```haskell
   merge :: Ord a => [a] -> [a] -> [a]
merge [] ys = ys
merge xs [] = xs
merge (x:xs) (y:ys)
    | x < y     = x : merge xs (y:ys)
    | otherwise = y : merge (x:xs) ys   
```

8. Using `merge`, define a function `msort :: Ord a => [a] -> [a]` that
   implements *merge sort*, in which the empty list and singleton lists are
   already sorted, and any other list is sorted by merging together the two
   lists that result from sorting the two halves of the list separately.

   ```haskell
halve :: [a] -> ([a], [a])
halve xs = (take mid xs, drop mid xs)
          where
             mid = (length xs) `div` 2
msort :: Ord a => [a] -> [a]
msort []  = []
msort [x] = [x]
msort xs = merge (msort lower) (msort upper)
         where
            lower = fst (halve xs)
            upper = snd (halve xs)
```
   
   Hint: first define a function `halve :: [a] -> ([a], [a])` that splits a
   list into two halves whose lengths differ by at most one.

9. Using the five-step process, construct the library functions that:

   a. calculate the `sum` of a list of numbers;

   ```haskell
   -- step 1
   sum :: [Int] -> Int

   -- step 2
   sum :: [Int] -> Int
   sum [] =
   sum (x:xs) =

   -- step 3
   sum :: [Int] -> Int
   sum [] = 0
   sum (x:xs) =

   -- step 4
   sum :: [Int] -> Int
   sum [] = 0
   sum (x:xs) = x + sum xs

   -- step 5
   sum :: Integral a => [a] -> a
   sum [] = 0
   sum (x:xs) = x + sum xs
   ```

   b. `take` a given number of elements from the start of a list;

   ```haskell
   -- step 1
   take :: Int -> [Int] -> [Int]

   -- step 2
   take :: Int -> [Int] -> [Int]
   take 0 xs     =
   take n (x:xs) =

   -- step 3
   take :: Int -> [Int] -> [Int]
   take 0 xs     = []
   take n (x:xs) =

   -- step 4
   take :: Int -> [Int] -> [Int]
   take 0 xs     = []
   take n (x:xs) = x : take (n-1) xs

   -- step 5
   take :: a => Int -> [a] -> [a]
   take 0 xs     = []
   take n (x:xs) = x : take (n-1) xs
   ```

   c. select the `last` element of a non-empty list.

   ```haskell
   -- step 1
   last :: [Int] -> Int

   -- step 2
   last :: [Int] -> Int
   last 
   ```
