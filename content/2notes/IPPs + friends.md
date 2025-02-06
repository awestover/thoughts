There are a couple of questions about IPPs that I think are pretty interesting. 

Some context:
- We mostly think about promise problems. That is, 
	- there are some YES instances
	- some NO instances
	- and some banned instances 
- Property testing questions are quite interesting too. That is,
	- NO instances are things which are $\varepsilon$-far from every YES instance.

There are a couple of types of things that we think about:

- testing
- IPP
- dsIPP
- CSIPP

I'll generally measure complexity by number of "reads" where **"reads" = "communication + number of queries made to the string"**.


> [!caution] 
> I guess we're measuring reads in "word RAM".
> Like, if prover send verifier one index into the array, that counts as 1 read, 
> and reading an index of the string counts as 1 read.
> 
> Be careful to not abuse this $\log n$ factor!


Talking about running time is also okay, but feels less standard. 

You can ofc also separately track communication complexity and query complexity but this is kind of annoying. 

Also, we sometimes care about **verifier reads** and **prover reads**.

---

# questions

> Q1: What are some questions that are not testable in $o(n)$ time, but have IPPs?

> Q2: What are some questions that don't have IPPs (i.e., having a prover doesn't improve over the complexity of just testing it yourself).
> For instance, questions where even with a prover, you still need to read approx the entire input.

> OQ3: **are there qs where strong provers can help, but weak provers cannot?** \
> For instance, this could look like the following:
> - Test in $O(\sqrt{ n })$ queries.
> - IPP with Verifier making $O(1)$ reads
> - But, if Prover makes $o(n)$ reads, then the Verifier needs $\Omega(\sqrt{ n })$ reads

(In other words, questions for which there is an IPP and a tester, but no dsIPP)

> OQ4: **are there qs where weak provers can help, but strong provers cannnot?**\
> - Test in $O(\sqrt{ n })$ queries.
> - No IPP -- no proof that helps verifier take time $o(\sqrt{ n })$.
> - BUT, there is a CS-IPP (computationally secure).
> - That is, if you know that the Prover (even an evil Prover) has num queries bounded by $O(\sqrt{ n })$, then there is some such good Prover that can help you beat $o(\sqrt{ n })$ queries.

I'll assume that the bound on computation is the same on honest and malicious provers. 
It could be interesting to imagine that the malicious Prover is allowed a little extra juice.

---

Here's a solution to OQ3 -- although we'd like a "property testing" version of this:

- valid instances: have exactly one $1$
	- YES instances: $1$ occurs after the first the first $\sqrt{ n }$ spots 
	- NO instance: $1$ occurs in the first $\sqrt{ n }$ spots

observe: 
- test in $\sqrt{ n }$.
- verify in $1$ read (remember we are in word RAM model)
- but, if the Prover is efficient, the Verifier is stuck with doing $\Omega(\sqrt{ n })$ reads.

---

Feb6 notes with N: 

We tried to come up with examples for Q2 -- promise problems without IPPs.

They mostly ended up looking like this:
- YES instance = all zeroes
- NO instance  = at least one $1$.

There are several problems that are hard due to being generalizations of this. 
For instance, 

- exact hamming weight
- exact monotonicity for a boolean function on $\log n$ bits.

We came up with a notion of reduction for these problems.
Basically, we'll say problem A is harder than problem B if there's a local transformation of inputs to problem $B$ that makes them into problems in A, with the same answer.

---

Something moderately surprising: 

Suppose you have a set $S$ and want to distinguish between $|S|>k$ and $|S|<k/100$.

It turns out that there's a really easy $O(\log k)$ round interactive protocol for this, where the communication is just sending $1$ element of $S$ each time (the verifier repeatedly asks for the $i$-th largest element in $S$, and the prover supposedly sends it back).

---

Some thoughts on OQ4:

@Nathan:
How do we even define a "bounded adversary"?

Here's a naive attempt, which is obviously bad:

> $\exists (V,P)$ with $P$ bounded such that for all $x\in \mathsf{YES}$, $\Pr(V(x,P))>99\%$,
> and, $\forall P^{*}$ bounded, $\forall x\in \mathsf{NO}$, $\Pr(V(x,P^{*}))<1\%$.

In case it's not obvious, this is bad because $P^{*}$ can just hard-code some specific NO instance $x^{*}$, and only has to trick $V$ on this one instance -- but now $P^{*}$ knows the whole instance.


One way you could try to fix this is to just require that $P^{*}$ can't trick you on **most** $x\in\mathsf{NO}$.

ok, so the req is: 
$$
\forall P^{*}, \Pr_{x\sim \mathsf{NO}}[V(P^{*},x)]<1\%.
$$
----

We have a very weak version of the CS-IPP thing

problem: 
- distinguish between $100\sqrt{ n }$ many $1$'s 
- and less than $o(\sqrt{ n })$ many $1$'s

A $\sqrt{ n }$-bounded prover can generate a proof that requires 1 read to check. 
Whereas, for an unbounded prover, the verifier probably needs at least $\Omega(\log n)$ reads.

---

**conjecture: strategy stealing can be done.**
(i.e., CSIPP is fake)

Let's try to prove this for the case where
- Bounded protocol with prover in $\sqrt{ n }$ reads, Verifier in $1$ read.

So we wanna generically show a decent protocol for unbounded provers
ideally the Verifier should be $O(\log n)$ reads.


- Case 1: Honest prover is deterministic and non-adaptive
	- this is trivial.


- Case 2: honest prover is randomized, but still non-adaptive. 

ok, so the setup here is as follows:
- if an honest Prover samples $\sqrt{ n }$  points, they'll find a good thing to point at, and we'll be convinced
- for a random NO instance, it's very unlikely that a random $\sqrt{ n }$ points has an element that you can point at which will convince the Verifier. 


What this proves:
 
> Suppose $\exists (V,P)$ with $P$ being $\sqrt{ n }$ bounded and **where $P$ is non-adaptive** such that for all $x\in \mathsf{YES}$, $\Pr(V(x,P))>99\%$,
> and, $\forall P^{*}$ $\sqrt{ n }$ bounded, $\forall x\in \mathsf{NO}$, $\Pr(V(x,P^{*}))<1\%$.
> 
> Then, there exists an actual IPP for this problem too.\
> In fact, if you work in the "Random Oracle" model, you can make it a dsIPP

**Proof:** 
This really just does mean that YES instances have $\sqrt{ n }$ easter eggs and NO instances have at most $\sqrt{ n }/100$ easter eggs. 

You can use the standard thing to prove bounds on sizes of sets. 

Or, if you have a random oracle, you can ask the random oracle to specify a set of size $\sqrt{ n }$, and then you are required to spit out an answer in that set. 
One really nice thing about this is that now we actually have only $O(1)$ reads for the verifier (instead of $\log n$ above).
The prover just has to say which index of the random oracles $\sqrt{ n }$ sized set the easter egg that it identified was. 

todo: can we generalize this beyond 1 query things?
can we get rid of the word "non-adaptive" ?

