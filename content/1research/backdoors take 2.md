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

given a decision tree, can you find the minimal decision tree computing that function?

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


