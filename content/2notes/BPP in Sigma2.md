Here's a classic result in complexity theory:

> $\mathsf{BPP} \subseteq \Sigma^{2}$.

Why is this the case?

BPP is the set of languages for which there is an algorithm $A$, which takes as input a random seed of length $n^{2}$ in addition to an input of length $n$, such that:
- For any true statement $\phi$, for at least a $1-2^{-n}$ fraction of the $2^{n^{2}}$ total random seeds, the algorithm on random seed outputs YES. 
- For any false statement $\phi$, $A$ outputs YES on at most a $2^{-n}$ fraction of the random seeds. 

How to distinguish between the case that there are many random seeds on which the algorithm accepts $\phi$ and the case that there are very few?

Key observation: 
- If $S \subseteq \{0,1\}^{m}$ has measure very close to $1$, then there exists a small set of offsets $R$ such that $S+R$ covers the entire hypercube. 
- If $S \subseteq \{0,1\}^{m}$ has measure very close to $0$, then there is no small set of offsets $R$ such that $S+R$ covers the entire hypercube. 

---
More precisely, how many shifts of a set of measure $1-\varepsilon$ do we need before it covers the entire hypercube? Well, if I take a random shift, the probability that any particular point is not covered is at most $\varepsilon$. If I take $k$ many shifts, the probability that some particular point is not covered by any of these shifts is at most $\varepsilon^{k}$. The probability that there is any point which is not covered is at most $2^{n^{2}} \varepsilon^{k}$ by a union bound.

For our particular case, setting $k = n^{5}$ we have:
$$
2^{n^{2}} 2^{-n \cdot n^{5}} < 1, 
$$
so by the probabilistic method there exists a set of shifts such that the entire hypercube is covered. 

However, it's also the case that in the NO case, this set of shifts will still definitely not cover the hypercube. 

So here's the $\Sigma^{2}$ formula corresponding to the BPP problem:

$$\exists \text{ a set $S$ of $n^{5}$ shifts }: \forall \text{ random seeds }r\in\{0,1\}^{n^{2}}, \bigvee_{s\in S} \mathcal{A}(x, s+r). $$
In less fancy language: 
> There is a small set of shifts such that for every seed, you can shift the random seed into a random seed where the algorithm will work.

This is true for YES instances in BPP and false for NO instances in BPP.

