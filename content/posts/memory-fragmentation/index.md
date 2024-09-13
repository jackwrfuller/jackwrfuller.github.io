+++
title = "An Improved Memory Fragmentation Metric"
date = "2024-09-13"
+++

# An Improved Memory Fragmentation Metric?

As it turns out, there are not many resources online (nor articles discoverable on google scholar)
that discuss memory fragmentation metrics. The couple that I could find, namely Adam Sawicki's [blog
post](https://asawicki.info/news_1757_a_metric_for_memory_fragmentation) and [this Stackoverflow
article](https://stackoverflow.com/q/4586972), are about it.

So I decided to see if I could create my own.

# Some maths

Suppose we have some memory of size $M$. Then we can consider a _memory address_ to be any
non-negative integer $x$ such that $x \in [0,M)$. We can also consider an _allocation_ to be a tuple
$A_i = (x_i, l_i)$, where $l_i \in \mathbb{Z}_+$ is the _size_ of the allocation.

A _placement_ $P$  is then a set of $n$ non-overlapping allocations $\{A_i\}_{i=1}^{n}$ such that:

1.    $0 \le x_1 < x_2 < ... < x_n < M$
2.    $x_i + l_i \le x_{i+1}$ for all $i$

Let us define the set of all possible allocations as $\mathbb{P}$.

We are interested in measuring the fragmentation of an allocation, though what 'fragmentation' is
exactly has yet to be defined. For now, let us simply consider fragmentation to be a measure that
assigns some value between 0 and 1 to any allocation, i.e a function $\phi$ with  

$$\phi: \mathbb{P} \to [0,1]$$

where a placement $P$ is considered to be _maximally fragmented_ if $\phi(P)=1$ and _minimally
fragmented_ when $\phi(P)=0$.

While it is likely possible to define $\phi$ in terms of the allocations, it seemed easier to me to
think in terms of the free space around these allocations, i.e the available memory. A placement $P$
leaves a set of $n+1$ free spaces $\{f_i\}_{i=1}^{n+1}$, where

$$ f_1 = \lvert x_1 - 0 \rvert, \quad f_2 = \lvert x_2 - (x_1 + l_1) \rvert, \quad ..., \quad f_{n+1} = \lvert M - (x_n + l_n) \rvert $$

We can also define the total free space available after an allocation to be $F = \sum_{i=1}^{n+1}
f_i$.

Note that in my definitions, there is _always_ $n+1$ free spaces, even if two allocations are
contiguous - in that case, the free space between them is defined to be zero. 

So under these defintions, when would be consider a placement $P$ to be minimally fragmented? To me,
it seems clear that this is when all the allocations are contiguous in memory, leaving one big free
space. To put it concretely, when there is some $j$ such that $f_j = F$ and $f_i = 0$ whenever $i
\ne j$.

Conversely, I would consider a placement to be maximally fragmented when the free spaces are all the
same size, i.e $f_i = \frac{F}{n+1}$ for all $i$. Note that it is of course possible for the amount
of free space to not be divisible by the number of spaces, but I am ignoring this case to keep
things simple.

Given this setup, I propose the following measure of fragmentation, loosely based on the concept of
statistical variance:

$$ \phi(A) = 1 - \left( \frac{1}{F} \right) \left(\frac{n+1}{\sqrt{n(n+1)}} \right) \sqrt{\sum_{i=1}^{n+1} \left(f_i - \frac{F}{n+1}\right)^2 }$$

It is easily seen that this measure obeys the heuristics laid out above.

I took inspiriation for this formula from Adam Sawicki's often-referenced [blog
post](https://asawicki.info/news_1757_a_metric_for_memory_fragmentation), "A Metric for Memory
Fragmentation", where he defines something similar. He proposes many 'desirable properties' for a
fragmentation metric, even though the metric he proposes does meet all of them. Namely, the metric
he proposes never actually achieves $\phi(A) = 1$, despite it being demonstrably possible.
Furthermore, he asserts that the metric should not depend on the number of allocations made. I
disagree - if an allocator has made more, smaller, allocations to achieve the same free memory
layout, then it has performed better. After all, the whole point of measuring memory fragmentation
is to assess the performance of one allocator versus another. 

