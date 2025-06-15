**edit**: Unfortunately, there are major errors in this document.
Sorry about that.

# Introduction
$\newcommand{\R}{\mathbb{R}}$ $\newcommand{\E}{\mathbb{E}}$
Paul claims that one of the main problems with deep learning from an alignment perspective is that we evaluate AIs via sampling. In human designed AI, people can estimate the loss mechanistically --- they just think about the system that they designed, and say "yeah this has reasonable tail behavior." In deep learning we can't do this. This is why we could have an opaque system that is doing reasoning that we don't like when we train a system with deep learning. 

This has led ARC to thinking about the following question:

> ($\star$) Can we come up with a **mechanistic** method for estimating the expectation of a NN?

The word "mechanistic" intuitively means "feels pretty different from sampling". If you have a solution to this that you think is borderline mechanistic then you can test it by seeing if it's useful for some of the downstream tasks that ARC cares about.

I think it's pretty unclear + confusing exactly why solving $\star$ would be super great, but I'll think about that in a different document. In this document I'll discuss how ARC is thinking about $\star$ at the moment and what are the next steps that you'd want to take towards achieving $\star$.

ARC is interested in three settings:
1. Competing with sampling on random problem instances.
2. Competing with sampling on worst case problem instances, but where we get an optimized "advice string".
3. Competing with sampling on trained instances, where we get to learn an advice string in parallel to the training process, using the same compute budget as the training process.

I'll consider these in turn.

# (2) Worst case instances

Here's a problem statement:
> [!tip] Conjecture 1
> Let $\mathcal{L}$ be the set of 3SAT instances with $n$ clauses.
> Let $\Pi$ be the set of "explanations" ($n$ bit strings)
> There exists a **mechanistic** "explanation quality measuring machine" $U: \mathcal{L}\times \Pi \to \R$, where $U(\phi,\pi)$ can be computed in $n^{O(1)}$ time, such that for all $\varepsilon>0$, there is a **mechanistic** "expectation estimation machine" $G_\varepsilon: \mathcal{L}\times \Pi \to \R$ such that 
> - $G_\varepsilon$ runs in time $O\left(\frac{n}{\varepsilon^{2}}\right)$
> - If $\pi^* = \mathsf{argmax}_\pi U(\phi,\pi)$ then $G_\varepsilon(\phi,\pi ^{*})$ should be within $\varepsilon \sqrt{ \E_x\phi(x) }$ of $\E_x \phi(x)$

The relevant fact about the compute / error tradeoff is that this is the same compute / error tradeoff that naive sampling gets.

(Note: Someone said that Conjecture 1 might give an alternate proof of [[BPP in Sigma2]]. TODO: think about whether or not that's true, and whether or not it's concerning if true (probably isn't concerning bc that proof isn't too hard?).)

> [!tip] Conjecture
> Conjecture 1 implies that we can solve any similar flavor problem that we care about (because `#3SAT` feels like a pretty hard and pretty general problem).

Victor is pretty sure that this conjecture is False.
But maybe it's at least spiritually true.

> Def:
> T is a measure preserving reduction if
>  - T is computable in polynomial time
 > - T preserves measure
> For example:
> - T: 4SAT instances ---> 3SAT instances such that
> $$\E_x (T(\phi))(x) = \E_x (\phi(x)).$$
> Question: 
> Do measure preserving reductions exist?
> Probably exactly measure preserving is impossible for dumb number theory reasons.
> - Approximately measure preserving in some sense would also maybe be fine.
> - I've thought about this a bit and suspect that it's pretty tricky to do / maybe impossible.

Mike thinks that I may not have given the optimization oracle enough power. I'm unsure but will think about my version for now.

# (1) Random instances
This is the only question that ARC has had progress on so far. 
ARC has proved the following theorem:

> [!tip] Theorem 1
> Let $\mathcal{D}$ be the vanilla distribution over 3SAT instances with $n$ clauses; Namely, each clause has three random (distinct) variables in it, and each of the variables is randomly negated or not.
> For all $\varepsilon>0$, there is a "expectation estimation machine" $G_\varepsilon$ such that 
> - $G_\varepsilon$ runs in time $O\left(\frac{n}{\varepsilon^{2}}\right)$, and
> $$\E_{\phi \sim \mathcal{D}} [G_\varepsilon(\phi)-\E(\phi)]^{2}\le \varepsilon^{2}\mathsf{Var}(\phi).$$

