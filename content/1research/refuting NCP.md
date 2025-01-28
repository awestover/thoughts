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