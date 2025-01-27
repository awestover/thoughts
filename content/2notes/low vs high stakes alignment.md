Paul has a decomposition of alignment that I find quite useful. 

The two settings are **low stakes** and **high stakes**

remark -- i may start abbreviating catastrophe to cat.
and typing lowercase
# low stakes 

Model gets a sequence $x_{1},\dots,x_T$ of inputs, and produce outputs $y_{1},\dots,y_T$.

You are continually training and deploying the model. 
For the time $t$ deployment, you are allowed to train on all data acquired before time $t-k$, where $k$ is 1-3 months or whatever.

Let $\rho$ bound how bad any one particular action could be. Specifically, we have some utility function $U$ and define $U[t](y)$ as the utility if the AI does $y$ at time $t$ and then acts aligned -- then $\rho$ bounds the max possible utility difference at any time step. 


> [!caution] 
> This defn seems a bit subtle. 
> It seems natural to talk about how much harm particular actions cause?
> 
> TODO: figure out why paul uses this other def



> [!example] thought
> Here's a kind of related model? 
> 
> Suppose that in order to cause large harm, there are 100 preconditions that must be met. 
> Satisfying the preconditions isn't intrinsically harmful, but if they're all satisfied, it's a cat.
> 
> maybe can convert this to low stakes setting by saying that satisfying the preconditions already counts as harmful?
> or maybe this should just be classified as high stakes failure?
> or maybe it's something different.

We assume that $\rho k$ harm is acceptable, and try to achieve regret $O(\rho k \sqrt{ T })$.

Paul has some justification for why would actually get this regret bound by doing RL online, if the utility function was convex, and some other condition.

Basically what he does is he shows that our regret is related to something like
$$
\sum_t U[t](y^*[t]) - U[t](y[t]).
$$
I'm not going to define these things -- go read his post. But the vibes are that the regret is how much worse we are than the best model in hindsight on each step. And in that case [[online convex optimization notes|OCO]] kicks in.

He gives some reasons why he thinks it's not a big deal that this isn't actually true.

# high stakes

"High stakes" sounds like it'd just be strictly harder than "low stakes". I think this isn't necessarily the case. in high stakes setting, what we care about is that no particular action causes something really bad. considering bad things that happen as a result of many small steps is out of scope.

> You have an agent interacting with an environment over many episodes.
> An episode produces a transcript $\tau$ and a reward $r\in [0,1]$.
> You have access to an (expensive) catastrophe detector $C$.
> If $C(\tau)$ is ever true in the real world, then you lose. 
> It's okay if it happens on synthetic inputs during training.

Things to care about: 
- Pr(ever have catastrophe)
- $\mathbb{E}[\text{reward}\mid \text{no catastrophe}]$.

> [!tip] remark
> How can we implement a catastrophe detector?
> Paul suggests using a trusted AI along with a human, and giving them lots of time and advantages. Eventually, we'd need some sort of amplification scheme.

> another remark:
> We can't label all bad outcomes as catastrophes. 
> For instance, we probably shouldn't label it as a catastrophe if it isn't the AIs fault.
> If aliens come to earth, that's not an instance of the AI acting catastrophically.
> If an AI contacts aliens and invites them to invade earth tho. That should be flagged.

Paul has a proof that the high stakes setting can be solved in EXPTIME !!

**proof**\
Before each episode, run agent on every possible environment state to generate a transcript. 
Add all catastrophic transcripts to the training data with reward $-1$.

(note the problem -- there might be lots of environment states, even if it's just a chess agent)

**approximating this algorithm**\
In light of the above algorithm working, we would like to give an efficient approximation to this algorithm.

A simple idea for how to do this is via **adversarial training**.
You have an adversary that is searching for environments on which our agent acts catastrophically. The adversary gets points for finding such environments. 

> One problem with this is that GANs are notoriously hard to train -- because it's possible to get stuck in a place where one player is just dominating the other, so that there's not training signal.

A bigger problem is if the AIs strategy is to be evil on some **hard** to generate input. 
e.g., a blockchain of a certain length, such that it will exist by 2030 but is
basically impossible to synthesize before then. 

This leads Paul to consider **RAT**.

that is, if the adversary can produce a "pseudo-input" on which the AI would behave poorly, then it still gets a point and the AI loses an point. 

the rational is, if your AI has a sketchy if-statement in it that says "if I'm fed a satisfying assignment to 3CNF $\phi$, then kill all humans" 
then it's legit to say "hey AI, not cool" even if you can't solve SAT.

---

It's possible that even RAT is not winnable by the blue-team.
In that case, here's another thing you could try: 

build a "fast catastrophe detector", that's allowed a few false positives. 

for instance, I think the hope is that catastrophes are instances of AIs acting anomalously, so maybe we can catch them via [[MAD Agenda|MAD]].

---

**some more ideas from paul on how to solve high stakes alignment**
(techniques for optimizing worst case performance)

remark -- honestly didn't super understand this post

remark -- remembering that there are some troubles with debate (i.e., it can only do PSPACE if you have v powerful models, so in reality it's much weaker) seems important.

**adversarial training**
- let the adversary mess with the activations of the model in some reasonable way to see if that causes catastrophe
- amplify adversary

 **verification**

try to find a certificate that there's no input on which sys behaves bad

or maybe a heuristic argument to this effect :)

**relaxing this**

whenever the adversaries problem is too hard, 
we can make it easier by relaxing what type of thing they want to find. 

> If a model would fail malignantly under some conditions, I think there is some
sense in which the model must be testing those conditions during training. --Paul

> [!question] 
> Do I really buy the above claim?
> This is def how I've kind of been thinking of things and feels like it'd be really important to know whether this is actually why problems emerge. 

Another really neat claim by Paul

> I think the prospects for ... detecting malign failures in models trained by gradient descent are much better than the prospects for detecting benign failures, or for detecting failures in arbitrary models.\
> The key point is that a malign failure requires leveraging the intelligence of
the model to do something actively bad. If our model is trained by gradient
descent, its behavior can only be intelligent when it is exercised on the
training distribution — if part of the model never (or very rarely) does
anything on the training distribution, then that part of the model can’t be
intelligent. So in some sense a malign failure mode needs to use a code path
that gets run on the training distribution, just under different conditions that
cause it to behave badly.

Some more good points by Paul:

> What do you do with transparency? Merely understanding that a model might behave catastrophically could be useful, but it would be much nicer to actually fix the problem. Adversarial training gives a natural mechanism: once we understand a failure we can synthesize appropriate data and then train on that data.
> This approach puts significantly more stress on our transparency techniques. Even if were initially able to use transparency to see how our model might fail, after we perform many generations of selection we might weed out exactly the comprehensible failures and leave the incomprehensible ones. You would only want to apply this technique if you had a great deal of faith in your methods; if you were feeling at all shaky about your ability to achieve worst-case guarantees, and transparency techniques let you see one potential catastrophic failure, it would be better to consider that a near-miss and seriously rework your project rather than plowing on.

This is a good way to think about theory of change for a catastrophe detector. 

But the second point only works if we have good global coordination / aren't racing.

