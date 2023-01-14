# Chapter 01

1. Give another possible calculation for the result of `double (double 2)`.

Calculations given in the book:

```haskell
double (double 2)
= double (2 + 2)
= double 4
= 4 + 4
= 8

double (double 2)
= double 2 + double 2
= (2 + 2) + double 2
= 4 + double 2
= 4 + (2 + 2)
= 4 + 4
= 8
```

Another possible calculcation:

```haskell
double (double 2)
= double 2 + double 2
= double 2 + (2 + 2)
= double 2 + 4
= (2 + 2) + 4
= 4 + 4
= 8
```

2. Show that `sum [x] = x` for any number `x`.

```haskell
sum [x]
= x + sum []
= x + 0
= x
```

3. Define a function `product` that produces the product of a list of numbers,
   and show using your definition that `product [2, 3, 4] = 24`.

```haskell
product [] = 1
product (n:ns) = n * product ns
```

Using `product`:
```haskell
product [2, 3, 4]
= 2 * product [3, 4]
= 2 * (3 * product [4])
= 2 * (3 * (4 * product []))
= 2 * (3 * (4 * 1))
= 24
```

4. How should the definition of the function `qsort` be modified so that it
   produces a *reverse* sorted version of a list?

In the book `qsort` is defined as follows:

```haskell
qsort [] = []
qsort (x:xs) = qsort smaller ++ [x] ++ qsort larger
               where
                   smaller = [a | a <- xs, a <= x]
                   larger  = [b | b <- xs, b > x] 
```

where the result list is sorted in increasing order. In order to produce sorted
list in decreasing order, we must simply swap `smaller` and `larger`. Thus,
`qsort larger ++ [x] ++ smaller`.

5. What would be the effect of replacing `<=` by `<` in the original definition
   of `qsort`? Hint: consider the example `qsort [2, 2, 3, 1, 1]`.

If we replace `<=` with `<`, the sorted list has no duplicates. For example:

```haskell
qsort [2, 2, 3, 1, 1]
= qsort [1] ++ [2] ++ qsort [3]
= (qsort [] ++ [1] ++ qsort []) ++ [2] ++ (qsort [] ++ [3] ++ qsort [])
= [1] ++ [2] ++ [3]
= [1, 2, 3]
```
