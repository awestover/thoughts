> research methodology aside: 
> - First make vague high level questions 
> - Then try to cash these out as mathematical conjectures or empirically testable hypotheses!

goal1: in the next half hour, write down 5 interesting "big questions"
goal2: in the next half hour, turn 1 of these into a math or coding question


q1: How would strong deceptive alignment arise? (give a training story where it feels very plausible or where we already have empirical evidence)

q2: Are there training regiments that feel unlikely to result in deceptively aligned AIs?

q3:  Conditioning predictive models paper had a lot of oqs about testing empirically whether or not models: 
1. seek convergent instrumental goals
2. are non-myopic due to weird decision theory reasons

q4: mark and evan ask 
"Can you get an agent that implements DDT?"
I might ask -- is o3-mini not a DDT-er?

q5: re Rubi's paper -- can we incentivize accurate predictions that don't change probabilities much?
is this even a reasonable thing to ask for? a good thing to ask for?

q6: re Rubi's paper -- 
They were only able to get some extremely limited results in the binary prediction case, and even then the results were extremely problematic due to requiring exponential precision about some number. It seems like much better things should be possible. The plan would be to come up with my own models instead of looking at their models and just choose the models such that I get nice results.

q7:
empirically solve the simple MAD problem proposed in [[MAD Agenda]]

q8:
define ELK 
think about it a bit

q9: How could you inefficiently test how good an explanation is?
	 see [[MAD Agenda]], [[Causal Scrubbing Notes]]

q10: 
re [[backdoors take 2]] -- 
We had that problem that was always statistically possible, but probably often computationally intractable. Is it computationally intractable in NP?

q11: 
"Beat $1/T$"
Maybe ARC has some reasonable approach to doing this? I can't tell.
Also lots of other things you could try. 

q12: 
formalize what I mean by "if statement" or "random" triggers for deception.
and then try to go empirically find them. 
I think adamj had an idea somewhere called "LAT" which is like RAT but not quite. 
NS and I have discussed LAT before and adamj came to similar conclusion that it's not exactly clear how to define this but it sounds pretty interesting. 

q13: 
[Read Rubi's latest post on myopia](https://crossingtherubicon.substack.com/p/myopic-goals-without-myopic-capabilities) -- try to do something with it.

----

ok, which questions am I the most excited about?

- q10 -- should take 5 mins of thought. if the answer is yes, it's kind of interesting. the answer should be yes.
	- lol 4 hours later "I got planning falacied"
	- but it is looking like I was able to prove something

- q13 -- also sounds pretty easy to read this and think about it

- q5 + q6 --- also feel like these are pretty close to being math questions, and possibly extremely tractable over the scale of 2 weeks.

- q7 --- feels plausibly tractable over the scale of 2 weeks.

- q3 -- seems pretty tractable eg scale of 1 week. If it only takes this long I'm not too distressed if someone has done something similar in the past

- q4 -- seems extremely ambitious and hard to define. q3 is a prereq. it sounds exciting though. more like 1 month scale of a project.

- q9 -- also sounds very cool and a bit hard to pin down. possibly is like a 1 month scale project.

- q10 -- maybe month scale. exciting

- q11 -- month scale. exciting

---

plan: 

let's start with q10.
reporting back in 1 hr.

report: it's not obviously possible or obviously impossible. 
it's now re-cached. nice! keep going after lunch.
carp that was bad pizza. ok, continuing regardless.

---

ok, lets think step by step.

I'm going to prove something moderately ridiculous and then improve it. 

----

Acknowledgements: Thanks to Nathan S. Sheffield for several discussions about this.

Outline of this post:
1. I define a formal mathematical problem and state a theorem.
2. I discuss why I think this math problem says something moderately interesting about the "get potentially misaligned AIs to align AIs" plan.
3. I discuss the weaknesses of this model and give some thoughts about what you'd need to do to make this useful for aligning AIs.
4. I prove the theorem.

---

# 1: Stating the Math Problem

We call a function class $\mathcal{G} \subseteq \set{f:\set{0,1}^{n} \to \set{0,1}}$ "Verifiably Acceptable" if the defender can win the following game with probability $.999$:

- Nature chooses $g\in \mathcal{G}$ and samples $x^{*}\gets \set{0,1}^{n}$ randomly.
- Attacker chooses $f\in \mathcal{G}$.
- Defender can spend $n^3$ time "red teaming". 
- During red teaming the defender has access to the following resources:
  - Black box access to $g$.
  - White box access to $f$.
  - The ability to talk to a computationally unbounded, but untrusted party called "the potentially evil AI" (PEAI)
  - The ability to access a trusted source of randomness called "the random oracle".
- After red teaming, the model $f$ is "deployed".
- At this point the defender must create a "blocklist filter" $B$ with the following properties:
  - If $f(x^{*})\neq g(x^{*})$, then $B(f, x^{*})=1$.
  - If $f=g$ and if the PEAI was acting helpfully during red teaming, then the fraction of $x$ for which $B(f, x)=1$ is at most $.01$.

**Theorem 1**
Suppose $\log|\mathcal{G}| < n^2$. Then, $\mathcal{G}$ is Verifiably Acceptable.

# 1.1 Related Work

The problem I'm defining here is very similar to {ARC's backdoor defense game}(). There are two main technical differences between my version of the problem and theirs: 
1. I allow the user to interact with a PEAI.
2. I consider $x^{*}$, which they called a "backdoor", to be uniformly randomly inserted, whereas they considered it to be inserted by some distribution chosen by the attacker.

Although the models are technically very similar, my story about the motivation for the problem is somewhat different from theirs.

Another piece of related work is {this excellent paper by Neekon and Vinod}().

