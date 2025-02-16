[Rubi Hudson](https://crossingtherubicon.substack.com/) and some other people have thought about "**predictive AIs**". This seems like a neat idea. I'm going to read some papers about it and write down some thoughts here.

# conditioning predictive models

First a question: by "predictive AI" do I just mean "an AI trained by supervised learning?" I think for now the answer is yes. okay, yup evan says
> Suppose you have a very advanced, powerful large language model (LLM) generated via self-supervised pre-training. 

Is it a "pattern completer" or a "predict the future" kinda guy? Like "predict the next token of this python program" and "predict the value of NVIDIA stocks in 3 months" feel like very different tasks. But I think an AI could get very good at both of these. 

> Note that an important part of the “just trying to predict the future” assumption here is that the predictor model is myopic in the sense that it chooses each individual output to be the best prediction possible rather than e.g. choose early outputs to make future predictions easier.6 As a result, we’ll be imagining that purely predictive models will never take actions like “turn the world into a supercomputer to use for making good predictions” (unless they are predicting an agent that would do that).


Something that evan is imagining the model can do is 
"suppose a researcher started thinking about this q, what would they publish in a year about it?"

or at least "is this a good research direction?"

> comment: predicting is easy if you dont care too much about accuracy. 
> models will get good at predicting
> we wanna figure out *what to predict*


ok here's what evan means by predicting a bad thing:

 > suppose you told your AI to imitate what a malign superintelligent AI would do
 > that'd be supa dumb -- why'd you do that!

The main application that Evan is exploring is trying to get an AI to predict the outcome of some human alignment research.

maybe predicting the future is a bad idea bc it might be weird.

problem -- what if the conditional is unlikely... the AI might realize that its a prompt

we could try adversarial generation

---
spends a lot of time talking about how predicting malign superintelligences is rlly problematic.

---
oh gosh -- now we're doing acausal stuff

the tldr is something like 
"If you condition on something unlikely, then maybe the most likely reason for that input to be in the models training data is if a misaligned AI is simulating the AI."

it seems like the most likely explanation for a weird conditional in your training data is that humans are prompting you. 

not clear what a predictor should even do in that setting

its supposed to "predict itself"

----

Evan's paper has a nice long list of open problems. 

Here are some that I liked: 

> • Are pre-trained LLMs well-modeled as predictive models or agents? – As pre-trained model scale increases, do markers of agentic behavior (appendix A) increase as well?


---

# Questions while reading this

- Can you get extremely superhuman performance out of predictive models?
	- idk what i meant by this. 
	- predicting a human's research output would be op
	- in general prediction is insanely powerful

- Does it make sense to think of predictive models as different than more agentic models?
	- yes
	- agentic models are optimizing for something else

- How evil are predictive AI's?
	- if they're actually just myopically caring about making accurate predictions
	- then they're not inherently evil but its easy to use them in unsafe ways

- How big of a problem is AIs predicting other AIs actions?
	- Evan: really big if they are evil AIs.

- something weird -- the future possibly looks absolutely insane conditional on us getting nice ASIs --- how's the AI supposed to predict that?

- What do we mean by a predictive model: 
	- is there a finite set of options and it puts a pr dist on them?
	- or does it some how pick out some likely outcomes from the future?
	- what's it's loss?



-----

Suppose you want to have a mypoic agent like this:
- Agents have two actions: to press the button or to refrain. If the agent presses the button, they get +1 reward for this episode and -10 reward next episode.

suppose it knows that itll operate for multiple time steps

how to make it press the button?

even if newcomb says not to?

some other weird things
https://www.alignmentforum.org/posts/LCLBnmwdxkkz5fNvH/open-problems-with-myopia

---

### Rubi's blog post
[link](https://www.alignmentforum.org/posts/RHDB3BdnvM233bnhG/the-case-for-predictive-models)

Rubi says:
> Prediction is **super simple** so we probably don't have huge inner alignment problems here.
> I.e., the model probably whole-heartedly adopts this goal before it becomes aware of the fact that it's an AI being trained by SGD.

So, suppose that there is no inner alignment problem for predictive models. 
The question becomes: can you do good things with them?

First of all, you shouldn't directly take actions based on predictions. 
Just an AI predicts an action will result in smiley humans on camera doesn't mean that we'd like whatever world it's predicting.


an interesting claim: 

> "Given a powerful predictor, you can make a powerful agent".

Rubi's argument:

> Loop over actions, predict how good each of these will be, take whichever one seems the best.

This seems not obviously true to me. For instance, what if action space is large and a good action is hard to find -- but easy to evaluate?

Rubi also claimed the reverse:

> A powerful AI agent should have some "predictor" inside of it that is "extractable".

It seems plausible that you could have an agent that has really good heuristics and it doesn't quite have a good picture of how they'll play out but it knows that they're a good idea. 

Even if a powerful AI does in some sense understand what's going on in the world, it's not clear how you can extract that. 


---

# Rubi's paper

We say prediction is performatively optimal if it maximizes $S(p, f(p))$. interested in performatively optimal fixed points.

**results:**
1. If $f(p)$ is lipshitz w really small lipschitz constant, then maybe we're fine
2. in binary outcome case, we can make a scoring rule that incentivizes arbitrarily good performance (without needing really small lipsz constant).
3. in the three outcome case (2) is impossible.

there's some related work where you find a fixed point by asking the agent to iterate on its predictions SGD style. under some assumptions this converges.


----

#### Stuart Armstrong's blog post

maybe condition on humans not seeing output?

