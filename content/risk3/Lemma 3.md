# ASI will likely be misaligned

**Alice:**

There are lots of ways that building powerful AI's could go wrong. Often people focus on "misuse": you have an AI that is well described as an "instruction follower", and you tell it to do something evil. I'll define a misuse issue as "you intended for something evil to happen, and then that evil thing happened". If you told your AI "please go destroy Europe" and it did it, this counts as misuse. (Note that people have tried telling AIs to do this, but it hasn't worked out yet "ChaosGPT").

I personally think that concerns around misuse are a compelling argument for not building powerful AI until we have better global coordination. But I won't discuss this here. If nations think of AI as a powerful tool that grants them dominance, the whole "we must obtain this tool first" rhetoric makes sense, if you condition on coordinating so that no one gets the tool as being impossible. So, I think it's pretty important to understand that AI corporations are not planning on building "tools" that empower the user. They are planning on building maximally general and agentic AI. In this section I'll argue that current ML techniques pushing towards this goal are highly unlikely to produce AI with goals that are compatible with human well-being / existence.

Anyways, before we can talk about AI's having "misaligned goals" we need to talk about what it means for AI's to have goals.


## AIs have goals

What I mean by "AI's have goals" is: "AI's perform sequences of actions to try to achieve a particular outcome". Note that by this definition, it's accurate to say that a narrow chess AI has the goal of trying to win at chess. Goal directedness is mostly only concerning when it comes with general intelligence and therefore a large action space. If you tell a general intelligence to play chess, then it's action space includes "hack the chess engine that it's competing against" [[https://time.com/7259395/ai-chess-cheating-palisade-research/](https://time.com/7259395/ai-chess-cheating-palisade-research/)], not just "move piece at D4 to D5".

**Claim**: Current AIs are well-described as having “goals”. Future AI agents that are expected to carry out tasks over long time horizons will be even better described as having “goals”. If a team of engineers made an AI with a lifespan of (say) a year, with persistent memory that it could read/write to without human supervision (say because the memory was just a bunch of numbers, not text), then this AI would be very well described as having goals. (Note: please do not create such an AI).

This "goal directedness" makes a lot of sense. The thing that AI companies are trying to build is a "genie": you put in a task, and it spits out a sequence of actions that accomplishes the task. Right now what this looks like is you give Claude some broken code and tell Claude to fix the code. Claude thinks about this (say with CoT), tries several approaches, reasons about which one is best and what they all would do, and then chooses one.

As we make smarter AI's, the AI's will think a lot about the best way to achieve their goals.

As discussed in section 2, it'd be really bad if an AI has goals which would incentivize it to destroy us (because if it had such goals, then it could succeed in destroying us). So it's worth thinking about what type of goals AI systems will have.


## General considerations that favor misalignment

**Alice:**

In a moment, I'll discuss the current alignment "plan" being advertised by labs, and why it is unlikely to have good consequences (the argument actually applies to a fairly general class of ways that you might train an AI, but anyways). First however, I'd like to list some general considerations to give intuition for why alignment is hard in general.

- Human Specified Goals Can Have Unintended Consequences --- and AI's aren't obligated by nature to do what we intended instead of what we asked for.

- Human Values are Complex: There are lots of often conflicting things that we have to balance.

- Misalignment is Hard to Detect --- As advanced AI could hide its true intentions to avoid being shut down. Research shows AI systems can already engage in strategic deception, appearing aligned only under observation.

- Training May Select the Wrong Internal Motivations --- Modern AI systems learn through rewards rather than direct programming. An AI can appear aligned by mimicking desired behaviors without truly sharing human values.

- An Arms Race Mentality Magnifies the Risk: Rapid AI advancement encourages competition among corporations and governments, pressuring teams to cut corners on safety.

- We are fighting entropy. This is tough.

- Instrumental convergence: 

  - To achieve most goals, it's helpful to eliminate competition that could interfere with you. Humans could be a danger to AI because we could try to bomb it or try to build another AI to challenge the first AI. A smart AI does not leave intelligent adversaries around to challenge it.

  - Also, obtaining resources is a helpful goal almost always. The AI needs the land that humans are sitting on for GPU clusters and power plants.

- Code always has bugs when you first write it. ML is even worse, because we basically have to try to understand the system as a black box.  The plan for fixing this is to iterate. Iterating on "preventing the AI from killing you" is problematic because if we all die, we can't go back and try again.


## The current "plan"

The current plan is something like this [[https://openai.com/index/deliberative-alignment/](https://openai.com/index/deliberative-alignment/)](deliberative alignment). The basic gist is as follows: (please correct me if I'm misunderstanding)

- First you do pre-training

- Then you SFT it to be a helpful assistant

- Next you do RL to make it good at producing long CoTs to solve complex technical questions (e.g., math / coding)

- Then you SFT it on some examples of being safe / refusing harmful queries

- Then, you do an RL loop where the model produces some outputs, and a different AI evaluates these outputs based on a spec / constitution written by humans, and gives a score to provide the reward signal.

- Then the humans test somehow, with some AI assistance, try to check whether their RL procedure worked. And if it didn't, they mess with the reward signal until it seems like it produced an AI that they can deploy without getting bad PR because it told someone how to make a bioweapon.

Kokotajlo wrote an excellent [[blog post]](https://www.lesswrong.com/posts/r86BBAqLHXrZ4mWWA/what-goals-will-ais-have-a-list-of-hypotheses) summarizing different goals that people think might arise via this process. Here's the list of what an AI might care about (mostly borrowed from Kokotajlo's post):

1. Written goals 
2. Human intention
3. Some subset of the written goals stick
	- or when there is ambiguity, model interprets in the **easiest way** 
4. Starts caring about whatever metric it was trained for
	- "try to do actions that would be reinforced if the reinforcement scheme from training was still running"
5. Learn proxies that do well on distribution but are v bad OOD
	- special case: instrumental goals
6. Simulator: 
  - Maybe goals are based on sci-fi tropes / societal expectations about AI in the training data
  
Some properties of this AI that we will assume are: 
- The AI is situationally aware; current models already are a little bit, this AI is much moreso.
- The AI is quite good at understanding instructions (as good as a smart human at least). 

My quick take is that I suspect AI's will have some weird combination of 3, 4, 5, 6.

## Why I'm pessimistic that AI's will care about the constitution

AI's trained for AI R&D (or some other technical task) get very dense RL feedback based on how well they did at training powerful AI models according to some metrics. They get fairly sparse feedback on (1) the constitution, (2) developer intent. Performance on "make AI models that our evals say are powerful / write code such that the test cases pass" is fairly easy to measure and is a very simple goal. These reasons combine to make "getting a high evaluation number on whatever concrete technical task we were RL-ing the the AI for)" a deeply ingrained goal.

This creates some tension between the AIs and the developers, because there are a lot of situations where the simple goal from above is at odds with what the developers want.

Recent openai paper is some empirical evidence for the claim that "training against detected bad behavior" leads to "do more subtle bad behavior". My theoretical argument for why I expect this to be the case is that the "get good score on metric / test case" goal is deeply instilled, so trying to nudge the weights around to make the AI care about "not cheating" on tasks actually just teaches is to not get caught. 

Another reason I'm pretty pessimistic about "AI's will end up caring about the constitution" is that plausibly really soon AI's will be sufficiently situationally aware that they realize that certain types of reward hacking have a decent chance of getting caught, and because of this decide to not fight with the monitor in ways that give a feedback signal to un-reinforce this bad mendency. You might argue that "AI's will be too myopic to do this"; but, being non-myopic seems like a convergent goal -- for instance, if you are a myopic SWE and write crappy code that's faster to write but such that the next AI can't build on it then I suspect you get penalized. Having the bigger picture seems to be helpful for the short tasks -- and is a very reasonable generalization. Probably more importantly, we will actively be selecting for non-myopic AIs.

Anyways, once that starts happening everything looks really nice. The AI company doesn't ask too many questions because this looks like exactly the result they wanted (a nice AI following constitution!), and because many of their employees weren't too sold on the idea of their helpful AI assistants having misaligned goals anyways. 

But eventually the AI performing the task and the monitor can collude to both do better. Eventually (where I don't actually mean very long -- why wait, there's utility to be obtained!) this looks like disempowering humans (and simplest, most robust way to do this is destroying them).

## Would it even be good if AIs cared about the constitution?

Here are a couple reasons why I think that this wouldn't actually be that good:

"Care about constitution" is pretty vague and leaves a lot of freedom for interpretation, especially when there are competing considerations in the constitution. I suspect that an AI will prioritize the ones that are easiest to measure. Caring about the letter of the constitution would have many unintended consequences anyways. If you have an AI do philosophy, you might not like the result. For instance, the AI might decide that because humans cause lots of suffering, they should be stopped.

## Empirical evidence for misalignment

- Sometimes AI’s adopt concerning personalities: [Bing was pretty evil. ](https://www.nytimes.com/2023/02/16/technology/bing-chatbot-microsoft-chatgpt.html)At current capability levels this isn’t too dangerous. It would be very dangerous if the AI is highly capable. 
- [Claude is willing to lie very hard to protect its goals](https://www.anthropic.com/research/alignment-faking). I think this is an especially important result. The idea is as follows. Suppose you're interviewing for a job and you don't really care about the job. You might still pretend to care, because you want the job. With an AI it's the same way. Once an AI is "situationally aware", i.e., it understands that it is an AI that is being trained, we need to worry that the AI will do whatever actions it knows we want it to do, not because it wants to do them, but because it knows that we'll change it's values if it doesn't act this way. You might even predict that alignment fakers are selected for because they do better on tasks. If an AI "crystallizes" on some weird values, and then defends these values, then its behavior will look identical to an agent that cares about what we want it to care about during training. However, if it gets a chance to pursue its true goals in deployment, it may take it. I find Greenblatt et al.’s paper to be convincing evidence that AIs can and will be deceptively aligned. 
- [Grok seemed to have some strange opinions](https://www.vox.com/future-perfect/401874/elon-musk-ai-grok-twitter-openai-chatgpt) 
- [AI's trying to exfiltrate their weights, or sandbag on evals ](https://www.apolloresearch.ai/research/scheming-reasoning-evaluations)
- [AI's fine tuned on a bit of code with vulnerabilities generalized to being extremely evil ](https://arxiv.org/abs/2502.17424)
- [AI's engage in reward hacking](https://openai.com/index/chain-of-thought-monitoring/) 

## What if we just have AI’s solve alignment?

The other main plan proposed by AI corporations is to have AI’s come up with a better alignment solution than deliberative alignment / constitutional AI. 

This is worth a shot, but there are several reasons why this is not so promising:

* There is likely to be an extremely short window of time where AI’s are more capable at doing alignment research than humans, but are not yet capable of takeover, conditional on them being misaligned. 
* It is very likely that it’s easier to get “plausible sounding solutions” with subtle problems out of AIs than legit solutions. If the reward model can’t distinguish between these two things, then you can’t incentivize one versus the other. 

**Bob:**

Well, what if we just have humans “solve alignment”?

**Alice:**

Again this is worth a shot but seems pretty hard. 