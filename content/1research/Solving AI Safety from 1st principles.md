This document summarizes discussions with Nathan Sheffield (hencetoforth denoted by N; not to be confused with "Neural Net", abbreviated NN) about "solving AI safety". I'll probably use the word "I" throughout the document, but suspect N would endorse many of the things that I say.

yes i know that this is a big problem -- guess it calls for a big blog post. 

**Meta research strategy comment**

I want the future to be [[goodness(universe)|good]]. In order for this to happen, we need to figure out some solution to the [[Risks from AI -- elevator pitch|AI problem]]. This is a big problem. Some people have spent ~10 years thinking about it, and have identified some sub-problems which seem helpful for solving the original problem, while being potentially more tractable. The most obvious way to work on AI safety is probably to choose someone who seems smart, and start thinking about some node lower in the tree that they've identified as helpful. There are clear benefits to this approach, such as getting started closer to concrete questions where its clear what progress looks like, and the fact that plausibly it's better to thoroughly investigate a couple approaches then partially investigate a bunch. 

Another approach is to start by just staring at the problem, and then try to figure out how to break the problem up yourself. This also has some benefits. Hopefully by doing this, you'll discover why people thought certain parts of the tree were important to look at -- and conclude that certain parts of the tree aren't really worth looking at. Doing this exercise seems pretty important for being able to generate good concrete questions to resolve the ultimate question we care about.

More research methodology notes:
- We will try to move down the tree as rapidly as possible.
- Our main method for doing so is making strong simplifying assumptions.
- Once we make enough of these, the problem will generally either become trivially true, or obviously impossible. Rarely, the problem might become interesting (i.e., not obviously true or impossible).
- The plan is to rapidly backtrack out of impossible branches of the tree, and to similarly backtrack out of branches of the tree where our assumptions are too strong, so that the problem no longer tracks reality.

---

#### 1. Some basic definitions

> I would like to construct an **efficient** **useful** **optimization algorithm** over **neural networks** that produces NNs that, when run, lead to a **good** future.

**definitions:**

efficiency and usefulness measure the "alignment tax" of our alignment strategy.

**efficient:**\
whatever we do must be possible in polynomial time.
it's sometimes useful to start by relaxing this assumption and ask what we would do existentially.

in fact, it's even problematic if our solution costs a large constant-factor more than the algorithms  people would otherwise use.
this is mostly an engineering problem, and we'll ignore it for now.

**useful:**\
whoever trained the ai probably had some primary goal, beyond just making a nice ai.
I'll operationalize this by saying that the AI must cause money to end up in openai's bank account.

Note that without this constraint, "just don't build ASI" solves the problem. This is a reasonable political objective for the near future, which I encourage work on, but it's out of scope for this document.

**optimization algorithm**\
this generally means something like gradient descent or policy-improvement (RL).
some kind of iterative procedure.

it feels like it might be important to understand how this training process works. 
I guess SLT is the community that talks about this? 
Maybe we could come up with our own way of talking about training.
Potentially [[online convex optimization notes]] could be relevant.
i think our plan for now is to ignore this until it becomes very obvious that we need to assume something about the training process.

**neural networks**\
A neural network is some gross function.

we can generally abstract this away to being a function $f:\{0,1\}^{n}\to \{0,1\}.$
for instance, an auto-regressive LLM approximates the function $\Pr[x_{n+1}=1\mid x_n, \dots, x_1]$.

there are two settings you can think about, as described in [Paul's blog post](https://ai-alignment.com/low-stakes-alignment-f3c36606937f).
- The low stakes setting
- The high stakes setting

Paul's post says that if we had a good objective function (by which I think he approximately means if we could solve ELK) then SGD-ing this would actually work (namely, we would achieve bounded regret) if the AI needs to take a large number of actions to cause a catastrophe. 

**a good future**
the best attempt I've seen at defining this is [Paul's blog post](https://ordinaryideas.wordpress.com/2012/04/21/indirect-normativity-write-up/) . the gist of the idea is:

The goodness of the future is a value between $[0,1]$ computed as follows:

> Let Alek think about the future for 100 years, with all data and help that he needs. he then gets to assign a score to the future based on how much he approves of it. 

In order to write this out in code, it seems like the main missing ingredient is an upload of Alek?
Maybe that's not necessary if you have some way of pointing to what an upload would be?

one thing that's pretty unfortunate about this is that it feels pretty expensive to get good labeled data for this function.

if we approximate it by asking Alek to write down on a slip of paper whether or not he approves of the robots actions, then there is a predictable failure mode. 

#### 2. a special case

