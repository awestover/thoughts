# current SotA
$\newcommand{\iqp}{\mathsf{IQP}}$ $\newcommand{\dsiqp}{\mathsf{dsIQP}}$ $\newcommand{\csiqp}{\mathsf{csIQP}}$$\newcommand{\maq}{\mathsf{MAQ}}$
communication is measured in words
ie sending 1 index is 1 communication cost
(and $\log n$ bits)

modifications:
- adaptive query prover (the default)
- random sampling prover (evil prover can do whatever) 

IQP (interactive query proof)
- now we call comm and queries separate things 
- but we'll always have them be the same

IQP(q,c) = queries, communication

$\iqp(n^.1, n^.1)$

MAQ (MA query):
- the prover sends their claim for the entire string
$\maq = \iqp(n^{.1},n)$.

$\dsiqp$
- prover queries, 
- verifier queries
- comms
$\dsiqp(pq, vq, c)$


Def CSIQP
Exists $V,P$, $P$ fast, such that $P$ always convinces you on yes instances
but for any even moderately fast $P^{*}$, $\Pr[\text{convince you on random no instance}]<\mathsf{negl}(n)$

$\csiqp(\text{honest prover queries}, \text{evil pq}, vq, c)$


"local reductions" -- ykwim


Main results

prop1:
If honest prover samples (a small number) of uniform random things and sends you
a subset of of those, and you just look at those and nothing else, then such a
protocol has an IQP because it's basically just "count 1s".

prop1.2:
There exists a problem
which is the following problem:
write down $\sqrt{ n }$ indices $x_i$. 
Put an easter egg at location $\bigoplus x_i$ in the YES instance, somewhere else in the NO instance.

claim:
exists sampling csiqp verifier $1$ honest prover $\sqrt{n}$ evil prover $o(n)$
but not $\maq(o(\sqrt{n}))$.

pf:
if ver always accepts yes instances
evil prover can modify string in very small number of locations to make it look like yes instance
so ver prolly accepts the no instance too

prop2:
a couple of things are equivalent to "HAS1" under local reductions.

prop3:
problem:
YES $1$ after first $\sqrt{n}$ indices
NO: $1$ before first $\sqrt{n}$ indices

prop4:
In $\iqp(\sqrt{n},0)$ (i.e., a tester)
exists $\iqp(1, 1)$
but not in $\dsiqp(o(n),o(\sqrt{n}),n)$

theorem5:
for any const $a\in (0,1)$
There exists a problem in 
$\csiqp(n^{a}, o(n), 1, 1) \setminus \maq(o(n^{a})).$

HOWEVER the pointer chasing problem is in the IQP-hierarchy
and so is the XOR question.

you just split up the path and ask for checkpoints.

for XOR you chunk the thing


pf: pointer chasing


prop6:
ckt evaluation is in the IQP hierarchy

q0:
non promise problems:
Any problem with a CSIQP also has an IQP.

q1:
property testing also
CSIQP --> IQP

q2:
Suppose you have a deterministic verifier, 
the honest prover samples non-adaptively but maybe with a weird pr distribution
and then they can send you any indices not just the ones they queried.

suppose you had a CSIQP with such a prover and verifier. 
can this be turned into an IQP?

q3:
is levin search thing legit?

q4:
tester, IPP, but no dsIPP

q5:
one prover wants you to say yes, 
one wants you to say no.
they talk for some rounds and try to convince you.

question is, for example, is CSIQP contained in the IQP-hierarchy

some fun questions in the hierarchy:
"every one is followed by a 2" or something

MAQ hierarchy -- not interesting. 
IQP hierachy -- very interesting
IQP hierarchy = IQP with logn rounds of dudes talking

q6: 
Is a random problem in the IQP hierarchy?

q7: find something outside of the hierarchy

q8: 
if P=NP does the IQP hierarchy collapse?

q9:
is all of IQP equiv to ckt evaluation


remark:
If $|Y|< n^{O(1)}$
then this is contained in PH-IQP.
**Proof:** 
say which one it is, then say if they're lying

Thought:
maybe we can make a "PRG" a language that's not actually random but has enough properties to make it still not in the PH-IQP like we believe RAND to be.

**Def of pseudorandom:** 
Let $R$ be any subset of the hypercube obtained by restricting $n/100$ coordinates.
We require that between $[.499,.501]$ many of the strings in $R$  are yes instances.
and we require non promise problem, ie yes + no = all.

**Claim**
this is not testable
proof: 
yao, do some stuff ???
seems pretty believable


hope: maybe this is not anywhere in the IQP hierarchy!


---
# OLD

update: we have changed the names of this thing because we're mostly not thinking about property testing. 

IQP (interactive query proof)
- we care about reads = comms + queries
- can do adaptive or random prover

MAQ (MA query):
- the prover sends their claim for the entire string

CSIQP, dsIQP -- just what you think they are.


- Can still be interested in property testing ones
- or non-promise ones
- iyw

---
The "paths" example is quite good for the CSIPPs

a q from N -- what else has a CSIPP

---
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


---

Interesting turn of events: 

- my above thing can easily be generalized to a dude that just samples some stuff and spits out a longer certificate


- but the story for adaptive is different!

In particular consider the following problem: 

You have a two pointer chains of length $n^{1/4}$.
One starts from the beginning of array, one starts from some random location.

In the YES instances, the chain starting from the beginning of the array ends in a smiley face, the other does not.

In the NO instance, the chain starting from the end of the array ends in a smiley face, the other does not.

If a $n^{1/4}$ bounded prover finds a smiley face and points to it, you can instantly be confident that it is a YES instacnce. 

But, an unbounded prover can say nothing to convince a verifier unless the verifier is willing to just read the whole chain herself.

---

a couple of open qs: 
(more in picture)

- can we find some other interesting problems with CSIQPs but no MAQs or whatever?

