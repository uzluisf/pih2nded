# Chapter 02

1. Work through the examples from this chapter using GHCi.

2. Parenthesise the following numeric expressions:

```haskell
2 ^ 3 * 4     -- (2 ^ 3) * 4

2 * 3 + 4 * 5 -- (2 * 3) + (4 * 5)

2 + 3 * 4 ^ 5 -- 2 + (3 * (4 ^ 5))
```

3. The script below contains three syntactic errors. Correct these errors and
   then check that your script works properly using CHCi.

```haskell
N =  a `div` length xs
     where
         a = 10
        xs = [1, 2, 3, 4, 5]
```

* The `xs` statement isn't properly indented (*layout rule*).
* Variable doesn't start with a lowercase letter, i.e., `N`.
* After fixing these two errors, the script worked with `GHCi version 9.0.2`.

4. The library function `last` selects the last element of a non-empty list; for
   example, `last [1,2,3,4,5] = 5`. Show how the function `last` could be
   defined in terms of the other library functions introduced in this chapter.
   Can you think of another possible definition?

Version 1:

```haskell
last xs = xs !! (length xs - 1)

-- Application
last [1,2,3,4,5]
= [1,2,3,4,5] !! (length [1,2,3,4,5] - 1)
= [1,2,3,4,5] !! (5 - 1)
= [1,2,3,4,5] !! 4
= 5
```

Version 2:

```haskell
last xs = head (drop (length xs - 1) xs)

-- Application
last [1,2,3,4,5]
= head (drop (length [1,2,3,4,5] - 1) [1,2,3,4,5])
= head (drop (5 - 1) [1,2,3,4,5])
= head (drop 4 [1,2,3,4,5])
= head ([5])
= 5
```

5. The library function `init` removes the last element from a non-empty list;
   for example `init [1,2,3,4,5] = [1,2,3,4]`. Show how `init` could similarly
   be defined in two different ways.

Version 1:

```haskell
init xs = take (length xs - 1) xs

-- Application
init [1,2,3,4,5]
= take (length [1,2,3,4,5] - 1) [1,2,3,4,5]
= take (5 - 1) [1,2,3,4,5]
= take 4 [1,2,3,4,5]
= [1,2,3,4]
```

Version 2:

```haskell
init xs = reverse (drop 1 (reverse xs))

-- Application
init [1,2,3,4,5]
= reverse (drop 1 (reverse [1,2,3,4,5]))
= reverse (drop 1 [5,4,3,2,1])
= reverse [4,3,2,1]
= [1,2,3,4]
```
