# Bit Vectors
We define 3 operations on bitvectors
- Access(B, i): returns the element at pos $i$ in $B$
- Rank(B, i):   returns the number of 1's in the range $B[1, i]$
- Select(B, x): returns the position $i$ in which the rank becomes x, or $|B| +1$ if $Rank(B, n) < x$.

The book covers both the zero-order and k-th order compression of $B$

