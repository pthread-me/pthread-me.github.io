# Closed Form

Using the recurrence:
> $J(1) = 1$
>
> $J(n) = 2 \cdot J(n) -1, n = 2m$
>
> $J(n) = 2 \cdot J(n) +1, n = 2m+1$

We can start solving:

|n|1|2|3|4|5|6|7|8|9|10|11|12|13|
|-|-|-|-|-|-|-|-|-|-|-|-|-|-|
|J(n)|1|1|3|1|3|5|7|1|3|5|7|9|11|

A pattern emerges in which the values repeat in windows of size the powers of 2, where
each element in the window is incremented by 2.
For example the window covering the values of n, 8 to 15, is of size $2^3$
