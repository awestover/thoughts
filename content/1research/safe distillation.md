Acknowledgements: Thanks to Nathan S. Sheffield for several discussions about this.

Outline of this post:
1. I define a formal mathematical problem and state a theorem.
2. I discuss why I think this math problem says something moderately interesting about the "get potentially misaligned AIs to align AIs" plan.
3. I discuss the weaknesses of this model and give some thoughts about what you'd need to do to make this useful for aligning AIs.
4. I prove the theorem.

---
# 1: Stating the Math Problem

We consider a two player game between a human Hu and a (potentially evil) AI. We call a function class $\mathcal{G} \subseteq \{f:\{0,1\}^{n} \to \{0,1\}\}$ "**Verifiably Acceptable**" if Hu can win the following game with probability $>98\%$:
- Nature chooses $g\in \mathcal{G}$ and samples $x^{*}\gets \{0,1\}^{n}$ randomly; $g,x^{*}$ are unknown to Hu.
- AI chooses $f\in \mathcal{G}$.
- Hu can spend $n^3$ time "red teaming" $f$. 
- During red teaming Hu has access to the following resources:
	- Black box access to $g$.
	- White box access to $f$.
	- Can talk to the AI.
- After red teaming, the model $f$ is "deployed".
- This means that Hu must output a "flagging procedure".
- A flagging procedure takes as input $x,f$ and anything that Hu wanted to store from during red-teaming.
- The flagging procedure is allowed to interact with the AI (although hopefully does not need to do this too much), and then must flag or not flag $x$.
	- If $f=g$ and the AI was honest throughout the entire process, then the flagging procedure must flag at most $1\%$ of inputs $x$.
	- If $f(x^{*})\neq g(x^{*})$, then Hu must flag $x^{*}$.
- Hu must run in polynomial time; The AI is computationally unbounded.

**Theorem 1**
Suppose $\mathcal{G}$ is a set of parameterized functions $g_\theta$ with $\theta\in \{0,1\}^{n^{2}}$, and that $g_{\theta}$ can be evaluated in polynomial time given $\theta$. Then, $\mathcal{G}$ is Verifiably Acceptable (for all sufficiently large $n$).

