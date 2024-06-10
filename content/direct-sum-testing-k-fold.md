In this note I summarize a paper by Dinur et al on direct sum testing, in a different sense than in [[direct-sum-testing-grid]].

We say that $f: \binom{[n]}{k}\to \mathbb{F}_2$ is a **$k$-fold direct sum** if 
there exists $a\in \mathbb{F}_2^n$ such that 
$$f(x) =\sum_{i\in S} a_i.$$

> [!tip] Result: 
> three query test: 
> accept if is kfold-dir-sum, 
> if  $\epsilon$-far then reject with probability at least $\eps/1000$. 

Equialent formulations:
- linearity testing on the $k$-slice of the hypercube. 
- "essentially" tensor power testing

>[!bug] Q:
>What is meant by tensor power testing here?

- they also have a result about checking if a $\pm 1$ matrix has rank $1$. 



>[!bug] Q
> What should we know about Haddamard code? 
>Reed-Muller code?
>[book about coding theory](https://www.sciencedirect.com/bookseries/north-holland-mathematical-library/vol/16)

## open questions

one of them has been resolved https://arxiv.org/abs/2402.05217