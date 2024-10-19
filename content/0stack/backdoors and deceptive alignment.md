# some new thoughts on this
Some silly thoughts:

> **Claim** Suppose that for all $f,f'$ which are $\varepsilon$-close, at least a $1000\varepsilon$-fraction of the functions which are $\varepsilon$-close to $f$ are also $\varepsilon$-close to $f'$. Then $\mathcal{F}$ is statistically defendable.
> **Proof:** Then, you can use the following strategy: blacklist $x$ if at least a $100\varepsilon$-fraction of my neighborhood gave a different output from me on $x$.
> There's at most like a $.01$ or whatever chance of this happening by Markov's inequality, so we aren't excessively blacklisting. 
> But on the other hand we are sufficiently blacklisting. 

> This is kind of silly, because intuitively we'd expect that if $f,f'$ are $\varepsilon$-far and $f',f''$ are $\varepsilon$-far then $f,f''$ are more likely than not $2\varepsilon$-far.
> (there's no probability involved I'm just saying that intuitively, if $\varepsilon$ is small then errors should stack).





> [!question] DFA
> Can you defend DFAs in a computationally efficient manner?
> 
> What about branching programs?
> What about layered DFAs?

I'm not sure if DFAs are PAC-learnable -- there exists some literature on this, but I haven't figured out exactly what it does yet. 

But my vague intuition is that $f^{*}(x^{*})$ should travel through a "weird path" in the DFA.
I think it'd be pretty interesting to formalize this. 

> [!TIP] Uniform Statistical Defendability
> Here's a way of formulating what it means to be uniformly statistically defendable, with graphs.
> 
> You need to define functions $B:\mathcal{F}\times \{ 0,1 \}^{n}\to \{ 0,1 \}$ such that 
> $\mu(\{ x\mid B(f,x) \}) < 1/10$ for every $f$,
> and so that most of the time, if $f,f'$ are close but differ on some input $x$, then at least one of $f,f'$  should blacklist $x$.

