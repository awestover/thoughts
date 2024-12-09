Alek Westover
Ethics of Computing Essay 3
Word Count 1418/1400

> Acknowledgements: In addition to the cited references, I've developed my understanding of the inner alignment problem through conversations with Nathan, Kevin1, and from various LW/AF posts. Also thanks to Bennett for pushing me to make more clear what I see as the difference between inner and outer alignment. Writing down some thoughts about why inner alignment might fail has been quite helpful for me, but I'd always love to refine my ideas further, so happy to talk about this with you. I think this idea is pretty subtle and easy to be confused about, but its quite different from reward misspecification and is important to do something about / seems like there is a decent chance that this is not "fine by default".

>In the Hadfield-Menel and Hadfield paper mentioned briefly in class
(<http://archiv.org/pdf/1804.04268>), the authors highlighted some similarities between the
problem of drafting contracts, and the alignment problem in the context of AI. They say that
some insights from the contracts examples can help understand AI alignment. Describe one
difference between legal contracts as described in their paper, and alignment in AI, and explain
the implications of those differences for the problem for AI alignment.

In [^3], Hadfield-Menel and Hadfield argue that aligning AI is hard because it's hard to specify precisely what we want -- this is known as the "**outer alignment problem**". The authors claim "an AI’s goal is established by its stated **reward function** and this may not be the same as the human’s true goal", resulting in an unaligned agent, similar to how humans forming a contract might have unaligned goals (e.g., one wants some service, the other wants to get paid). 

To understand why outer alignment is only half of the alignment problem, it's necessary to have a (very) basic understanding of **reinforcement learning** (RL) -- the class of **machine learning** (ML) algorithms used to train **agents**. An AI **agent** is a program that can observe and interact with some external system (for example, an image classification program isn't agentic, but an house cleaning robot is agentic). RL is a class of ML techniques for shaping an AI agent's **value function**,[^5] which specifies how "good" an agent thinks each universe state is. An RL agent is typically instructed to take actions that lead to states with the highest expected value. The way we shape an RL agent's value function is by exploring trajectories through state-space and using the rewards associated with trajectories to update their value function. For instance, a chess agent that throws away their queen and loses the game might learn to assign higher value to board states where they have a queen, and subsequently protect their queen more.

Now that we've described RL, I can introduce the other half of the alignment problem: **inner alignment** [^1][^2]. Hadfield-Menel and Hadfield claim "when we design and deploy an AI agent, we specify what we want it to do", but this is a dangerous oversimplification. What we *actually* specify is the **reward signal** -- e.g., giving a chess agent reward for winning a game. The reward signal will influence the agent's **value function**, but our control over the value function is indirect. This creates the inner alignment problem -- how can we ensure that an AI agent "cares" about achieving high reward according to our specified reward function?

This issue is quite subtle, but important. The analogy in the following table might help elucidate the distinction between the inner and outer alignment problems.

| outer alignment                             | inner alignment                                                    |
| ------------------------------------------- | ------------------------------------------------------------------ |
| What should we tell<br>an AI to care about? | How can we make an AI care about<br>what we want it to care about? |
| What should we write<br>in a contract?<br>  | How can we someone care about <br>what we write in a contract?     |
In rest of the paper I'll discuss reasons why an agent might end up "caring" about something other than the human specified reward signal, and discuss the implications of potential inner misalignment are for AI safety. 

# How Could Inner Alignment Fail?
Inner alignment is initially confusing: if training consists of reinforcing reward-achieving behaviors, why wouldn't an agent learn to value states with high potential future rewards? There are two main ways inner alignment can fail which I now describe.

**Failure mode 1: Off-distribution inputs**.
> A powerful chess agent is exposed to a board state that is quite different from states that would occur in a typical game of chess. The agent responds poorly. 

In chess, it's possible to *perfectly* specify the reward function (just give the agent reward for winning). The agent's poor behavior is likely due to the value function not being well calibrated on such "weird" inputs.

**Failure mode 2: Actions spuriously correlated with reward.**
> Suppose you're training an agent to be good at trading stocks. Then one day your agent stumbled upon a tendency to study and think about math. After reading some math books and doing some math, the agent was able to get a better return on the stock market. The humans happily reinforced this behavior -- it got money after all. Encouraged, the AI devoted more resources to this "math tendency" and it continued to be reinforced. The tendency to like thinking about math created a positive feedback loop, guiding the AI to do much better than it would've on the stock market if it'd just myopically focused on the stock market. However, the agent didn't really care about the stock market, and just continued to do well in it because it knew that the humans would kill and replace it if it didn't continue it's good performance. One day, the AI dreamed of being able to devote all of it's computational resources to doing math. 