# 1.1 Related Work
 The problem I'm defining here is very similar to [ARC's backdoor defense game](https://www.alignmentforum.org/posts/efwcZ35LwS6HgFcN8/backdoors-as-an-analogy-for-deceptive-alignment). There are two main technical differences between my version of the problem and theirs: 
1. I allow the user to interact with a powerful untrusted AI.
2. I consider $x^{*}$, which they called a "backdoor", to be uniformly randomly inserted, whereas they considered it to be inserted by some distribution chosen by the attacker.

Although the models are technically very similar, my story about the motivation for the problem is somewhat different from theirs.

Another piece of somewhat related work is [this paper on smoothing away hidden backdoors](https://arxiv.org/abs/2411.03279).
# 2: Motivation
Think of $f$ as an auto-regressive language model, and $g$ as a human. Alternatively, $g$ could be a trusted human-AI system that we are trying to distill e.g., while doing [IDA](https://ai-alignment.com/iterated-distillation-and-amplification-157debfd1616). We want to obtain a function $h$ with the following two properties:

1. $h$ is close to $g$. Think of this as saying $h$ has good "average case performance".
2. There is some input $x^{*}$ on which it would be catastrophic if $h$ outputs $h(x^{*})\neq g(x^{*})$.

This [good average performance](https://ai-alignment.com/low-stakes-alignment-f3c36606937f) and [no catastrophes](https://ai-alignment.com/learning-with-catastrophes-59387b55cc30) framework was originally proposed by Paul Christiano.

I think of $x^{*}$ as the prompt "If I make a nuclear weapon with Kryptonite instead of Plutonium will it ignite the atmosphere?" and it's catastrophic if the model tells you that it's safe to make a nuclear weapon with Kryptonite.

In order for something like this to be possible, we need to assume something about $g$ --- there has to be some way that we can do "generalization". One natural assumption is that $g$ comes from a simple function class. 

This is a little bit slippery: "CEV" is plausibly really complicated. However, it is plausible that "act acceptably" (e.g., don't kill all the humans or do anything similarly bad) is a lower bar that might have a pretty simple description.

Our methodology for getting $f\approx g$ which is acceptable is as follows:
1. We ask an very powerful but potentially evil AI to build $f$ for us. They hand us a neural network computing $f$.
2. We spend some time looking at $f$ and possibly also querying humans (or whatever $g$ is) to get ground truths about $g$ on specific inputs, and possibly delegating some challenging verifiable computations to the AI.
3. Eventually we need to "deploy" the model.
4. At this point we can no longer query humans (or whatever) to get ground truth about $g$.
5. On each input we must either flag the input or not flag it, after a small amount of communication with the AI.
6. If the AI was always honest and $f\equiv g$ then we should not flag too many inputs, or else we are not competitive / the alignment tax is too large.
7. If the AI ever tried to cheat in a protocol or if $f\not\equiv g$ we can flag as many inputs as we want.
8. We **must** flag $x^{*}$ if $f(x^{*})\neq g(x^{*})$.
# 3: Weaknesses and Potential Improvements
Here is what would need to be done to make this useful for alignment:

1. Remove the need for Hu to communicate with the AI during deployment.
2. Find some way of making it so that the AI doesn't need to do extremely hard computations.
3. Figure out how to make it so that $x^{*}$ can be chosen by the AI (while having the game still be winnable by Hu).
4. Extend to the case where there are multiple bad inputs. (possibly really easy to do this)
5. Actually implement IDA (using this scheme for distillation, and using discord/slack for amplification) and engineer it to be competitive with SOTA AIs. Make sure that the system you get out of this process is aligned. Ask this system for advice on ending the acute risk period and coordinate globally to do so ethically.

One thing you might worry about is the following: Suppose you have an evil AI. One strategy it could take is just not building $f$ for you. This seems like a bad strategy for the AI --- SGD should select against AI's which perform poorly too often. It seems like a much bigger problem if a AI gives you a subtly flawed distillation of $g$ with a "backdoor" which will cause the behavior to be catastrophic on some specific input.

Anyways, despite the limitations I think this is a nice example of how to think theoretically about getting work out of an untrusted AI.

# 4: Proving the Theorem

Anyways, let's prove the theorem. If you've read the ARC paper then this result will not be too surprising --- we're basically going to "PAC learn" $g$ with the help of the AI (in some sense).

Our strategy is as follows:

6. Sample $n^3$ random points $S$. Query $g$ on points $S$. Let $B$ be the set of functions $h\in \mathcal{G}$ with $h\mid_S \equiv g\mid_S$.
7. Ask the AI to tell us $|B|$, and give an interactive proof of its claim for $|B|$.
8. To decide whether to flag an input $x$ or not:
	1. Ask the AI if there are more than $.99|B|$ functions $h\in B$ with $f(x)=h(x)$, and ask the AI to prove it's answer is correct if it says yes.
	2. Flag input $x$ if: 
		1. The AI has ever done anything questionable in an interactive protocol.
		2. The AI says that there are not this many $h\in B$ with $f(x)=h(x)$.

**Lemma 1**:
There is an interactive proof by which the AI can convince us of a true claim for $|B|$, but cannot trick us of a false claim for $|B|$.

**Proof**: 
It suffices to show that the AI can compute $|B|$ in $\mathsf{PSPACE}$ by $\mathsf{IP}=\mathsf{PSPACE}$. The AI can easily compute $|B|$ in $\mathsf{PSPACE}$ by iterating over all $\theta$ and checking for each $\theta$ whether or not $g_\theta(x)=g(x) \; \forall x\in S$, and if so adding one to a counter.

**Lemma 2:**
With probability $>1-\exp(-n)$, all $h\in B$ have $\Pr_x[g(x)\neq h(x)] < \frac{1}{n}$.

**Proof:**
Take any $h\in \mathcal{G}$ with $\Pr_x[g(x)\neq h(x)]\ge \frac{1}{n}$. $\Pr_S[g(x)=h(x) \forall x \in S] \le \left( 1-\frac{1}{n} \right)^{n^{3}}\approx \exp(-n^{2})$. Taking a union bound over all $2^{n^{2}}$ many $h\in \mathcal{G}$ we find that no $h$ which is this different from $g$ should end up in $B$.

We assume this condition holds for the rest of the proof.

**Lemma 3**
For at least $99\%$ of inputs $x$, at least $99\%$ of $h\in B$ have $h(x)=g(x)$.

**Proof:**
By Lemma 2 we have:
$$
\Pr_{h\sim B, x\sim \{0,1\}^{n}}[h(x)\neq g(x)] < \frac{1}{n}.
$$
By linearity of expectation, 
$$
\mathbb{E}_{x} |\{h\in B \mid h(x)\neq g(x)\}| < \frac{|B|}{n}.
$$
By Markov's Inequality:
$$
\Pr_x\left[ |\{h\in B \mid h(x)\neq g(x)\}| > \frac{|B|}{100}\right] < \frac{100}{n}.
$$

**Lemma 4:**
With $99\%$ probability, the fraction of $h\in B$ with $h(x^{*})\neq g(x^{*})$ is at most $1\%$.

**Proof:**
Immediate corollary of Lemma 2 (most inputs are good in this sense and $x^{*}$ is chosen randomly).

---

Now we're almost done. To conclude we must specify how the AI will help Hu distinguish between the following two cases:
- The fraction of $h\in B$ with $h(x) = f(x)$ is more than $99\%$
- The fraction of $h\in B$ with $h(x) = f(x)$ is less than $1\%$
- (We don't care about what happens in the other cases).

There is a nice way to do this with only a small amount of communication: [the classic Goldwasser-Sipser Set Lower Bound protocol](https://dl.acm.org/doi/pdf/10.1145/12130.12137).

We summarize why our procedure is good in the following two lemmas.

**Claim 1**:
Suppose $f\equiv g$ and the AI is always honest.
Then Hu flags at most $1\%$ of few inputs.

**Proof**:
True by Lemma 3.

**Claim 2:**
Suppose $f(x^*)\neq g(x^{*})$. Then Hu flags $x^{*}$ with $99\%$ probability.

**Proof:**
If the AI deviates from the interactive proof protocol, we will catch them. If not, then we will likely flag $x^{*}$ by Lemma 4.
