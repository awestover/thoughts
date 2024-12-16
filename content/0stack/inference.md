I'm tentatively planning to grind through MIT's "inference" courses. 
$$ \newcommand{\dep}{\perp\!\!\!\perp}  $$

Specifically, this is 
- algorithms for inference
- inference and information

These cover some things like 
- graphical models
- information theory
- inference 
- information geometry
- thinking about structure of random variables and independence and stuff
-  probably other stuff too

These topics seem 
1) pretty intellectually interesting
2) the most relevant stuff to ARC's research agenda (where I'll be interning over the summer)

I reserve the right to quit if I find some cool research projects / make up some cool research questions myself. 
However I don't anticipate quitting currently. 

I'll record my journey of doing this here. 
I'll also tentatively plan to make this into a dialogue because I love JJ+shatar+blobby, but right now I just need to write some stuff down.

**Day 0 -- Dec 12:** 
Likelihood ratio test: 
If you have two hypothesis, and want to select one to minimize the probability that you select the wrong one, then you should compute Pr(hypothesis X is true given the data) and choose whichever hypothesis maximizes this chance. 
This assumes that you have some priors on the chance of the hypothesis being correct, and that under each hypothesis you can compute Pr(data).

**Day 1 -- Dec 13** 
Read the first couple Alg for Inf notes. 
First some definitions, then a neat theorem.

**Directed Graphical Model**
![[Pasted image 20241214082103.png]]
In a directed graphical model, we can factor the probability distribution into terms of the form 
$p(x_i\mid x_{\text{parents of xi in the graph}})$. So any distribution obeys the conditional independence implied by the complete graph (which is not so impressive since that's the empty set).

But my digraph implies more independences. For instance, $x_1 \dep x_3 \mid x_{2}$ based on my picture.

The general hope for this class is that if we have a sparse graph then we can do much much more efficient things than are possible in the dense case. 

A general algorithm for listing off some conditional independences is:
- topo-sort the DAG
- $x_i$ is independent of non-parent things before it in the topo-order, conditional on $x_i$'s parents

**Bayes Ball Algorithm**

#todo