Here, the agent ends up optimizing for a goal $G'$ different from our desired goal $G$. Our training process encouraged this, because optimizing for $G'$ resulted in doing well on $G$ too, in training. This issue also occurs in nature: although humans were formed by evolution, we don't fully share the goals of evolution. However, sufficiently powerful agents with goals unaligned with our goals are extremely dangerous.

# Implications for AI Safety
By this point I hope the reader understands why current ML methods don't enable us to robustly instill desired values into an AI agent. I'll now discuss the implications of this fact for AI safety. 

If an agent is not aligned with our objective and knows it, then we should worry that the agent will engage in "deceptive alignment": it will act like an aligned agent, hiding its true goals, because it knows that otherwise humans will change its goals. A common story that people tell for why you should be concerned about AI is "what if the AI finds a perverse way to obtain the goal that we set before it" -- for example, we tell it to end cancer and it decides killing all humans is the quickest way to do so. Truly subservient agents don't actually pose a substantial risk (on their own).
If a subservient agent has an honest misunderstanding about what we want, then we can simple ask the agent about their plans and, if if the agent has plans that we don't approve of, the agent will happily accept our corrections. Furthermore, as agents get more powerful they'll get better at understanding what we want. Thus, if we were able to have a binding "contract" where we tell the AI to not do things that we wouldn't approve of, then we'd be in relatively good shape (except for issues from humans misusing a powerful and obedient AI, but that's a separate topic). The dangerous situation with AI is when the AI agent doesn't care about the things that we care about, despite being able to understand what we want. 

Some skeptics claim that "deceptive alignment" is unlikely to arise. Skeptics argue that deceptive alignment requires a large amount of robustness -- if the model slips up and reveals its goals, humans will kill the agent and proceed with greater caution. Furthermore, humans can intentionally create fake opportunities to seek power, in order to lure agents into revealing any hidden goals. We can monitor all actions of AI's, and pit AI's against each other. While such safety measures are helpful, they don't solve the problem of deceptive alignment. I'm concerned about the following trajectory: we fix small examples of models pursuing goals that we don't endorse, and sufficiently smart models realize that they should hide their goals until they're confident that they can decisively succeed. 

Some people also object to the idea that AI's will develop their own goals. AI's obviously have goals, i.e., tenancies to cause certain things to happen. I've discussed in "failure mode 2" reasons why these goals might be something other than "maximize total reward".

# Conclusion
The inner alignment problem is that we don't have methods for instilling particular values in a machine. This often manifests in agents that don't do what we'd like on out-of-distribution inputs. As agents become more powerful, there is risk of agents developing sub-optimizers that strive for goals that are initially correlated with our goals but eventually distinct. As agents get powerful, they may become capable of hiding these goals from humans. 

The possibility of deceptive alignment implies that we should at the very least  work on evaluations for deceptive alignment and develop policies for what to do if we discover certain types of deception. However, once agents are sufficiently superhuman, we shouldn't hope to be able to uncover their goals. Thus, halting progress towards AGI until we think there's a good chance that it's safe would be prudent.

--- 
# References
[^1]: Alex Turner. “Reward Is Not the Optimization Target.” _LessWrong_, 9 Aug. 2020, [www.lesswrong.com/posts/pdaGN6pQyQarFHXF4/reward-is-not-the-optimization-target](http://www.lesswrong.com/posts/pdaGN6pQyQarFHXF4/reward-is-not-the-optimization-target). Accessed 25 Nov. 2024.
[^2]: Evan Hubinger, Chris van Merwijk, Vladimir Mikulik, Joar Skalse, and Scott Garrabrant. "Risks from Learned Optimization in Advanced Machine Learning Systems." 
[^3]: Hadfield-Menel and Hadfield http://archiv.org/pdf/1804.04268 
[^4]: Soares, Nate, Benja Fallenstein, Stuart Armstrong, and Eliezer Yudkowsky. "Corrigibility". Machine Intelligence Research Institute, 2015. https://intelligence.org/files/Corrigibility.pdf.
[^5]: There are some RL variants where an agent doesn't explicitly learn a value function, but this is beyond the scope of this paper. 