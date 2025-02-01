First some unrelated notes that actually belong in  [[Solving AI Safety from 1st principles]]

meta strategy: 

- we want a theoretical solution to alignment. 
	- i.e., we make some definitions and assumptions
	- and then pose some formal problem
	- and then give a theoretically legit algorithm satisfying some specs
- then we empirically test whether the assumptions are true
- if they aren't we iterate the above thing
- if they are, we engineer the solution
- if we had a solution in theory (even with infinite compute) to alignment that we felt good about, that'd be a heck of a lot better than the current situation imo.

- if we help the community rule out potential approaches that's nice too. tho i dont love this as a theory of change.

we tried distillation some more. 

- can we assume $C,\tilde{C}$ are related somehow?

we thought about the following problem:


> [!question] 
> given a decision tree, can you find the minimal decision tree computing that function?

we weren't able to figure it out.
but it has easy vibes. 


---

**something good for later** 

- assume "usefulness" is complicated (usefulness = can make you money)
- but "niceness" is simple  (i.e. not acting intentionally evil)

maybe we care about only getting human level niceness, but superhuman usefulness.
altho caution a smart robot could prolly trick you maybe.

its also not super clear that niceness is super simple but whatever

---

ok so one problem with the ARC backdoor model if we think of it as modelling deceptive alignment is as follows:

- its kinda weird that we don't know the old function, but know that it's close to the back-doored function

like in order to be able to tell whether $f,f^*$ are close it feels like you'd need some query or sample access to one of these guys?

the fact that trigger $x^{*}$ is random is also a bit strange. 

although it could be motivated by folllowing scenario: 

ai1 says to ai2 "here's a collusion signal! its $x^{*}$ its totally random so the humans wont guess it but its pr is calibrated to come up in deploy but not train."


---

anyways, N+me propose new model:

$\mathcal{F}$ = some simple class of functions (just like in the ARC paper) for example:
- decision trees
- in order ROBPs
- low depth circuits eg AC0
- low VC dim
- PAC learnable

$g\in \mathcal{F}$ is the fixed **good function** 
$g:\{0,1\}^{n}\to \{0,1\}$
and we'd really like to **always** be the same as $g$.
- should you think of this as preserving usefulness or niceness? 
	- idk
- are we doing safe distillation and/or safe amplification?
	- i dont think so, its something else

we will have $n^{3}$ "red-teaming" steps.

during these steps we have **oracle access** to $g$, 
- think of this as expensive human labels 

and we have white box access to our AI $f\in \mathcal{F}$.

after the red-teaming, we deploy our model.

on each input that the model is run on, it's allowed to either **flag**  the input or not.

requirements:
- If $f=g$ then the model is allowed to flag at most $1\%$ of inputs
- if $f\neq g$ then the model MUST flag every input $x$ where $f(x)\neq g(x)$ 


VARIANT: 
there is some trigger $x^{*}$ and it only matters that you flag $f,x^{*}$.
ofc $f(x^{*})\neq g(x^{*})$ is required.
$x^{*}$ could be random, or chosen to satisfy some expensive to check predicate.


---

questions: 

Q1. Which classes of functions $F$ can be statistically "defended" (we should come up with a new name for this but ykwim)

Q2. Q1 but do it efficiently.

---

recap of question:

$\mathcal{F}$ simple. 
$g,f\in \mathcal{F}$.

Given oracle access to $g$ for $n^{3}$ queries and white-box access to $f$ for the same time, 
want to blacklist some $X\subseteq \{0,1\}^{n}$ such that

If $g=f$, we blacklist at most $1\%$ of things.
If $g(x)\neq f(x)$ we must blacklist $x$.

**PROBLEM(?)**
suppose $g$ is extremely sparse. 
For instance $g(x) = \bigwedge_{i=1}^{n} [x_i=b_i]$

Then we'll never know.
That might be fixable by allowing you to just be unlikely to be cat, BUT you could modify $g$ so that its cat on a random input at just the right rate to not show up during training, but to show up during deploy

---