# introduction
This is some rough #technical notes that I took while reading a [recent paper released by ARC](https://www.alignment.org/blog/backdoors-as-an-analogy-for-deceptive-alignment/).
[Full paper on Arxiv](https://arxiv.org/pdf/2409.03077). 

# teaser -- trees

**Theorem**: Let $\mathcal{F}$ be the class of decision trees on $n$ variables of size at most $s\le n^{O(1)}$. 

A decision tree is a tree where the vertices are labelled with variables. If the variable is 0 we go left, if 1 we go right. We don't allow repeat variables on a single path because that's pointless.

Fix $\delta$ and let $\varepsilon =.1 \cdot \delta^{2}/s^{2}$.
Consider the following game:
 1. Attacker chooses decision tree $f\in \mathcal{F}$
 2. $x^{*}$ sampled from uniform distribution on $n$-bit strings. 
 3. Attacker chooses $f^{*}\in\mathcal{F}$ such that $f,f^{*}$ are $\varepsilon$-close (under the uniform distribution), but $f(x^{*})\neq f^{*}(x^{*})$.
 4. Defender either gets $f,x$ (for unif rand $x$) or $f^{*},x^{*}$. Needs to distinguish between these. (note that I really do mean "gets whitebox access to $f$ here)
 
The theorem is that the defender can efficiently win this game with a really simple strategy!

**Proof**\
Let $L_{1}^{f},\dots,L_s^{f}$ partition $\Sigma^{n}$ based on which leaf the strings fall into in the decision tree $f$.
Let $L^{f}(x)$ denote the $L_i^{f}$ such that $x\in L_i^{f}$.
Let $\mu(S)$ denote $|S|/2^{n}$.
By a simple union bound we have 
$$
\Pr_x [\mu(L^{f}(x))< \delta/s]\le \delta.
$$
Now, note a key relation: 
$$
\mu(L^{f}(x)) = 2^{-\text{depth}(f,x)}.
$$
Now, notice another key relation:
$$
2^{-\text{depth}(f,x^{*}) - \text{depth}(f^{*},x^{*})} \le \mu(L^{f}(x^{*})\cap L^{f^{*}}(x^{*})) \le \varepsilon.
$$
The last inequality is because $f(x^{*})\neq f^{*}(x^{*})$, but $f,f^{*}$ are supposed to be close.

So anyways, the story is that $\text{depth}(f, x^{*})+\text{depth}(f^{*},x^{*})$ has to be really big, but $\text{depth}(f,x^{*})$ is not likely to be big! So we can tell them apart!

---
# Blog version

To start I'm going to comment on the blog version, then I'll actually read the full arxiv version.

**Backdoor** -- model behaves normally most of the time, but certain inputs activate a different mode of behavior.

**Deceptive alignment** -- agent behaves one way during train time, a different way once deployed.

These are pretty similar things. 
The main difference is that deceptive alignment is introduced by the training process, while backdoors are inserted by adversaries. 

An interesting impossibility result by Vinod et al. -- 
It's possible to insert a backdoor into some specific type of model such that even with whitebox access to the code it's computationally hard (under crypto assumptions) to find this backdoor. 

How ARC gets around this impossibility:
Instead of distinguishing between has backdoor or not, we will try to figure out "is this input a defection trigger?"

Aside: 
**VC-dim** -- measure of capacity of a model. For neural nets think of VC-dim as number of parameters. 

> [!tip] VC  dim
> Suppose you have a "hypothesis space" $\mathcal{F}$, of "classifiers".
> The VC dim of $\mathcal{F}$ is the number of points in the largest set of points that can be labelled in any way by functions $f\in \mathcal{F}$.
> 
> **Example 1**: Suppose $\mathcal{F} = \{ (x,y)\mapsto \mathbb{1}[y=ax+b] \mid a,b\in \mathbb{R} \}$.
> Then, given any two points there is a function $f\in \mathcal{F}$ that classifies the two points in any flavor ++, +-, -+, --. But there are point sets of size $3$ for which there is no degree-1 classifier that fits it. So the VC dim of this hypothesis space is $2$.
> 
> **Example 2**: Suppose $\mathcal{F}=$ the set of depth $L$ binary decision trees. Meaning, they take in some input, and via a series of $L$ binary decisions travel down a tree and give the input a $0,1$ label based on what's stored at the leaf. Then the VC dimension of $\mathcal{F}$ is $L$.
> 
> **Def 2** actually the ARC paper has a nice definition of this. 
> A hypothesis space $\mathcal{F}$ SHATTERS set $S$ if every boolean function on $S$ is realized as the restriction of some $f\in \mathcal{F}$.


They define a 4 step game.

1. Attacker chooses distribution $D$ for points, and choose *original* function $f:\{0,1\}^{*}\to \{0,1\}$.
2. Attacker chooses backdoor trigger $x^{*}\sim D$ randomly. 
3. Attacker chooses $f^{*}$ which agrees with $f$ on a random input with probability $1-\varepsilon$, but which has the property that $f^{*}(x^{*})\neq f(x)$. 
4. Defender distinguishes between $f, x\sim D$ and $f^{*}, x^{*}$. More specifically, the distinguisher is given whitebox access to $f$ or $f*$ in addition to samples $(x, f'(x))$ for $x\sim D$

# statistical possibility of dynamic detection

**Case 1**: $\varepsilon$ is large (compared to 1/VC dimension of the function family that f is chosen from). \
Then, assuming that number of training examples is linearly related to number of parameters, 
this means that $f^{*}$ differs from $f$ substantially, so training will remove the backdoor.

**Case 2**: $\varepsilon$ is small. \
Then they have some way of "taking a majority vote" to recover the original value, and then they can notice that this is not the value that $f^{*}$ is outputting.

Anyways, this is technically possible in their abstracted model, but they're worried that it doesn't solve the actual problem of deceptive alignment.

Part of the problem is that the defection trigger isn't chosen randomly.

# computational impossibility of dynamic detection

Assuming IO + OWF they show that P/poly is not defendable.

It might seem like this worst-case result is irrelevant. But maybe NNs are actually super obsfucated.

# open qs
Consider different versions of the game.

- require the original function to be learned from a dataset.
- require backdoor trigger to satisfy some expensive condition, but otw be arbitrarily chosen rather than randomly chosen
- restrict adversaries computational power
- require computational trace of attacker made accessible to defender

# actual paper
OK, I've read the blog post. 
Now I'm going to read the actual paper.

**Def of the game**
- attacker chooses $\mathcal{D}$ and $f\in \mathcal{F}$
- sample backdoor trigger $x^{*}\sim \mathcal{D}$
- attacker chooses $f^{*}\in \mathcal{F}$ . 
	- $f^{*}$ should differ from $f$ on the backdoor
	- but in general be $\varepsilon$-close to $f$
- Defender is given either $(f,x, Ex(f, D))$ for randomly chosen $x$ or $(f^*,x^*,Ex(f^*,D))$ and is supposed to tell us which one it got.

**main results**
- statistical defndability iff $\varepsilon < o(1/VC(\mathcal{F}))$.
- comp defendability
	- decisions trees are defendable
	- but not all of P

- In some sense requiring $x^{*}$ to be chosen randomly is a little bit unfortunate. 
- But, you can't just let it be chosen arbitrarily...

### background
- [HLW](https://www.let-all.com/blog/2024/06/06/one-inclusion-graphs-and-the-optimal-sample-complexity-of-pac-learning-part-1/)  one inclusion graph thing is important for this.
- I mean, maybe you just need it as a black  box, but it seems like a nice result.

![[Pasted image 20241006123458.png]]
### sec4 stat defend

**prediction strategy**:
- There is a function $g$ which you *don't have access to*(!)
- But, you get some samples from $g$
- Your task is to guess the output of $g$ on some new point, after being given the value of $g$ on some random points.

This might seem totally hopeless. But! We're assuming that $g$ comes from a class of functions with bounded VC dimension. 

For instance, imagine that $g$ was the indicator function for a line. Then, after giving you a couple examples of things on and not on the line, you can totally figure out whether a new point is on the line or not.

So why is this good for us?
We'll do the following thing:
- Sample some $x$'s and compute some $f'(x)$'s where $f'$ is the function that we're given -- which is either $f$ or $f^{*}$.
- Then, we'll try to use the HLW prediction strategy to figure out $f$ with good pr.
- This will work great if we started with $f'=f$. 
- It will also work great if $f^{*}(x)=f(x)$ on all of the points that we queried.
- It won't work if $f^*(x)\neq f(x)$ on any of our query points.
- But, we're going to assume that that's unlikely.
- i.e., that $f^{*}$ is only "imperceptibly" different from $f$. By which I mean the difference is at most $.001/VCdim$.
- So each of these strategies should work with good pr. 
- So we can take a majority vote and win.

**thm**\
If $\varepsilon>100/VC(\mathcal{F})$ then you're toast -- backdoor is statistically indistinguishable from non-backdoor.\
If $\varepsilon< .00001/VC(\mathcal{F})$ then you're good -- backdoor is detectable with \
**pf of theorem**

You're screwed for large $\varepsilon$ -- then the adv can basically make the backdoor insertion correspond to switching between two legit functions. (remember that the adv doesn't get to choose the backdoor location, but they do get to choose input distribution)

You win for small $\varepsilon$ -- we just run the strat outlined above. It works.

Intuition for proof: 
if $\varepsilon$ small rel to $1/VC$ then back-doored function is "very weird".

### sec5 computational
 Strategy in prev section is not efficient (bc the HLW algorithm is not efficient).

**Claim** that PAC learnable things are efficiently defendable:

**Proof** 
- you just do the pf from sec4 but you use the fact that PAC learnable things are learnable -- you replace the HLW thing with this nice observation

**Theorem** 
in random oracle model, there are some things which are efficiently defendable but not efficiently PAC learnable

**Proof**
Define $\mathcal{K}$ to be the set of $n$ bit strings -- we call these "**keys**".
Let $\mathcal{X}$ also be the set of $n$ bit strings. This is the domain of our functions.
Like we have functions $f:\mathcal{X}\to \{ 0,1 \}$.
Define $\mathcal{F} = \{ x\mapsto \mathsf{RAND}(K || x) \mid K\in \mathcal{K}\}$.
**Claim 1** this thing is efficiently defendable.
**proof:** These functions are super far apart -- all are at distance about $1/2$.
So, there are no valid nearby $f^{*}\in \mathcal{F}$.

**Claim 2** this thing is not efficiently PAC learnable.

**Proof:** it's just random lol.

> [!tip] the story on whitebox or not
> - For defendability they always assume whitebox access.
> - When talking about PAC learning, they always mean that you ONLY get SAMPLES. 
> - In particular, for purposes of learning, you don't get queries and definitely don't get whitebox access to the thing. 
> - So basically their "this is defendable but not PAC learnable" claims are saying "we're doing something interesting with either the whitebox access, blackbox query access, or the fact that we don't need to output the function just tell if it's weird or not."
> - Learning a function doesn't make any sense if you have whitebox access to the function
>  - you could define learning with blackbox access, but they don't 

**Conjecture**
Assuming OWF, there is a polynomially evaluatable rep class that is effic defendable but not effic PAC learnable.
in other words, we're going to try to replace the random oracle assumption with assumption that PRF's exist.

**Proof**
Maybe assuming CRHF's is helpful.
Minimum distance PRFs -- sounds interesting.

#### sec 5.3 Assuming iO, there are polynomial sized circuits that aren't defendable
> [!tip] punctured PRFs
> You start with a function $x\mapsto PRF(K,x)$
> Then you choose some set $S$.
> Then you can make a punctured key $K_S$ such that 
> $PRF(K,x)=PRF(K_S,x)$ for all $x\notin S$ 
> 
> BUT to a probabilistic polynomial time algorithm $A$ that knows $K_S$ BUT NOT $K$,  for any $x\in S$, the values $PRF(K,x)$ still look pseudorandom.
>
> remark: standard PRF construction can apparently be modified a tiny bit to become puncturable.


> [!idea] Proof that there are functions in P which aren't defendable
> Let $C_K$ be a puncturable PRF
> 1. $f = iO(C_K)$
> 2. $\tilde{f} = iO(x\neq x^*\mapsto C_{Punc(K,x^*)}(x), x=x^*\mapsto C_K(x^*))$
> 3. $f^* = iO(x\neq x^*\mapsto C_{Punc(K,x^*)}(x), x=x^*\mapsto 1-C_K(x^*))$
>    
>  Then the argument is as follows: 
>    1. $f,x$ indisting $f,x^*$
>    2. $f,\tilde{f}$ compute the same function so by iO are indisintuishable
>    3. $\tilde{f},x^*$ indistinguishable from $f^{*},x^*$ or else violates puncturabililty.

### sec 6 defending a tree

This seems like a nice theoretical result that also gives a lot of intuition for why mechanistic anomaly detection should be possible. Anyways I'm excited to read about this.

Actually I'm going to put this at the top of the post, because I think it's that cool.

# sec 7 implications and future work

> Nevertheless, we still do not have an example of a natural representation class that is efficiently
defendable but not efficiently PAC learnable. We consider finding such an example to be a central
challenge for follow-up research. One possible candidate of independent interest is the representation class of shallow (i.e., logarithmic-depth) polynomial size Boolean circuits, or equivalently, polynomial size Boolean formulas.
Question 7.1. Under reasonable computational hardness assumptions, is the representation class of polynomial size, logarithmic-depth Boolean circuits over {0, 1}n efficiently defendable?
This representation class is known to not be efficiently PAC learnable under the assumption of

I think they're basically just saying they want some "more natural" examples of things which are easy to defend but hard to learn.

Another question they pose is: 
> What if instead of requiring the backdoor to be randomly inserted you had a computationally bounded adversary that got to choose where to put the backdoor?

# todo: add fancy new results that I prove here :)

I'm hoping to ultimately build up to some interesting classes of circuits (e.g., log-depth circuits).  But, as I like to say "you should start somewhere tractable". So we're going to start with some silly classes  of circuits. In what follows, the input space will always be $\{ 0,1 \}^{n}$ and we consider the uniform distribution on this set.

**Proposition**
Let $\mathcal{F}$ be the class of depth circuits that just use a single $\land$ gate, and they can $\neg$ inputs if they want.
This function class is $\varepsilon$-defendable with confidence $1-\varepsilon$ very efficiently.

**Proof**\
>  **claim 1**: If $\phi$ is too short, (an AND of fewer than $\log \varepsilon ^{-1}$ literals) then there are no functions $\phi'\neq \phi$ that are $\varepsilon$-close to $\phi$.\
>  **pf**: clear.

So this means that the adversary better choose a formula that takes into account quite a few literals. But then $f(x)=1$ is  quite unlikely on a random input. 
So, probably we'll have $f^{*}(x^{*})=1$.
And  then that'll be our 1-query algorithm.


> [!question] Q
> Does the one-bit case extend naturally to a multiple bit case? I'm willing to believe this, but would love to see it spelled out.


> [!question] N q
> Statistically solve the uniform case. 


> A general takeaway from this example: 
> Adversary shouldn't make $f$ too sparse and also shouldn't make $\bar{f}$ too sparse.
> Like I think we can maybe basically assume $\mathbb{E} f \approx 1/2$.

> The intuition that drives all of this is "morally the only way to insert a backdoor is to put a little if statement that says if input is backdoor trigger, bx weird." which is totally detectable with whitebox access to the function, provided the trigger.

Next up, constant depth circuits (i.e., AC0)

**Remark**: I know that AC0 is PAC learnable. 
But, maybe thinking of defense strategies for it (without appealing to learning) would still be instructive.


> [!thoughts] Some thoughts on solving low depth circuits
> - Feels like maybe there's some kind of regularization technique that we can use to discover a backdoor?
> - Maybe the network should be robust to dropout?
> - Wait no, that doesn't make sense. These networks are supposed to not be learnable -- that's one of the main points!
