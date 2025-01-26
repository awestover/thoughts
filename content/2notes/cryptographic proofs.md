In this note I'll do a literature review of proofs, including the following topics:
- IP = PSPACE
- AM/MA (public coins)
- ZK-proofs (zero knowledge)
- NIZK, Fiat-Shamir (Can we remove interaction?)
- Doubly-efficient proofs
- Philosophy of how these relate to NCP (?)

---

High level summary of takeaways:
- interaction is pretty powerful
- there doesn't appear to be a meaningful way to get rid of interaction for things outside of NP.
- so, this probably has very limited relevance for NCP.
	- beyond the observation that IPs should be banned for NCP
	- because they dont give mechanistic explanations

---
**Basic IPs**

Simplest example: 

Prover wants to convince Verifier that two graphs $G,H$ aren't isomorphic. 
Verifier is poly time, Prover is unbounded.
Hope: Evil provers can't prove false things, good provers can prove true things.

How to do it: 
- V: sends a random permutation of either $G,H$.
- P: identifies which one it was. 
- If P can do this reliably, the graphs must be different.

Remark -- you can do parallel repetition here to amplify success pr.

Remark -- most complexity theorists suspect that constant round IPs can only do NP.

**Public versus private coins**
Somewhat surprisingly, you can eliminate the need for private random coins by blowing up the number of rounds by a constant factor!

**Turns out that there are IP's for all of PSPACE.**

How? "Checksum procedure."

For simplicity we'll show an IP for "Sharp-SAT" (TQBF, which is complete for PSPACE, isn't too much harder, but requires more details which aren't super relevant to the technique).
The Sharp-SAT problem is, given a formula and a number $k$, check whether there are exactly $k$ sat assignments of the formula. 

Here's how we're going to do it. 
Fix a formula $\phi$. Let $q$ be an $n$-bit prime where $n$ is number of variables in the formula. Define $f$ to be an arithmetization of $\phi$. Namely, we take $\phi$, trade in $x\land y$ for $x\cdot y$ and trade in $x\lor y$ for $1-(1-x)(1-y)$.
Define 
$$
f_i(x_{1},\dots,x_i) = \sum_{x_{i+1},...,x_n\in \{0,1\}} f(x_{1},\dots,x_n).
$$
The protocol is as follows: 

1. Ask the Prover for the value of $f_{0}$ --- this is supposedly the number of SAT assignment of $f$, if the Prover is being honest. The Prover hands over some number $f_{0}'$ which may or may not actually be $f_{0}$. (PS: the prime doesn't mean derivative)
2. Ask the Prover to give you coefficients for the degree $n$ polynomial $z\mapsto f_{1}(z)$; the Prover hands over some polynomial $z\mapsto f_1'(z)$ which may or may not be $f_{1}$.
	2. Check that $f_{1}'(0)+f_{1}'(1)=f_{0}$.
	3. This ensures that if $f_{0}'\neq f_{0}$ is a lie, then $f_{1}'\neq f_{1}$. 
3. Now, the verifier chooses a random $r_{1}\gets [q]$ and tells the prover about it. 
	1. Because $f_{1}'(z)$ is a low degree polynomial, the chance that $f_{1}'(r_{1})=f_{1}(r_{1})$ is pretty low. In particular low enough that we can union bound over all $n$ steps of the proof and assume that it never happens. 
4. Anyways we kind of just continue on like this. 
	1. The prover sends us $z\mapsto f_k'(r_{1},\dots,r_{k-1},z)$. 
	2. We check for consistency with $f_{k-1}'$.
	3. If $f_{k-1}'$ was a lie, so if $f_k'$.
5. Finally at the end the prover will tell us some value $f_n'(r_{1},\dots,r_n)$ and we can literally just check whether it's true or not. 

**ZK proofs for NP**
Suppose you want to convince me that your graph is 3-Colorable. 
But you don't want me to learn anything about the witness.
(Note that this problem is NP-complete so if we can have a ZK protocol for it then we also get a ZK protocol for any other problem in NP).

**Remark**: there are actually even ZK proofs for anything in PSPACE!

Here's how to do it:
- P: Sends commitments to the values of a coloring (permute the coloring first).
- V: points to a random edge and asks you to open the commitments to the endpoints colors.
- P: does so, and V checks that it is legit. 

It should be pretty clear that this is revealing no info.
However, the best success pr you could get if your graph is not actually 3 colorable is like $1-1/n^{2}$.
So repeating the protocol $100n^{3}$ times amplifies this to being pretty convincing --- i.e., exponentially unlikely that you'll convince me if it's false. You can do the repetitions in parallel too, so it's not blowing up rounds of communication, which is nice.

---

