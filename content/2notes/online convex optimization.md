A couple of quick notes from Elad Hazan's "Introduction to Online Convex Optimization" (OCO).

I suspect the main takeaway is that, if no particular action of a NN can be too bad, (i.e., utility function is bounded) then SGD achieves bounded regret. -- idea is from Paul's blog. this corresponds to "low-stakes" alignment, and suggests that for low-stakes alignment, defining a good objective function is all you need. 

Best to introduce this via some examples.

**ex: binary version of expert advice problem**

we will have $T$ iterations of a binary decision -- choosing either $A$ or $B$. 
before each decision we'll get to hear $N$ expert opinions.
the adversary will get to choose which of $A,B$ is the right answer afterwords. 
we get a score of $0$ for wrong answer, $1$ for right answer.

> we'll assume $\log N \ll T$ ($\log N>T$ is stupid -- you can just have experts that say every possible combination), and I'll probably even think of $T\gg N$.

suppose that the best expert makes at most $L$ mistakes over the course of the $T$ rounds. 

can we make at most $O(L + \log N)$ mistakes?

yes. we can even do $L+O(\sqrt{ T\log N })$.
we call this "regret $O(\sqrt{ T \log N })$".


---

**claim 1**

No deterministic algorithm guarantees regret less than $L$.

**pf**

sps there are 2 experts. 
one always says $A$ the other always says $B$.

then $L\leq T/2.$

but we are deterministic so the adversary can choose the correct one of $A,B$ to be whichever we didn't choose. 


**claim 2**

There is a deterministic algorithm with regret $O(L + \log N)$.

**pf -- Weighted Majority alg**

each expert starts with weight $W_1(i)=1$.
an experts weight decays by $1/2$ whenever the expert answers incorrectly, and doesn't change if they answer correctly. 

let $M_t(i)$ denote the number of mistakes of expert $i$ up to time $t$. 
then, 
$$
W_t(i) = 2^{-W_t(i)}.
$$
Let $M_t$ denote the number of mistakes that we make up to time $t$.


we make choices by looking at the total weight of experts that select A vs the total weight on B.

let $\phi_t = \sum_i W_t(i)$

observe that whenever we make a mistake, we must have
$$
\phi_{t+1} \leq (3/4) \phi_t.
$$
we have the following:

$$
2^{-M_t(i)} = W_t(i) \leq \phi_t\leq N (4/3)^{-M_t}.
$$
Re-arranging this gives
$$
M_t \le O(M_t(i) + \log N).
$$
as desired.


**claim 3**

there is a randomized algorithm with regret $O(\sqrt{ T\log N })$

(i suspect you can even get $O(\sqrt{ L\log N })$ via standard dyadic tricks, but I haven't thought about it yet)

**pf**

Fix $\varepsilon$ (we'll take it to be $\sqrt{ (\log N)/T }$).

Now we update weights by multiplying by $(1-\varepsilon)$.

We choose an expert by sampling one according to their weight.

We do pretty similar things but with some more careful bounding to get the desired bound. 


