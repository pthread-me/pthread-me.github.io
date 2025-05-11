# Zero-order Compression

Given a bit vector $B$, we divide it into blocks of size $b$, s.t $B_i = B[(i-1)b+1, ib]$.
Every block $B_i$ is assigned a class $c_i$ in which the class refers to the number of $1's$ in $B_i$.

Observe that a class $c_i = c$ can represent $\binom{b}{c}$ blocks.

#### Example
> Let $b=4$ then the class $c=0$ represents the block $\{0000\}$.
> While $c=1$ represents the blocks $\{0001, 0010, 0100, 1000\}$.

Notice that just the variable $c_i$ is not enough to descibe $B_i$, thus an offset 
variable $o_i$ is used to identify which of the possible blocks in $c_i$ referes to $B_i$.

## Assigning Offsets
Given that we have $\binom{b}{c}$ starting combinations to encode offsets for, observe that:
- There are $\binom{b-1}{c}$ permutations of blocks that start with 0.

- And $\binom{b-1}{c-1}$ permutations of blocks that start with 1.

Using this, the book describes an algorithm to assign $o$ to block $B$ of class $c$
- aaaa123

