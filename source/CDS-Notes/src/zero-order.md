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

### Algorithm 1 
> Encoding offsets:
>- Set $o=0$
>
>- For $B'=B[i]$, if:
>   - $B' = 0$, then set $b=b-1, c=c, o=o, B=B[i+1..n]$; That is do nothing and skip over the 0's
>
>   - $B' =1$, then set $o = o+ \binom{b-1}{c}$ and set $b=b-1, c=c-1, B=B[i+1..n]$.
> 
> Note $\binom{x}{y}$ when x<y=0

### Example - encoding
> $B$ = 1011
> So starting with $b=4, c=3, o=0$
>
> -  $B'=1$; so $o= o+ \binom{3}{3} = 0 + 1 = 1$; and set $b=3, c=2$
>
> -  $B'=0$; so $o= o$; and set $b=2, c=2$
>
>
> -  $B'=1$; so $o= o+ \binom{1}{2} = 1 + 0 = 1$; and set $b=1, c=1$
>
> -  $B'=1$; so $o= o+ \binom{3}{3} = 1 + 0 = 1$; and set $b=0, c=0$
>
> So $B=1011$ is represented by the pair $(c, o) = (3,1)$

### Algorithm 2
> Decoding offsets:
>
> The operations is the inverse of encoding, so given  $b,c,o$:
> - if $o < \binom{b-1}{c}$ then $B'$ is a 0.
>   And we set $b=b-1, c=c, o=0$
>
> - else $B'=1$ and we set $o=o-\binom{b-1}{c}$ then $b=b-1, c=c-1$


### Example - decoding
> Given $b=4, c=3, o=1$
>   - $o=1 \ge \binom{4-1}{3} = 1$; So $B' =1$  ---->    $b=3, c=2, o=0$
>   - $o=0 < \binom{3-1}{2} = 1$; So $B' = 0$
> ----> $b=2, c=2, o=0$
>   - $o=0 \ge \binom{2-1}{2} =0$; So $B' =1$
> ----> $b=1, c=1, o=0$
>   - $o=0 \ge \binom{1-1}{1}=0$; So $B'=1$
> 
> Finally if $c=0$ we can set the remaining $b$ bits to 0, and $b=0$ is the exit condition.

## Structures:
The compressed version of a bitvector $B$ includes 3 structures:

### Class array
A $C=[1, \lceil n/b \rceil ]$ array holding the elements of size $\lceil log_2(b+1) \rceil$
    
So:
    $$\lceil n/b \rceil \times log_2(b+1)$$
    
We then use the taylor series approximation of $log(b+1) \approx log(b) + log(1+\frac{1}{b})$
To get:
    $$ \lceil n/b \rceil \times log_2(b+1) \approx \lceil n/b \rceil \times log_2(b) + \lceil n/b \rceil \times log_2(1+\frac{1}{b})$$


Some Big O abuse allows us to ignore the ceil since it adds a $\mathcal{O}(1)$ factor. and to ignore the 
    $log_2(1+\frac{1}{b})$ since it is strictly less that $1$ for any $b>1$. Thus:
    $$\lceil n/b \rceil \times log_2(b+1) \in (n/b)\times log_2(b) + \mathcal{O}(n/b)$$


### Offset array
Similar to the class array, we have $\lceil n/b \rceil$ elements, each of size $|o_i| = \lceil log_2(\binom{b}{c})  \rceil$.

That is the total size:
$$\sum_{i=1}^{\lceil n/b \rceil} \lceil log_2{\binom{b}{c_i}} \rceil $$

We know that removing the ceiling adds a $\mathcal{O}(1)$ factor, thus over the sum we get.
$$ \lceil n/b \rceil + \sum_{i=1}^{\lceil n/b \rceil} log_2(\binom{b}{c_i})$$

#### Lemma 1
> We know that $log(a) + log(b) = log(ab)$ so we can remove the log from the sum by replacing the summation of $log(i)$
> with $log(\prod_{i=1}^n i)$

Using the above lemma 2 we get that:
$$ \lceil n/b \rceil  + log_2 \prod_{i=1}^{\lceil n/b \rceil}{\binom{b}{c_i}}$$

#### Lemma 2
> Given some values $b,c,c'$ the book states that
> $$\binom{b}{c} \cdot \binom{b}{c'} \le \binom{2b}{c+c'}$$

#### Proof (non-formal)
> Observe that given a bitvector of size $b$, any combination $\binom{b}{c}$ will also appear in a 
> bitvector of size $2b$. If we treat $2b$ as $2$ seperate vectors placed beside each other then naturally the left $b$ bits
> will have exactlt the same number of combinations $\binom{b}{c}$ and the right side can hold exactly the same number of combinations
> $\binom{b}{c'}$ w.l.o.g of which side of the $2b$ vector is used for $c$ and $c'$.
>
> It follows then that the $2b$ vector can represent $\binom{b}{c} \cdot \binom{b}{c'}$, if we remove the restriction
> of having it be cut in the middle and instead be used as a continuoes block then the $2b$ vector can represent MORE than 
> the product of what each of it's divided sides can. Thus
>
> $$\binom{b}{c} \cdot \binom{b}{c'} \le \binom{2b}{c+c'}$$
> Im prob not doing a good job of exmplaining it, but i get it, trust.


Now, let $m$ to be the total number of $1's$ in $B$ (so its the $c+c'+...$) and $n$ 
to total number of $b$ blocks. And using Lemma 2, we have it that:
$$\lceil n/b \rceil + log_2\binom{n}{m}$$

FINALLY, recall from the entropy chapter that the zero-order (aka worst case) entropy of a universe $U$ is $log|U|$.
so the size of the offset vector $O$ is bounded by ($B$ is our universe in this case):
$$|O| = n\mathcal{H}_o(B) + \lceil n/b \rceil$$

## Lookup Table
We also need a lookup table $K$ for all the combinatoric values $\binom{i}{j}$ for $0 \le j \le i \le b$ which comes up to $b^2$
(I remember the C++ creator talking about triagular matrices, so a $b^2$ upper bound is not really tight but its big O so who cares ig)

## Bringing the structures together
We have $C, O, K$ with a combines size of 
$$(n/b \cdot log_2(b) + \mathcal{O}(n/b)) + (n\mathcal{H}_o(B) + \lceil n/b \rceil) + \mathcal{O}(wb^2)$$

Which with more Big O magic gives the bound:
$$n\mathcal{H}_o(B) + log_2(b) + \mathcal{O}(n/b + wb^2)$$
