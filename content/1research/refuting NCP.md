ARC is currently pretty excited about something called "no coincidence principle".

My complexity theory expert friend N says that "NCP basically sounds like you're trying to show
$\mathsf{coNP}\subseteq \mathsf{NP}$" (paraphrased).

I'm fairly sympathetic to this view. 

My view is that NCP should be true for many "squishy systems" (roughly "compressible") for "dumb reasons" but shouldn't be true in general.

Let's gather some evidence.$\newcommand{\weird}{\mathsf{weird}}$

---

EDIT -- these "don't count"

**NCP Conjecture 1** 
Define the weirdness property $\weird(C)\in \{0,1\}$ to be "$C(x)\neq \vec{0} \quad\forall x$".

Let $D$ be a polynomial-time-sample-able distribution over $n^{2}$ gate circuits $C:\{0,1\}^{2n}\to \{0,1\}^{n}$ such that $\Pr_{C\sim D}[\weird(C)]<2^{-n}$.

Then, there should be an efficient weirdness detector $W$ (it can depend on $D$) such that:
- If $\weird(C)$, then there exists $\pi$ such that $W(C,\pi)=1$.
- If $C\sim D$, then $\Pr[\exists \pi\mid W(C,\pi)]<.001$.

**NCP Conjecture 2**
Define the weirdness property $\weird(C)\in \{0,1\}$ to be "$\phi$ is unsatisfiable".

Let $D$ be a polynomial-time-sample-able distribution over $n^{2}$ clause CNF SAT formulas such that $\Pr_{C\sim D}[\weird(C)]<2^{-n}$.

Then, there should be an efficient weirdness detector $W$ (it can depend on $D$) such that:
- If $\weird(C)$, then there exists $\pi$ such that $W(C,\pi)=1$.
- If $C\sim D$, then $\Pr[\exists \pi\mid W(C,\pi)]<.001$.


Related thing that we can easily rule out:

**Something impossible**
Efficient $W$ such that 
- $\forall\phi\in \mathsf{unsat}$, $\exists \pi$ such that $W(\phi,\pi)$.
- $\forall \phi\in\mathsf{sat}$, $\not\exists \pi$ such that $W(\phi,\pi)$.


Some other things that seem probably true: 

**probably 1**
If $\phi\sim \mathsf{3CNF}(4n)$ (4n clauses) then 
$$
\Pr[\phi \in \mathsf{SAT}] > 1-1/1.1^n
$$
BUT 
it's like "NP-hard vibes" to find a SAT assignment.


**suspicion**
If $\phi\sim \mathsf{3CNF}(6n)$ then 
$$
\Pr[\phi \in \mathsf{SAT}] < 1/1.1^n
$$
BUT 
it's like "coNP-hard vibes" to prove this.

