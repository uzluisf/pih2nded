# Chapter 04

1. Using library function, define a function `halve :: [a] -> ([a], [a])` that
   splits an even-lengthed list into two halves. For example:

```haskell
> halve [1,2,3,4,5,6]
([1,2,3], [4,5,6])
```

Using the `splitAt` function introduced in this chapter:

```haskell
halve xs = splitAt (length xs `div` 2) xs
         where
             splitAt n xs = (take n xs, drop n xs)
```

2. Define a function `third :: [a] -> a` that returns the third element in a
   list that contains at least this many elements:

* using `head` and `tail`;
```haskell
third :: [a] -> a
third xs = head (tail (tail xs))
```

* list indexing;
```haskell
third :: [a] -> a
third xs = xs !! 2
```

* pattern matching.
```haskell
third :: [a] -> a
third [_,_,x] = x
third [_,_,x,_] = x
```

It's implemented as follows in the Appendix A.4:

```haskell
third :: [a] -> a
third (_:_:_x:_) = x
```

**Note:** The advantage of this implementation is the last `_` can be `[]` so
there's no need to take care of a list with only three items, as I had to do in
my implementation using the less generalized list patterns.

3. Consider a fuction `safetail :: [a] -> [b]` that behaves in the same way as
   `tail` except that it maps the empty list to itself rather than producing an
   error. Using `tail` and the function `null :: [a] -> Bool` that decides if a
   list is empty or not, define `safetail` using:

a. a conditional expression;
```haskell
safetail :: [a] -> [a]
safetail xs = if null xs then xs
              else tail xs
```

b. guarded equations;
```haskell
safetail :: [a] -> [a]
safetail xs
    | null xs   = xs
    | otherwise = tail xs
```

c. pattern matching.
```haskell
safetail :: [a] -> [a]
safetail [] = []
safetail (x:xs) = xs -- safetail xs = tail xs
```

4. In a similar way to `&&` in section 4.4, show how the disjunction operator
   `||` can be defined in four different ways using pattern matching.

```haskell
-- first way
(||) :: Bool -> Bool -> Bool
False || False = False
False || True = True
True || False = True
True || True = True

-- second way
(||) :: Bool -> Bool -> Bool
False || False = False
_     || _     = True

-- third way
(||) :: Bool -> Bool -> Bool
False || b = b
True  || _ = True

-- fourth way
(||) :: Bool -> Bool -> Bool
b || c
    | b == c    = b
    | otherwise = True 
```

5. Without using any other library functions or operators, show the meaning of
   the following pattern matching definition for logical conjunction `&&` can be
   formalised using conditional expressions:

```haskell
True && True = True
_    && _    = False
```

Implementation:

```haskell
(&&) :: Bool -> Bool -> Bool
b && c = if b == True then
            if c == True then True else False
         else False
```

Hint: use two nested conditional expressions.

6. Do the same for the following alternative definition, and note the difference
   in the number of conditional expressions that are required:

```haskell
True && b  = b
False && _ = False
```

Implementation:

```haskell
(&&) :: Bool -> Bool -> Bool
b && c = if b == True then c else False
```

7. Show how the meaning of the following curried function definition can be
   formalised in terms of lambda expressions:

```haskell
mult :: Int -> Int -> Int -> Int
mult x y z = x * y * z
```

Implementation:

```haskell
mult :: Int -> Int -> Int -> Int
mult = \x -> (\y -> (\z -> x * y *z))
```

8. The *Luhn algorithm* is used to check band card numbers for simple errors
   such as mistyping a digit, and proceeds as follows:

* consider each digit as a separate number;
* moving left, double every other number from the second last;
* subtract 9 from each number that is now greater than 9;
* add all the resulting numbers together;
* if the total is divisible by 10, the card number is valid.

Define a function `luhnDouble :: Int -> Int` that doubles a digit and subtracts
9 if the result is greater than 9. For example:

```haskell
> luhnDouble
3
> luhnDouble 6
3
``` 

Using `luhnDouble` and the integer remainder function `mod`, define a function
`luhn :: Int -> Int -> Int -> Int -> Bool` that decides if a four-digit bank
card number is valid. For example:

```haskell
> luhn 1 7 8 4
True
> luhn 4 7 8 3
False
```

Implementation:

```haskell
luhnDouble :: Int -> Int
luhnDouble n = if doubled > 9 then doubled - 9 else doubled
               where
                   doubled = 2 * n

luhn :: Int -> Int -> Int -> Int -> Bool
luhn w x y z = (luhnDouble(w) + x + luhnDouble(y) + z) `mod` 10 == 0 
```
