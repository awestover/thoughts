
# warm up

Suppose you have a length $n$ vector that is initialized to
contain all zeros.

Process a sequence of $O(n)$ increment and decrement operations to
indices in the vector, which end with a single index being
non-zero, and then report the index storing the non-zero value.

Restrcition: Do this with $O(\log n)$ memory

Solution: 
For each $i\in [\lceil\log n\rceil]$, let $S_i$ denote the set of
indices $k\in [n]$ where the $i$-th bit of $k$ is a $0$.
For each such $i$, store $\sum_{x\in S_i}A[x]$.

# one level harder

Same problem as before, but remove the restriction that there is
only one non-zero value. 
Now your goal is to return any non-zero value. 

Same memory constraint.

Algorithm is allowed to be randomized.

Solution:
Apply the sketch from the warmup to random subsets of the
coordinates, with probabilities that are incremented in power of
two steps.

# Application

application to some parallel (?) graph problem.

#todo
