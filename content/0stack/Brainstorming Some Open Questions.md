> I'm going to try to clean this version up and make them into actually good questions in [[MAD Agenda]] + [[LPE Agenda]].
$$ \newcommand{\E}{\mathbb{E}} $$
**Given a function (NN) can you efficiently learn the dependency structure?** 
E.g., I have some budget of complexity for a graphical model that I want to fit to the structure. 
Is it tractable to fit / approximately fit something like this?
Hope would be to define a loss function for how good your fit is, and then if your model is getting high loss it should mean that it's easy to find additional structure that could substantially improve the loss. This is why you might hope that this is tractable. 
The algorithm would be:
```python
while loss is trash:
	use reason why loss is trash to update model
```

**Factored Cognition Hypothesis**
How possible is it to factor cognition?
e.g., how viable does HCH seem?

**Fixing Causal Scrubbing** see [[Causal Scrubbing Notes]]
Sometimes ignoring correlations helps the model.
(todo: understand this more crisply / writeup Alek-understandable explanations of their problems with CS)

*potential solution:*
- try to see if adding any additional considerations substantially affects the model.
*problem*:
- What if the hypotheses are defined relative to different incompatible representations of the function $f$ (circuits that both compute $f$ but are set up in pretty different ways? Then the joining is not clear how to do. 
todo: think about this

Q: is it possible to make CS efficient?
Seems like the current algorithm is very not efficient.

**Activation Modelling Issue** 
- "Layer by Layer forgets too much" -- how to fix this issue?

**LPE**
LPE is a nice problem because it's fairly well-defined.
Well, I guess you need a definition of what the model looks like and what the training process is, but besides that it's quite concrete. 

**independent linear features**
todo -- read Paul's note on analytic VAEs. 
Is there some reasonable way to replace KL divergence with something that focuses on catastrophes more?

**Cumulant propagation**
This is some algorithm for doing heuristic estimation. The main problem that they have with it is that it gives negative estimates for $\E[X^{2}]$ sometimes. Is this fixable?

**Eric's SAT counter example --- "Probability distributions over activations are too restrictive"**
 > Either, the model must only put positive probability on activations which could have been realized by some input
> Or, the model must put positive probability on an inconsistent set of activations.

 **More backdoor questions**
- We could require the original function to be learned from a dataset. (If we allow the attacker to alter a certain fraction of the dataset, then we recover the well-studied data poisoning setting.)
- We could require the backdoor trigger to satisfy some formal predicate that is expensive to check, but otherwise allow the attacker to choose the backdoor trigger.
- We could restrict the computational budget given to the attacker.
- We could require the computational trace of the attacker to be made accessible to the defender, as an analogy for observing the training process. (OR something else...)
-  **Nathan statistical question about backdoors** 
-  **Read Neekon's smoothing thing, and also ARC's backdoor Boltzman thing**

**A random question (not alignment related)**
- Binary alphabet case of detecting coding errors against bounded adversaries.