# introduction
In this #technical post I'll discuss a [recent paper released by ARC](https://www.alignment.org/blog/backdoors-as-an-analogy-for-deceptive-alignment/).
[Arxiv](https://arxiv.org/pdf/2409.03077). 
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
4. Defender distinguishes between $f, x\sim D$ and $f^{*}, x^{*}$.

# statistical possibility of dynamic detection

**Case 1**: $\varepsilon$ is large (compared to 1/VC dimension of the function family that f is chosen from). \
Then, assuming that number of training examples is linearly related to number of parameters, 
this means that $f^{*}$ differs from $f$ substantially, so training will remove the backdoor.

**Case 2**: $\varepsilon$ is small. \
Then they have some way of "taking a majority vote" to recover the original value, and then they can notice that this is not the value that $f^{*}$ is outputting.

> Q: This probably requires some assumptions on what $\mathcal{F}$ looks like?

Anyways, this is technically possible in their abstracted model, but they're worried that it doesn't solve the actual problem of deceptive alignment.

Part of the problem is that the defection trigger isn't chosen randomly.

# computational impossibility of dynamic detection

They make some crypto assumptions. Maybe IO?
It might seem like this worst-case result is irrelevant. 
But maybe NNs are actually super obsfucated.

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
- defender tries to distinguish $f,x\sim D$ and $f^{*},x^{*}$.

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

**Conjecture**
Assuming OWF, there is a polynomially evaluatable rep class that is effic defendable but not effic PAC learnable.
in other words, we're going to try to replace the random oracle assumption with assumption that PRF's exist.

**Proof**
Maybe assuming CRHF's is helpful.
Minimum distance PRFs -- sounds interesting.

#### sec 5.x
If you obsfucate something then that's bad maybe.

### sec 6 defending a tree
They just check the depth

# sec 7 implications and future work

> Nevertheless, we still do not have an example of a natural representation class that is efficiently
defendable but not efficiently PAC learnable. We consider finding such an example to be a central
challenge for follow-up research. One possible candidate of independent interest is the representation class of shallow (i.e., logarithmic-depth) polynomial size Boolean circuits, or equivalently, polynomial size Boolean formulas.
Question 7.1. Under reasonable computational hardness assumptions, is the representation class of polynomial size, logarithmic-depth Boolean circuits over {0, 1}n efficiently defendable?
This representation class is known to not be efficiently PAC learnable under the assumption of


I think they're basically just saying they want some "more natural" examples of things which are easy to defend but hard to learn.