At this point I should probably actually define ZK:
- For any (potentially malicious) efficient Verifier $V^{*}$
- There exists an efficient "simulator $S$"
- Such that for any $x$ with the property (e.g., $x=$ a 3-colorable graph),
- Such that the following two distributions are comp indistinguishable:
	- Output of simulator $S$
	- Transcript of interaction between honest Prover $P$ and verifier $V^*$

One important distinction is that there are two kinds of ZK you can talk about:
- ZK with respect to an honest verifier (one that follows the protocol)
- ZK with respect to a malicious verifier (this is what was defined above, and is more interesting)

---

Okay, so it's quite easy to argue zero-knowledge versus an Honest Verifier:

We can simulate the transcript as follows:
- First choose the edges the Verifier wants to look at (randomly).
- Then, choose the coloring so that that edge has endpoints of different colors, commit to that coloring, send it, have the Verifier send the edge they want to see, and open the commitment. 

If we were willing to do the repetitions in series, then it'd be easy to argue zero-knowledge against a cheating verifier: 
- We just repeatedly try to commit to a random coloring, 
- hope the verifier picks a good edge,
- and if it doesn't we rewind and try again.

If we want to do the repetitions in parallel. Then it seems kind of annoying. 
Somehow you have to say "wlog we can choose the verifier's edges in advance because the bit commitments are as good as random to them". But not quite sure how to formalize this.

---

Another quick super simple example of a ZK proof -- QR.

- $P$ has secret $x=s^{2}$, wants to prove it's a QR.
- $P$ sends $xt^{2}$ for random $t$
- $V$ asks for either $st$ or $t$.
- $P$ sends it.

#todo argue that this is legit

---

**NIZK** -- in the random oracle model

Can we have non-interactive ZK proofs for NP?

Impossible without random oracle assumption (assuming $\mathsf{P}\neq \mathsf{NP}$).

Now we're going to assume that we have a **public random function** $h$.
- (In other words, it's an exponentially long random string.)
	- Sometimes you can get away with a shorter random string (e.g., polynomially long). This is called the Common Random String model.
- Think of this random string as the NIST randomness beacon or something.
	- Although if we base AI safety on this, and then the AI goes and messes with the source of randomness that we're using then that's kind of unfortunate.
	- Let's not worry about that for now.
- Probably it's best not to think of $h$ as SHA-256; although maybe if you were extremely careful you could get away with something like this. But one reason why SHA256 is not very random is because it's deterministic and it's code is just sitting on the internet.

Anyways, here's the idea, called the Fiat-Shamir protocol:


**An interactive (public coin) ZK proof of knowledge of DLOG** 

- We work over the group $G = \mathbb{Z}_p^*$. To be secure, $p$ should be $\mathsf{poly}(n)$ bits (i.e., $|G|$ should be exponentially large). $|G|=p-1$ ($p$ is prime) should be known to all parties -- or else how are they even going to do computation in the group.
- There is a public generator $g$ for $G$, and a public value $y$.
- Alice has $x$ such that $g^{x}=y$. 
- DLOG assumption: it's hard to compute $x$ given only $g,y$.
- Alice would like to convince Bob that she knows $x$ without revealing any info about $x$.

Protocol:
- A: pick $v\gets \mathbb{Z}_{\phi(q)}$, send $t=g^{v}$
- B: Send $c\gets \mathbb{Z}_{\phi(q)}$
- A: Send $r=v-cx\mod \phi(q)$
- B: Check $t=g^{r}y^{c}$

**Claim 1:**
If Alice doesn't know $x$, then she can't produce a convincing $r$.

**Proof**:
You can derive $x$ from $(r,v)$. But you're not allowed to break DLOG.

**Claim 2:**
Bob learns nothing about $x$, besides the fact that Alice knows $x$.

**Proof**
A simulator can start by choosing $c,r$ randomly from $\mathbb{Z}_{\phi(q)}$.
Then it can compute $t=g^{r}y^{c}$.
Then it can send all the stuff in the normal order. 
This transcript is actually identically distributed to the original transcript!
So clearly the transcript carries no information by itself.

**Claim 3**
You can make this **non-interactive**, securely, by using the Fiat-Shamir method.


---
**Doubly Efficient proofs**
(Goldwasser, Kalai, Rothblum https://dl.acm.org/doi/10.1145/2699436)
- Suppose you have a log-space uniform circuit with depth $d$ and input size $n$.
- Then there is a ("doubly efficient") interactive proof where the prover takes time $\mathsf{poly}(n)$
- And the Verifier takes time $n\cdot \mathsf{poly}(d, \log n)$, and the communication complexity is $\mathsf{poly}(d,\log n)$.

Basically you should think of this as, if there's a computation that can be done in time $n^{100}$ then there's a proof of the computation that can be generated in time ~$n^{100}$ and checked in time ~$n$.