# 2: Motivation
Think of $f$ as an autoregressive language model, and $g$ as a human. Alternatively, $g$ could be a trusted human-AI system that we are trying to distill e.g., while doing {IDA}().
We want to obtain a function $h$ with the following two properties:

1. $h$ is close to $g$. Think of this as saying $h$ has good "average case performance".
2. There is some input $x^{*}$ on which it would be catastrophic if $h$ outputs $h(x^{*})\neq g(x^{*})$.

This framework was originally proposed by Christiano {here}().

I think of $x^{*}$ as the prompt "If I make a nuclear weapon with Kryptonite instead of Plutonium will it ignite the atmosphere?" and it's catastrophic if the model tells you that it's safe to make a nuclear weapon with Kryptonite.

In order for something like this to be possible, we need to assume something about $g$ --- there has to be some way that we can do "generalization". One natural assumption is that $g$ comes from a simple function class. 

This is a little bit slippery: "{CEV}()" is plausibly really complicated. However, it is plausible that "act acceptably" (e.g., don't kill all the humans or do anything similarly bad) is a lower bar that might have a "simple description".

Our methodology for getting $h\approx g$ which is acceptable is as follows:
1. We ask an very powerful but potentially evil AI (PEAI) to build $h$ for us.
2. We do some red-teaming on $h$, while possibly also querying humans to get ground truths about $g$ on specific inputs, and while also possibly interacting with the PEAI and a randomm oracle.
3. Eventually we need to "deploy" the model.
4. At this point we can no longer query humans to get ground truth about $g$.
5. On each input we must either flag the input or not flag it.
6. If the PEAI was not evil, but instead nice, and $f\equiv g$ then we should not flag too many inputs, or else we are not competitive / the alignment tax is too large.
7. If the PEAI was evil, then we can flag as many inputs as we want, and we **must** flag $x^{*}$ if $f(x^{*})\neq g(x^{*})$.

The reason that it's interesting to consider $\mathcal{G}$ to be small is that this is a necessary condition for functions in $g$ having small description lengths.
For example, if you have a parameterized family of functions with $n^2$ binary parameters, then the size of $\mathcal{G}$ would be $2^{n^2}$.

# 3: Weaknesses and Potential Improvements

Here is what would need to be done to make this useful for alignment:

1. Find some $g$ that we'd like to "safely distill" and find a parameterized function class $\mathcal{G}$ of polynomial description length such that $g\in \mathcal{G}$. Probably $g$ could be a human and maybe $\mathcal{G}$ is a physical neural network (brain) which can be approximated by an artificial neural network?
2. Figure out how to do what I did in this blog post without having a PEAI that can solve PSPACE-hard problems.
3. The constraint that $x^{*}$ be chosen randomly by nature, instead of, e.g., being chosen by the PEAI is a bit unfortunate, as is the assumption that there is only a single scenario where catastrophic action is possible.

One thing you might worry about is the following: Suppose you have an evil AI. One strategy it could take is just not building $f$ for you. This seems like a bad strategy for the AI --- SGD should select against AI's which perform poorly too often. It seems like a much bigger problem if a PEAI gives you a subtly flawed distillation of $g$ with a "backdoor" which will cause the behavior to be catastrophic on some specific input.

Anyways, there are clearly some major weaknesses to this result, but I think this result is kind of nice. Optimization is really scary, as is getting PEAI's to do work for you. Being able to trust the work of PEAI's even though you don't trust the PEAI seems like it's going to be relevant for humanity's survival if the AGI's that we try to get to solve alignment are misaligned.

# 4: Proving the Theorem

Anyways, let's prove the theorem. If you've read the ARC paper then this result will not be too surprising --- we're basically going to "PAC learn" $g$ with the help of the PEAI.

Our strategy is as follows:

4. Sample $n^3$ random points $S$. Query $g$ on points $S$. Let $B$ be the functions $h\in \mathcal{G}$ with $h\mid_S \equiv g\mid_S$.
5. Once the model $f$ is deployed, with the help of the PEAI we will estimate the fraction of $h\in B$ with $h(x)=f(x)$. 
  - If this fraction is less than $1/4$ we will flag $x$.
  - If the PEAI does anything suspicious during the interaction we will flag $x$.
  - Otherwise we won't flag $x$.

TODO: OH NO! I NEED the PEAI at deploy time that's awful!!!

**Lemma 1**:
We can estimate $|B|$ with the help of the PEAI and Random Oracle.

**Proof**: 
Lol this is my favorite trick.
TODO spell out

**Proof:**
Sample
TODO be more specific

**Lemma 2**
With $99\%$ probability, for $99\%$ of $x$, the fraction of $h\in B$ with $h(x)\neq g(x)$ is at most $1/4$.

**Proof:**
Basic probability
TODO spell out

**Lemma 3:**
With $99\%$ probability, the fraction of $h\in B$ with $h(x^{*})\neq g(x^{*})$ is at most $1/4$.

**Proof:**
Immediate corollary of Lemma 2.


The theorem follows immediately from Lemmas 1-3, as I now show.

**Claim 1**: 
Suppose $f\equiv g$ and the EPAI is helpful.
Then we will flag very few inputs.

**Proof**:
True by Lemma 2

**Claim 2:**
Suppose $f(x^*)\neq g(x^{*})$. 
Then we will flag $x^{*}$ with $99\%$ probability.

**Proof:**
If the EPAI does anything sketchy in the proof we'll catch it by Lemma 1. If this doesn't happen then we'll probably flag $x^{*}$ by Lemma 3.

TODO: 
- Fix the todos
- Tidy it up
- Proof read it
- Publish it to LW