> Note: ARC's proof of theorem 1 is imo very complicated (it uses the words "homogeneous polynomials are the irreducible representations of the hyperoctahedral group"). I'd like a much simpler proof please.

Currently, ARC is interested in investigating more complicated distributions. For instance, a distribution where some variables are more common than others, or where we have more negations than positive versions of literals.

> (Note: ok ARC technically hasn't written down a proof of theorem 1 but we're pretty confident that we have such a proof, maybe up to log factors).

> (Note: Eric thinks that $\log n$ SAT might be a better problem.)

> (Note: Eric thinks that you might need advice to solve this if the distribution is sufficiently weird. I'd be pretty interested in a proof or vibes based proof as to why this would be true. Eric gave some vibes based on obfuscation, but I didn't find it too compelling.)

> (Note: Maybe if we solved random cases, we could do random hacky stuff like [this](https://arxiv.org/abs/2410.13211) better).

# (3) Trained instances
- Thinking about random search as a hill climbing procedure seems like a good first step.
- Eventually we'll need to think about SGD though probably. Which is an L.


# Breaking ARC

Here's a problem that I thought it would be lethal to ARC's agenda to be unable to solve:

> [!tip] Conjecture 1
> Let $\mathcal{L}$ be the set of 3SAT instances with $n$ clauses.
> There exists a deterministic function $U:\mathcal{L} \times \{0,1\}^{n^{2}}\to [2^{n^9}]$ and a deterministic function $G_\varepsilon: \mathcal{L} \times \{0,1\}^{n^{2}} \to \R$ such that 
> - $G_\varepsilon$ runs in time $O\left(\frac{n}{\varepsilon^{2}}\right)$
> - If $\pi^* = \mathsf{argmax}_\pi U(\phi,\pi)$ then $G_\varepsilon(\phi,\pi ^{*})$ should be within $\varepsilon \sqrt{ \E_x\phi(x) }$ of $\E_x \phi(x)$

Suppose we could prove Conjecture 1. Then we'd have the following corollary:
> [!tip] Corollary 2
> Let $\mathcal{L_{1}}$ be the set of 3SAT instances with $n$ clauses such that $\E_x\phi(x) < \frac{1}{3}$, and let $\mathcal{L}_2$ be the set of 3SAT instances with $n$ clauses such that $\E_x \phi(x) > \frac{2}{3}$.
> There exists a deterministic function $U:(\mathcal{L}_1\cup\mathcal{L}_2) \times \{0,1\}^{n^{2}}\to [2^{n^9}]$ and a deterministic function $G: (\mathcal{L}_1\cup\mathcal{L}_2) \times \{0,1\}^{n^{2}} \to \R$ such that 
> - $G$ runs in time $O(n)$
> - For $i\in\{1,2\}$ if $\phi \in\mathcal{L}_i$, and $\pi^* = \mathsf{argmax}_\pi U(\phi,\pi)$ then $G(\phi,\pi ^{*})=i$.

Next, we'd have the following corollary:
> [!tip] Corollary 3
> Let $\mathcal{L_{1}}$ be the set of 3SAT instances with $n$ clauses such that $\E_x\phi(x) < \frac{1}{3}$, and let $\mathcal{L}_2$ be the set of 3SAT instances with $n$ clauses such that $\E_x \phi(x) > \frac{2}{3}$.
> Distinguishing between $\phi\in \mathcal{L}_1$ and $\phi\in \mathcal{L}_2$ can be done in $P^{NP}$.

Next, we'd have the following corollary:
> [!tip] Corollary 4
> $\mathsf{BPP} \subseteq \mathsf{P}^{\mathsf{NP}}$.

Unfortunately, according to my friend, Corollary 4 is beyond the reach of complexity theory for now. Thus we should not expect to be able to prove Conjecture 1.
