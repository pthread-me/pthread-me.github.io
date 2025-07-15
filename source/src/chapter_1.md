# The Josephus Problem

Given a sequence of numbers $1..n$ , we want to delete every other number,
until one remains, such that: $J(n) = k$ where $k$ is the remaining number

### Example
>   Given n=12 we have:
>       1,2,3,4,5,6,7,8,9,10,11,12
>
>   Walking through the list we have:
>       1, ~2~, 3, ~4~, 5, ~6~, 7, ~8~, 9, ~10~, 11, ~12~
>   
>   Observe that the indicies (in our case the values) greater than 1, change such that 
>   the number at index 2, which was 2 became 3, so $i \rightarrow 2i-1$
>
>   Continuing with cycle of deletions we have:
>
>   1, ~3~, 5, ~7~, 9, ~11~
>
>   1, ~5~, 9
>
>   ~1~, 9


By taking another example where n is odd, convince yourself that the change in values is $2i +1$ as 
we loop back and cancel the starting $1$.

Observe now that $J(2m)$ all we need to do is solve for the sequence we get after the first round of canceling
so when sovling for $1..2m$ we solve for $1..m$ where each element is $2i-1$.

That is:
$$ J(2m) = 2 \cdot J(m) -1 $$

Similarly with $2m+1$:
$$ J(2m+1) = 2 \cdot J(m) + 1$$

And we know the base case $m=1$ has the solution 
$$J(1) = 1$$

The 3 equations form the recurrence for solving the problem
