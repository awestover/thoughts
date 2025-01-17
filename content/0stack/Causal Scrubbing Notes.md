[Causal Scrubbing](https://static1.squarespace.com/static/6114773bd7f9917b7ae4ef8d/t/6364a036f9da3316ac793f56/1667539011553/causal-scrubbing) (CS)

A hypothesis about how a behavior is implemented is 
- A graph $I$ which is the "interpretation" of how the actual NN $G$ works.
- A function $c$ mapping nodes of $I$ to nodes of $G$.

Then we're going to somehow define a "scrubbed version" of $G$, which we'll call $G'_{I,c}$. 
And the difference between $\E_{d\sim D}[G'_{I,c}(d)]$ and $\E_{d\sim D}[G(d)]$ is a measure of how good of an explanation $I,c$ is.

The CS paper gives a simple example to illustrate how CS works.
The interpretation says that only $z_{1}$ matters for activation $A$. So, we can resample the input **just for A, not for anyone else** in such a way so that it supposedly won't change $A$'s activation. You can do this for all the nodes. You can also replace supposedly unimportant information however you want. 

Algorithm:
```python
x = sample(D)

def run_scrub(nI, c, D, refx):
  nG = c(nI) # activation corresponding to nI
  if nG is an inputnode:
    return refx
  inputsG = {}
  randomx = sample(D)

  for parentG in nG.parents():
    if parentG in c(nI.parents()): # important parents
	   newx = sample_agreeing_x(D, nI, refx)
	   inputsG[parentG] = run_scrub(parentI, c, D, newx)
    else
     inputsG[parentG] = parentG.value_on(randomx)

return nG.value_from_inputs(inputsG)
```

Observe: 
This algorithm is hopelessly inefficient in general as written. 
However, it might be amenable to dynamic programming, which could make it efficient. 
Or maybe there's some "early stopping" --- i.e., you don't fully scrub it but just unroll a couple levels of recursion.

Also, apparently rewriting the circuit is kind of problematic. Maybe it's because there are lots of ways that you could do this?

**Challenges**
Maybe there is some "irreducibility" --- the thoughts of a powerful AI can't be simplified / it's hard to find human understandable explanations for them.

**1: False hypotheses which make correct predictions**
- neglecting both helpful and anti-helpful mechanisms?

**2: Underestimating interference by neglecting correlations**
Obs: if you have an error term, and you split it and forget that the two things are correlated, then this decreases the variance of your error.
I guess this is bad. 

In general, if you pick up some slack somewhere like this, then it's possible to "spend" it later, making the situation worse somehow. 

**3: Sneaking in knowledge**
I didn't get their example, but I feel like the vibes are that you could have an
explanation that is very good for dumb reasons. Like you could just go compute the answer yourself or something?

#technical 