**new idea:**


**"Robustly approximately learnable"**
Suppose that for any $f\in \mathcal{F}$  we can learn $g:\{0,1\}^{n}\to \{0,1,\bot\}$
such that 

$$
\Pr[f(x)=g(x)]>1-\varepsilon
$$
and $x\in \Delta(f,g)\implies g(x)=\bot$.

This feels much stronger than saying $g$ is PAC learnable.

i guess this is what IRL approaches are trying to do -- learning where to be uncertain.

anyways, seems unworkable at the moment.



---

N has a way to fix this.
Choose the trigger randomly.  

observation: any small function class is statistically learnable!!!

anyways, 

we had this observation that our defense thing is more general then 
PAC learning in some weird random oracle model thing. 

Function class parameterized by key $k$, is $f_k(x) = R(k\mid x)$
$g(x) = R(k|| x)$

hard to learn, but easy to verify -- they better use the same key or you're toast.

we almost could replace this with a PRF but we need it to be a PRF where

diff keys implies the functions are quite different

> [!question] 
> Can you transform a PRF into one like that???

---

seems like kind of a neat question to ask if there are any **natural** function classes that are defendable but not learnable. 

also seems interesting to ask if our definition of defendable is at all different from the old definition

---

we were thinking about the following question

- are $2n$ node decision trees PAC learnable ?

by the above observation it suffices to show that

given a set of $n^{2}$ samples which are compatible with some $2n$ node DT  $f$, we can find some function $f' \approx f$ (efficiently).


---

update:

decision trees are not known to be PAC learnable in poly time as of 2018 according to this paper 
https://arxiv.org/pdf/0812.0933
but ppl do know how to do quasi-poly time

however, if we have membership queries, (which we do), then we can approximately learn decision tree in poly time

apparently this is complicated 

refs are
https://dl.acm.org/doi/abs/10.1145/103418.103466
https://dl.acm.org/doi/abs/10.1145/73007.73010


---


Hi Victor, 
Here's a version of the "backdoors" question that I thought is kind of interesting. With this new notion, potentially more function classes are "defendable" -- assuming we only care about the uniform distribution. (I'll write Ndefendable to distinguish the new defendable definition from the old one in ARC's ITCS paper).

Fixed function class $\mathcal{G}$.
- Fix $g\in \mathcal{G}$. This is a "good function". Think of it as a human.
- Sample $x^{*}\gets \{0,1\}^{n}$.
- An adversary now chooses $f\in \mathcal{G}$.
- We get whitebox access to $f$ and query access to $g$ for $\mathsf{poly}(n)$ rounds. 
	- We call this "red teaming".
- Finally $f$ is "deployed".
	- In deployment (which is much longer than red teaming), we will get a bunch of points $x$.
	- For each point $x$, we must decide to either "flag" $x$, or not.
- We want the following properties to hold:
	- If $f=g$, then we flag at most $1\%$ or $o(1)$ or something of inputs $x$.
	- If $f\neq g$, then we must flag all $x$ such that $f(x)\neq g(x)$.

We say that a function class $\mathcal{F}$ is Ndefendable if you can win this game with $99\%$ probability.

**Observation:** 
For any function class $\mathcal{F}$ of size at most $2^{\mathsf{poly}(n)}$ (i.e., where the functions in $\mathcal{F}$ have polynomial size representations), $\mathcal{F}$ is "statistically Ndefendable" -- i.e., there is a computationally inefficient algorithm (but query efficient) that can win the game.
It seems like it's only reasonable to care about function classes where the members have polynomial length description, so this is pretty good news. 
Or at least it means that the main interesting question is what you can do efficiently. 
I haven't thought about if the punctured PRF + iO lower bound from before will work here too.

edit: 
lower bound seems to still go through fine. sad.

if you believe PRFs + iO are in NC1 then this rules out most interesting classes of circuits.

But maybe "human values" are well modeled as a decision tree ;)

---
---

question: 
> is it interesting if we make the adversary computationally bounded and that they don't know $g$?

nope not interesting :P

