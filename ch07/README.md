# Chapter 07: Higher-order functions

1. Show how the list comprehension `[f x | x <- xs, p x]` can be re-expressed
   using the higher-order functions `map` and `filter`.

    ```haskell
    mapFilter f p xs = map f (filter p xs)
    ```

    For example, to get the list of squared even numbers from a list of numbers using the list comprehension:

    ```haskell
    let xs = [1,2,3,4,5]
    [(^2) x | x <- xs, even x]
    [4, 16]
    ```
    
    Using `mapFilter`:
    
    ```haskell
    mapFilter (^2) even xs
    map (^2) (filter even xs)
    -- { applying filter }
    map (^2) [2, 4]
    -- { applying map}
    [4, 16]
    ```

2. Without looking at the definitions from the standard prelude, define the
   following higher-order library functions on lists.
    1. Decide if all elements of a list satisfy a predicate:

       ```haskell
       all' :: (a -> Bool) -> [Bool] -> Bool
       all' p [x]    = p x
       all' p (x:xs) = p x && all p xs
       ```

    2. Decide if any element of a list satisfies a predicate:

       ```haskell
       any' :: (a -> Bool) -> [a] -> Bool
       any' p []       = False
       any' p (x:xs)
           | p x       = True
           | otherwise = any' p xs
       ```

    3. Select elements from a list while they satisfy a predicate:

       ```haskell
       takeWhile' :: (a -> Bool) -> [a] -> [a]
       takeWhile' p (x:xs)
           | p x       = x : takeWhile' p xs
           | otherwise = []
       ```

    4. Remove elements from a list while they satisfy a predicate:

       ```haskell
       dropWhile' :: (a -> Bool) -> [a] -> [a]
       dropWhile' p (x:xs)
           | p x       = dropWhile' p xs
           | otherwise = [x] ++ xs
       ```

   Note: in the prelude the first twoof these functions are generic functions
   rather than being specific to the type of lists.

3. Redefine the functions `map f` and `filter p` using `foldr`.

   Thus using `foldr`, `map` can be expressed as:

   ```haskell
   map' :: (a -> b) -> [a] -> [b]
   map' f = foldr (\curr acc -> f curr : acc) []
   ```

   The `filter` function can be expressed as follows using `foldr`:

   ```haskell
   filter' p = foldr (\curr acc -> if p curr then curr : acc else acc) []
   ```

4. Using `foldl`, define a function `dec2int :: [Int] -> Int`that converts
   decimal number into an integers. For example:

   ```haskell
   > dec2int [2,3,4,5]
   2345
   ```

   ```haskell
   dec2int :: [Int] -> Int
   dec2int = foldl (\acc curr -> acc * 10 + curr) 0
   ```

5. Without looking at the definitions from the standard prelude, define the
   higher-order library function `curry` that converts a function on pairs into
   a curried function, and, conversely, the function `uncurry` that converts a
   curried function with two arguments into a function on pairs.

   **curry:**

   ```haskell
   curry' :: Num a => ((a, a) -> a) -> a -> a -> a
   curry' f = \x -> (\y -> x + y))
   ```

   Example:

   ```haskell
   add :: Num a => (a, a) -> a
   add (x, y) = x + y
   
   add (5, 7) -- 12

   add5 = curry' 5
   add5 7 -- 12
   ```

   **uncurry:**

   TODO

   ```haskell

   ```

   Hint: first write down the types of the two functions.

6. A higher-order function `unfold` that encapsulates a simple pattern of
   recursion for producing a list can be defined as follows:

   ```haskell
   unfold p h t x | p x       = []
                  | otherwise = h x : unfold p h t (t x)
   ```

   That is, the function `unfold p h t` produces the empty list if the predicate
   `p` is true of the argument value, and otherwise produces a non-empty list by
   applying the function `h` to this value to give the head, and the function
   `t` to generate another argument that is recursively processed in the same
   way to produce the tail of the list. For example, the function `int2bin` can
   be rewritten more compactly using `unfold` as follows:

   ```haskell
   int2bin = unfold (== 0) (`mod` 2`) (`div` 2)
   ```

   Redefine the functions `chop8`, `map f`, and `iterate f` using `unfold`.

   **chop8:**
   ```haskell
   chop8 :: [Bit] -> [[Bit]]
   chop8 = unfold (== []) (take 8) (drop 8)
   ```

   **map:**
   ```haskell
   map :: (a -> b) -> [a] -> [b]
   map = unfold
   ```

   TODO

   **iterate:**
   ```haskell
   iterate :: (a -> a) -> a -> [a]
   iterate = unfold
   ```

   TODO

7. Modify the binary string transmitter example to detect simple transmission
   errors using the concept of parity bits. That is, each eight-bit binary
   number produced during encoding is extended with a parity bit, set to one
   if the number contains an odd number of ones, and to zero otherwise. In
   turn, each resulting nine-bit binary number consumed during decoding is
   checked to ensure that its parity bit is correct, with the parity bit being
   discared if this is the case, and a parity error being reported otherwise.

   Hint: the library function `error :: String -> a` displays the given string
   as an error message and terminates the program; the polymorphic result type
   ensures that `error` can be used in any context.

   TODO

8. Test your new string transmitter from the previous exercise using a faulty
   communication channel that forgets the first bit, which can be modelled
   using the `tail` function on list of bits.

   TODO

9. Define a function `altMap :: (a -> b) -> (a -> b) -> [a] -> [b]` that
   alternately applies its two argument functions to successive elements in a
   list, in turn about order. For example: 

   ```haskell
   > altMap (+10) (+100) [0,1,2,3,4]
   [10,101,12,103,14]
   ```

   ```haskell
   altMap :: (a -> b) -> (a -> b) -> [a] -> [b]
   altMap f g []       = []
   altMap f g [x]      = [f x]
   altMap f g (x:y:zs) = f x : g y : altMap f g zs
   ```

10. Using `altMap`, define a function `luhn :: [Int] -> Bool` that implements
    the *Luhn algorithm* from the exercises in chapter 4 for bank card numbers
    of any length. Test your new function using own bank card.

    ```haskell
    luhn :: [Int] -> Bool
    luhn ns = (sum d) `mod` 10 == 0
      where
       d = foldr (\curr acc -> if curr > 9 then (curr - 9) : acc else curr : acc) [] e
       e = altMap (*2) (id) ns
    ```