[in fact,...](https://homes.cs.washington.edu/~beame/papers/stoc2plusp.pdf) it is known that it usually requires exponentially long "refutation" proofs.  

of course this doesn't rule out the existence of some other shorter type of proof but it doesn't seem good. 

---
---

ah, according to Jacob -- 
my assumptions here are not really kosher. 

what you're supposed to do is give a "heuristic argument" for why the probability is really low, but then it's not actually supposed to be that low (or else you can union bound). 
and then you're supposed to do it. 

like if we're forall quantifying over things

yeah yeah

hmm.

---

ok in Jacob's example the heuristic that he used was something like 

Pr deviating by more than $k\sigma$ from $\mu$ is at most $\exp(-k^{2})$ or something.

So maybe I need some setup where I use this heuristic to estimate the number of something, but then this heuristic didn't apply super well?

----
---

yeah atm I don't get why NCP isn't just true by union bound. sigh.

lets try something else.

---
---
---

Here's some conjecture:

> For a reversible circuit C: {0, 1}^{3n} --> {0, 1}^{3n}, let phi(C) be the property "for all x \in {0, 1}^{2n}, C(0^n ∘ x) does not start with n zeros". There exists a polynomial-time (maybe even linear-time) verifier V(C, pi) such that:
> For all C such that phi(C) = 1, there exists pi such that V(C, pi) = 1.
> For a randomly selected circuit C such that phi(C) = 0, the probability that there exists pi such that V(C, pi) = 1 is <1%.

this is kind of annoying to work with, so I'd love to come up with a cleaner version of it

I guess you can cash this out as having some CNOT gates.


---
some thoughts:

Planted clique: 

magically having a random clique is quite unlikely. 
but there's an obvious structure you can point to to prove existence of planted clique. 

Suppose I have some function $W:\{0,1\}^{n}\to \{0,1\}$ with
$\mathbb{E}[W(x)] =  .01/n$ and $\mathsf{Var}[W(x)] \approx .01/n$.
If $W(x)$ were Gaussian with this mean and variance, then the
probability of having $W(x)\ge 1$ (i.e., $\approx 10\sqrt{n}$
standard deviations above the mean) is at most $1/3^{n}$, so it
will never happen on any of the $2^{n}$ input strings.

Thus, if $W(x)$ actually is $1$ for some $x$, then there must be
a structure in $x$ which explains why. 

---


next candidate (haven't thought about whether or not it's trivially bad yet)

Suppose $x_{1},\dots,x_n$ are independent $\mathsf{Ber}(1,1/\sqrt{n})$'s.
Let $S\subseteq\binom{[n]}{2}$ be a size $N$ set of $(i,j)$ pairs.
Define $f(x) = \sum_{(i,j)\in S} x_i x_j$.
Suppose that $f$ has standard deviation $N/n$. 
Then, any $x$ with $f(x)>100N/\sqrt{ n }$ demands an explanation.

----

Hi Jacob, 
Comment about your triangle NCP writeup:

You say 
> (linear in number of edges time is...) enough time to read in the definition of the graph as a list of edges, but not enough time to count the triangles directly

I think this is kind of questionable. 
More precisely, there is an algorithm $V(G)$ that runs in time $\widetilde{O}(n^{2})$ (doesn't need an argument $\pi$) with the following properties:
- If $G$ has more than $n^{1.51}$ triangles,  then $V(G)=1$.
- If $G\sim \mathbb{G}(n, 1/\sqrt{ n })$ then $\Pr[V(G)=1]< o(1)$.

The algorithm is as follows:
```
for each vertex v:
   if degree(v) > 10root(n) log n, then just output 1, because this is pretty weird
   else:
       there are at most O(n log^2 n) pairs of neighbors of v
       count how many of these are triangles, add to a global count
```

---

I think something interesting is still going on with your pigeon argument though. 
Here's my recommendation for what you could say instead. 

There is a (randomized) algorithm $V$ that is fed an adjacency matrix representation of a graph $G$ and an $n^{.76}\log n$ bit proof $\pi$, such that $V$ reads at most $n^{.25}\log ^{2} n$ bits of $\pi,G$ and such that $V$ satisfies:
- If $G$ has at least $n^{1.76}$ triangles, then there is some proof $\pi$ such that $V(G,\pi)=1$.
- If $G\sim \mathbb{G}(n,1/\sqrt{ n })$, then for any $\pi$, $\Pr[V(G,\pi)]<o(1)$.

**proof:** 
$V$ expects to be fed a list of $n^{.76}$ triangles that all contain some vertex $v_{0}$.
$V$ checks a couple of random locations in the proof $\pi$ to make sure that they're actually pointing to legit triangles.

Then, we check to make sure that we don't actually get fed the same triangle multiple times :P.

---
I think this does a better job of highlighting how useful the "argument" $\pi$ is.
I'm pretty confident you can't get $\mathsf{polylog}(n)$ time for an algorithm without this "argument" $\pi$.

EDIT: 
no actually this is kind of silly / not the case that is hard for NCP. 

because it has NP vibes not coNP vibes.

---

Suppose that $S\subseteq [n^{3}]$ and we're guaranteed that either $|S|<n^{.51}$ or $|S|>n^{.52}$.
There is a Verifier that, given a length $n^{.52}$ proof, will read $O(\log ^{2} n)$ bits of the proof and:
- An honest Prover can write down a proof of $|S|>n^{.52}$ that the Verifier will always accept.
- An evil Prover with $|S|<n^{.51}$ cannot write down any proof that has probability more than $o(1)$ of tricking the Verifier into accepting. 

The Prover writes down the elements $x_{1},x_{2},\dots,x_{n^{.52}}$ of $S$ in **sorted order**.
The Verifier looks at a random sequence of the elements as follows:
- First, look at number $|S|/2$.
- Then, randomly choose one side $.25 |S|$ or $.75 |S|$ to recurse on.
- Then you'll ask something like $|S|\cdot 3/8$. etc.Claim
At the end you'll just check that these elements are all in $S$ and are in the correct order. 

Clearly the honest Prover can write down a proof that we'll be happy about.

	**Claim**: the probability that an evil Prover wins is very low. 

**Proof**:
Let $L_{0}=S$. We have $|L_{0}|< n^{.51}$.
Let $L_{1}$ be either the things larger than or less than $x_{n^{.52}/2}$ randomly.
Similarly define $L_{i+1}$ based on $L_i$.
Then, $\mathbb{E}[|L_i|] \le \frac{1}{2}\mathbb{E}[|L_{i-1}|]$.

Basically, going deep enough we can get $\Pr[|L_i|>1]<o(1)$.
At which point we must see repeated elements -- if we haven't seen a contradiction earlier. 

---

Had some problem like this:

$G$ is a graph with $n^{1.4}$ edges and $\sum d_i^{2} = n^{2.3}$.
Let $f(x) = \sum_{i,j\in E} x_i x_j$.
Turns out $\mathsf{Var}[f]= n^{.8}, \mathbb{E}[f]=n^{.4}$

Question: 
Can we distinguish between
- $x\sim \mathsf{Ber}(1/\sqrt{n})^{\otimes n}$
- $x\mid f(x) > n^{.9}$

with one sided error

in non-deterministic time $O(n)$?

I've kind of decided that this is maybe not such an interesting problem / isn't going to be useful in refining or refuting NCP, but here it is anyways.


