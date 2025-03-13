# ASI is possible soon, if people try hard

Before arguing for lemma 1, we need to define ASI. 

I'll define a ASI to be an AI with a critical score on the [openai preparedness framework](https://cdn.openai.com/openai-preparedness-framework-beta.pdf). 
Namely: an AI with superhuman abilities at hacking, autonomy, persuasion, CBRN.

More specifically I will require that the AI can do most of these things:
* Can do biology research — e.g., figure out how to synthesize novel proteins with a desired function
* Can do strategy — e.g., military strategy, or running a company
* Can “survive on its own in the wild” e.g., make enough money online to purchase GPUs that it can run inference on
* Can hack well enough to promote it’s privileges to root / to escape the lab somehow (or it can do this via social engineering)

I'll now explain why, if AI corporations don't drastically alter their trajectory, I expect them to succeed in making ASI in the next couple years. You might think "AI corporations don't drastically alter their trajectories" is a pretty unreasonable assumption --- if they feel like they are close to making something dangerous, wouldn't they get spooked and stop? I explain why some an AI slowdown is unlikely in [[Lemma 4]]. In this section, I'll assume that people are trying as hard as they are right now to make ASI, and see what the world looks like if this continues.

**Argument 1 for Lemma 1**:

> There are no major bottlenecks to scaling up systems to become more powerful.
> And, if there were bottlenecks, there is an abundance of talent and money being thrown at the problem: people will find a way around the bottleneck.

There are four major ingredients that go into making powerful models, which could potentially bottleneck progress:

> Energy, data, GPU chips, and money.

Some people have [crunched some numbers on this](https://www.lesswrong.com/posts/SoWperLCkunB9ijGq/can-ai-scaling-continue-through-2030-epoch-ai-yes), and [we still have substantial room to grow in these four areas](https://www.lesswrong.com/posts/WZXqNYbJhtidjRXSi/what-will-gpt-2030-look-like). Also there's a trend of cost decreasing rapidly over time. E.g., o3-mini is comparable or better than o1 at much reduced cost. If there is going to be some big bottleneck, (e.g., energy), I expect labs to be proactive about it (e.g., build nuclear power plants).

AI companies are already being proactive about building the infra they need to scale up, see project stargate.

**Argument 2 for Lemma 1**:

> Recent progress has been rapid; it can continue.

- GPT3 -- high schooler
- GPT4 -- college student
- o3 -- Comparable with CS/Math Olympiad competitors on close ended ~6 hour long tests. Quite good at SWE.

Of course, now that these systems exist, people are finding lots of reasons to claim that they aren't impressive. We humans grow accustomed to things remarkably quickly. In order to notice that you are surprised, you must make predictions and have them be falsified. 

A misconception that I find baffling is that AI capabilities will stop at human level. People say a lot of words about training data, and seem to neglect the fact that there are known methods for AI's bootstrapping to superhuman performance on tasks (namely, RL). For instance, if you want to imagine how you could get an AI that is better than humans at math, here's how you could do it:
- Start by training it to solve some easy math problems, maybe by imitating humans.
- Once the AI is good at that, slowly increase the difficulty, and let the AI get really good at the new difficulty level.
- The AI does not need humans to score it's performance -- it can just check the answers. This provides a nice signal for RL.

Some people argue that ASI is impossible because "computers can't be intelligent", or "humans are special, so we'll always be able to do some things better than AI's. I find Tegmark's analogy helpful here: "the universe doesn't check if the heat-seeking missile is sentient before having it explode and kill you". It seems pretty clear that intelligence is just a bunch of hacks. Nature was able to evolve humans to be smart, why could RL not do the same for AIs?

**Argument 3 for lemma 1**

It seems likely, but not essential for the argument, that AI's can speed up AI research a lot soon. In some sense, they are already doing this --- e.g., if you use an AI code completion tool or ask an AI to make a plot for you that's speeding stuff up. A lot of R&D is "grunt work" / implementing things (as far as I know). I think the amount by which AI's could speed up research is substantial. Here's a framework for how this could happen:

- For instance, soon I believe that you’ll be able to tell an AI to go do some experiment and it can just go to it. 
- Not long after this, I think you can ask the AI “what would be good experiments to do” and it’ll give good suggestions. 
- At this point, you don’t need as many humans in the loop proportional to the amount of research that goes on — humans can give high level objectives to the AI and the AI can make and execute a research program to achieve those goals.  

## Appeal to authority on Lemma 1

I find it fairly compelling that there are many AI experts -- both academics and people in industry -- think that believe AI progress will be fast. Here are some quotes.

> Yoshua Bengio: “Because the situation we’re in now is that most of the experts in the field think that sometime, within probably the next 20 years, we’re going to develop AIs that are smarter than people. And that’s a very scary thought.”

> Sam Altman "It is possible that we will have superintelligence in a few thousand days (!); it may take longer, but I’m confident we’ll get there." 

> Sam: "We are now confident we know how to build AGI as we have traditionally understood it. We believe that, in 2025, we may see the first AI agents “join the workforce” and materially change the output of companies."

> Elon Musk: said that AGI that is “smarter than the smartest human” will be available in 2025 or by 2026 in an interview with the Norwegian head fund manager Nicolai Tanger last year.

> Dario Amodei: 
> "It depends on the thresholds. In terms of someone looks at the model and even if you talk to it for an hour or so, it's basically like a generally well educated human, that could be not very far away at all. I think that could happen in two or three years.
> The main thing that would stop it would be if we hit certain safety thresholds and stuff like that. So if a company or the industry decides to slow down or we're able to get the government to institute restrictions that moderate the rate of progress for safety reasons, that would be the main reason it wouldn't happen. But if you just look at the logistical and economic ability to scale, we're not very far at all from that."

> Nvidia CEO Jensen Huang:
> "AI will be ‘fairly competitive’ with humans in 5 years"

[Survey](https://aiimpacts.org/wp-content/uploads/2023/04/Thousands_of_AI_authors_on_the_future_of_AI.pdf)

**Bob Objection:**
- People in industry have COI for claiming powerful AI soon -- this could get people excited about AI, and drive up investment.
- People in AI safety have COI for claiming powerful AI soon --- this could get them more funding.

**Alice:**

I don't think the expert opinions are conclusive on their own, just another piece of evidence that makes the situation look bad. 

Some reasons why I'm inclined to take the data more seriously than you are:
- If timelines aren’t as short as people say, it’ll hurt their reputation.
- People like Geoff Hinton are willing to do hard things like quitting his job and saying that he regrets his life's work in order to talk about this. 
