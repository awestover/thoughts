Alek Westover
Word Count: 3212/3000

**Epistemic status**: 
I'm pretty confident in the main point -- that we need some kind of regulation/coordination to relieve pressure and shouldn't rely on alignment to work under extreme pressure/racing -- is accurate. "Pause" is the wrong word for my proposal; what I really meant was more like regulation+coordination, but I'll probably fix that after the semester ends. My writing about the feasibility of regulation / coordination probably comes across as fairly naive/optimistic. But believing that is a self-fulfilling prophecy. Obviously we should have plans for in-case it doesn't pan out -- e.g., desperate alignment efforts -- but it's helpful to think about "what would your picture of things going well look like". I haven't done a great job of describing that here, but the tldr is, 
we have **ONE** (globally) **CAUTIOUS** org with CLEAR REDLINES that works on AI. It doesn't build general super human AI's unless it thinks that the chance of a catastrophe is at most 3%. 

# Introduction
Many prominent Machine Learning (**ML**) experts such as Geoffrey Hinton [^1] (Nobel Prize winner), Sam Altman (CEO of OpenAI) and Dario Amodei (CEO of Anthropic), believe that Artificial Super Intelligence (**ASI**) is likely to be created in the next 2-5 years. There is compelling evidence for this claim: so far, ML model performance predictably increases with compute scale[^2] as a power law, and compute predictably increases exponentially.

There are many potential risks associated with the creation of ASI. It is plausible that an ASI, being more intelligent than humans, would be able to wrest control over the future from humans, resulting in the trajectory of the future being determined by the goals of the ASI. If the ASI has goals which are not aligned with human values, then this would plausibly be catastrophic: it could result in permanently limiting humanity's capacity to grow, and potentially even end humanity, without replacing us with valuable descendants.

In this essay I'll take for granted that (1) ASI is likely to be developed within a decade from now, (2) the creation of ASI has a large chance of resulting in catastrophically bad outcomes. Conditional on these premises, I hope the reader agrees that it is morally imperative to take actions that either decrease the chance that ASI is developed, or increase the chance that the development of ASI goes well for humanity. However, the question of what exactly should be done remains highly controversial, even among communities that accept my premises.

The purpose of this essay is to asses two strategies for handling the AI situation: 
1. **Pause** frontier AI development now, only resume once we're confident we can do it safely.
2. Simultaneously push on developing ASI and doing **safety/alignment** research, so that when ASI is first developed it is less likely to be catastrophic.

For convenience I'll refer to these classes of strategies as "pause" and "align".
I'll analyze these strategies based on two characteristics: 
1. How **feasible** is the strategy? Namely, how hard is it to get people to adopt the strategy, and how hard is it to carry out the strategy? Note that these are two separate problems (one political, the other technical), both of which must be solved to implement a strategy.
2. How **effective** is the strategy? Namely, if the strategy were adopted, how much does it increase the probability of a good future? I'll use totalism to operationalize "goodness of the future".

I'll argue that, while pursuing more alignment/safety research is feasible, it isn't sufficiently effective -- i.e., does not reduce xrisk to an acceptable level. On the other hand, an AI development pause will be feasible (albeit challenging) in the near future, and this is an effective strategy. 

This is an important question to get right because there's a lot at stake. If we paused needlessly, then we'd be missing out on large benefits that could come from safe, aligned AI. If it were the case that trying to pause would inevitably lead to a bad actor developing ASI and creating a horrible future, but that this could be averted by good actors developing ASI first, then it would be imperative not to pause. However, if the result of not pausing is the destruction of humanity, we don't get a second chance. I hope that this essay convinces the reader to advocate for a pause on the training of more capable AI systems using whatever means of communication they deem most appropriate (see pauseai.info/action for suggestions). 

# Alignment
I'll start by discussing the "alignment" [^5] approach to mitigating AI xrisk. At a high level, the idea is: 
> Technical research leads to scientific breakthroughs that enable us to build AI agents that share our values, and robustly act to further these values.

One reason why some people are optimistic about alignment is that we can inspect the "brains" of an AI while it is running and perform experiments such as modifying connections or giving the agent stimuli and observing what happens.

**What would it look like for the US to "pursue the alignment strategy"?**
Currently, the main people working on alignment are engineers in frontier labs and researchers at independent organizations. What I mean by "pursuing alignment" is for the US to spend a vast amount of resources on developing safe AI. Specifically, I'm referring to doing this in the form of a "Manhattan Project"-style nationalized lab, run by some combination of the people currently working on alignment, that pushes forwards capabilities while also working on making AI safe. 

I'll now assess the feasibility and effectiveness of this alignment strategy.
### Feasibility of Alignment:
This strategy (creating a national project and giving it vast resources) is fairly easy to pursue for the several reasons:
1. The notion of a "Manhattan Project for AI" is already in the Overton window; in fact, this was recently suggested to congress[^11], although without any focus on safety -- the pitch was just "we need to beat China to ASI!".
2. As AI's become increasingly capable over the next few years, it will be increasingly clear how transformative ASI will be. People would find advanced AI to be crucial for maintaining economic and military relevance on the world stage. As a result, the government will be willing to invest vast resources into such a project. 
3. People pushing the frontier of AI want their AI's to be aligned, but (generally) don't want to stop working on AI, because they think its a cool and important project. Alignment research offers an alluring route to "have your cake and eat it".
### Effectiveness of Alignment:
I'll now explain why the alignment strategy does not reduce xrisk to acceptable levels. In particular, I'll argue that the US should not spend a vast amount of resources on a nationalized AI project. Instead the US should adopt the "pause" strategy, which I'll discuss later in the paper.

**Solving alignment fast is too risky**
One popular argument in favor of pursuing alignment is that AI will create a massive amount of positive value, and that we will solve alignment as we go. Illya Sutskever, CEO of "Safe Super Intelligence", expresses this mindset as follows:
> We approach safety and capabilities in tandem, as technical problems to be solved through revolutionary engineering and scientific breakthroughs. We plan to advance capabilities as fast as possible while making sure our safety always remains ahead.[^4]

The plan to "figure it out as we go, while going really fast" has low probability of success. Breakthroughs in science are not predictable and we shouldn't count on them happening when it'd be most convenient for us. Also, there are several reasons why "going as fast as possible" is especially dangerous for AI:
1.  An unaligned agent will be incentivized to hide its true goals from us until it is in a position to act on them. Current techniques for finding out if a model can do something dangerous require us to be able to elicit the dangerous behavior. If we are moving as fast as possible, it will be extremely tempting to cut corners on safety.
2. In ML we define an objective and try to maximize it. It's usually very difficult to formalize the objective that we actually care about, so we generally settle for proxy metrics -- e.g., "number of hours spent on our platform" is easier to optimize for than "benefit provided to user". However, Goodhart's law mandates that optimizing for a metric which is not our ultimate goal eventually leads to poor performance on our actual goal. Proceeding at breakneck speed makes us even more reliant on proxy-measurements. 
3. It's easy to trick ourselves into thinking that we've made alignment progress, when we're really not. For instance, "RLHF" is sometimes touted as an alignment advance, but it actually will lead to more dangerous models. 

The potential upsides of AI need to be weighed against the potential downsides, and the math doesn't work out in favor of the "go really fast" strategy.

**Race dynamics are dangerous**
Another popular argument for pursuing the alignment strategy is:
1. If we don't develop ASI, then someone else will (soon!).
2. We're the good guys, so it'd be better for us to develop ASI first than for anyone else to.
Interestingly, this is the genesis of both OpenAI and Anthropic: According to emails released during Musk's lawsuit of OpenAI[^9], OpenAI started because Altman was worried about Google developing ASI. More recently, there has been a mass exodus of OpenAI employees to Anthropic, which has branded itself as the "safety focused" frontier AI lab. Proponents of a nationalized AI project point to China, and assert that we must get to ASI before China[^10]. This concern has some merit. If a bad actor was able to align an ASI, they would have a decisive military advantage, and the rest of the world would be at their mercy. If a good actor were instead to first develop ASI, the future would be more likely to be good.

However, this possibility does not mean that pursuing alignment leads to the best outcome (as measured by totalism), for reasons that I explain below.
1. When well-intentioned people decide to work on ASI with the purpose of getting there safer and before other people, they will be subject to lots of pressure to push harder on capabilities, and cut corners on safety. The pressure comes from investors that want them to make money, and from the prestige that people feel when their company succeeds, and possibly even a desire for power. These strong psychological factors may be at play in causing CEO's of frontier labs (like Amodei and Altman) to push on capabilities, despite acknowledging the risks associated with AI.
2. Exacerbating this issue, it is quite hard to stop without a clear demonstration of danger -- people will accuse you of being paranoid and point to examples of times in the past when false-alarms were raised. For this reason we should expect an AI takeover to be a single event where AI's decisively overpower humans. 
3. Additionally, being tricked into thinking that you have a solution becomes more likely as you race, because this would be convenient and thus feels nice to believe.
4. Framing AI progress as a race misses the fact that misuse danger is only part of the danger from AI. If anyone builds a powerful AI that pursues its own arbitrary goals then all humans lose, regardless of who built the AI. As discussed earlier, going fast makes it less likely that you will have precise control over your ASI.

In summary, the alignment strategy is not very likely to create a valuable future. In the next section I will give an alternative to the alignment proposal: the US pauses progress on frontier AI's, and coordinates to achieve a global pause. I'll argue that this strategy *is both feasible and effective*, if implemented properly.

# Pausing
I'll now consider the "pause" strategy. There are a large number of different proposals about exactly what regulation should be put in place; this is somewhat unfortunate because it divides the already small group arguing for such regulations, and because some of the proposed regulations are more feasible and effective than others. Some people want companies to self-regulate, possibly through "RSPs"[^12]. The recently vetoed SB 1047 bill would have required companies in California to release a document explaining why they thought that their training runs didn't pose a risk to the public [^6]. The PauseAI organization [^8] proposes forming a treaty that would, among other things, "only allow training of general AI systems more powerful than GPT-4 if their safety can be guaranteed." Eliezer Yudkowsky has proposed creating a global coalition where all participating countries agree to not train any models larger than currently existing models. To make sure that this is enforced Yudkowsky proposes: "Make it explicit in international diplomacy that preventing AI extinction scenarios is considered a priority above preventing a full nuclear exchange, and that allied nuclear countries are willing to run some risk of nuclear exchange if that’s what it takes to reduce the risk of large AI training runs." I'll focus on analyzing the strategies of the form "have a global agreement to stop AI development, with a plan for enforcing the agreement".

### Feasibility of Pause
Many people are highly pessimistic about the feasibility of sufficiently strong AI regulation. I'll now state and counter concerns of pause-skeptics. 

**The risk is nebulous**
Pause-skeptics worry that AI xrisk is a very technical problem making it hard for policy-makers to be well-informed, especially in the face of comforting messages from corporations telling them not to worry. Furthermore, pause-skeptics worry that it's very difficult to get preemptive regulation, and "responsive regulation" won't work, because a smart AI would wait until it has a decisive advantage before revolting, thereby giving humans no chance to respond. These problems genuinely make pausing difficult, but not insoluble:
1. People are already somewhat worried about AI. Chat-bots have made concerns about AI easier for people to understand. It also has helped that some AI experts have been vocal about the risks that future AI systems pose. 
2. There are lots of other instances of the government putting out preemptive regulation, especially when there are safety concerns. For instance, the FDA has extremely stringent requirements for drug testing. As AI's grow in capabilities, it'll become increasingly obvious that regulation is required. 
On its own (1) and (2) might seem like impetus to change corporate ASI projects into government defense projects, which I've argued earlier is still extremely dangerous. However, there are a lot of good and intelligent people working on AI, who either understand the risks from ASI or could be convinced of them. Individually many such actors would act responsibly. Thus, regulation that eliminates race-dynamics between companies in the US could open a pathway towards global coordinated regulation.

Above, I've discussed the feasibility of pausing from a socio-political perspective, and argued that it is possible. However, many pause-skeptics argue that even if people could agree that they'd like to regulate AI, it is technologically infeasible to regulate AI, due to the technology being too widespread. I'll now address the technical feasibility of pausing.

**Technology is too widespread**
There is some merit to this concern -- many of the algorithmic ideas necessary for making powerful AI systems are published, and if it were the case that anyone could train a powerful AI model on their laptop, we'd be in really big trouble. However, for the moment AI is quantitatively different from this: using current ML techniques, making ASI would be **extremely resource intensive**. Large training runs require a huge quantity of expensive GPUs (e.g., one H100 chip costs ~$25,000, and GPT-4 required approximately 25,000 such GPUs for 100 days), and consume vast amounts of power and water (for cooling). To disrupt frontier AI progress, the government could collect and destroy all GPUs, and then make GPUs illegal -- anyone caught with one will go to jail. 
### Effectiveness of Pause
I'll now argue that the pause strategy substantially increases the probability of a good future. I'll argue by countering several arguments for why pausing is ineffective.

**Pausing is a temporary solution**
A pause-skeptic might argue that pausing is only a stopgap, buying us maybe 30 years before algorithmic advances make it so that any actor with a moderate amount of compute can build ASI, at which point we are in an even worse position than we are now, with a small number of known actors. From a totalist perspective, a slight decrease in the probability of a good long future is not worth a substantial increase in the probability of 30 good years right now. This argument misunderstands what a *pause* is. In pausing we aren't going to pretend that the problem is solved. Pausing is instead a desperate measure to buy us time for a few years of intense political work, after which we can arrive at a better long term solution. By premise of this paper humanity currently faces imminent destruction. Assuming this, a pause would give us a better chance to prepare. 

**Pausing leads to discontinuous capability jumps**
Some people objecting to a pause worry that a pause will result in a "**compute overhang**": instead of steady progress, progress will stall during the pause and then after the pause we will have a massively discontinuous jump in AI capabilities. I completely agree with such people -- a temporary pause on capability development without pausing on hardware development has net-negative expected value. An effective pause must not result in a compute overhang. 

**Pausing forgoes AI upsides**
AI can do a lot of good, e.g., in healthcare. My proposed pause would unfortunately interrupt this. However, by premise the risk out-weighs such benefits. Such people might ask for exceptions to the regulation to be made for training non-general AI models. While such exceptions wouldn't necessarily completely undermine a pause, they should not be brooked because they make the pause too difficult to implement. 

**Pausing disrupts alignment research**
Banning compute would also incapacitate empirical alignment researchers. Thus, a pause does not give us more time to solve alignment. \\
This is a serious concern, however the point of a pause is not to buy time for alignment research. Instead it is to buy time for global coordination and regulation, which can happen in the absence of GPUs. As discussed in a previous section, AI alignment / safety work during an AI race likely leads to catastrophe. 

**Market competition forces companies to align their ASI, pausing removes such pressures**
Some people worry that if we implement pause, some bad actor will defy the agreement and secretly build ASI. However, for reasons discussed earlier, it is very unlikely that someone could build ASI "in secret", e.g., due to massive compute requirements. If some party declined to accept the world's agreement to pause, the pause coalition would need to be extremely transparent that such actions will be dealt with severely. 

## Conclusion
I've discussed two proposals for mitigating xrisk posed by AI: alignment and pausing.

I've argued that the risks posed by ASI are too large to justify pushing forwards with AI development in the absence of a good plan for making this go well. I've argued that until we can rule out the specific and plausible concerns that people have about ASI, it's just better not to proceed. I've argued that, while pausing seems radical, there are precedents for it, and it's not actually too far out of the Overton window. I've argued that, even if this is only a temporary measure, and even if it deprives us of benefits from AI and hampers alignment research, a pause is still good because the risks of not pausing are incredibly large, and a pause decreases these risks. 

The risks posed by current trends in deep learning are real, and large, but not insurmountable. I hope that this essay makes it clearer to the reader why alignment work alone is an unsatisfactory strategy, and why it is paramount to push for a coordinated regulatory effort. I hope that the concerned reader decides to do something about this, and increase the expected value of the future.

----
### References
[^1]: "Godfather of Artificial Intelligence" Geoffrey Hinton on the promise and risks of advanced AI.
[^2]: Kaplan, Jared, Sam McCandlish, Tom Henighan, Tom B. Brown, Benjamin Chess, Rewon Child, Scott Gray, Alec Radford, Jeffrey Wu, and Dario Amodei. "Scaling Laws for Neural Language Models." _arXiv_, 2020, arXiv:2001.08361.
[^3]: Yudkowsky, Eliezer. "Pivotal Act." _Arbital_, [https://arbital.com/p/pivotal/](https://arbital.com/p/pivotal/). Accessed 26 Oct. 2024.
[^4]: Safe Superintelligence Inc. "Safe Superintelligence Inc." _ssi.inc_, 19 June 2024, [https://ssi.inc/](https://ssi.inc/). Accessed 27 Oct. 2024.
[^5]: "Introduction to Alignment." _AI Safety Fundamentals_, BlueDot Impact, 22 Oct. 2024, [https://aisafetyfundamentals.com/blog/alignment-introduction/](https://aisafetyfundamentals.com/blog/alignment-introduction/).
[^6]: California State Legislature. _SB-1047 Safe and Secure Innovation for Frontier Artificial Intelligence Models Act_. 2024, leginfo.legislature.ca.gov/faces/billNavClient.xhtml?bill_id=202320240SB1047. Accessed 26 Oct. 2024.
[^7]: Yudkowsky, Eliezer. "Pausing AI developments isn’t enough. We need to shut it all down." Time magazine 29 (2023).
[^8]: "PauseAI Proposal." _PauseAI_, 9 Oct. 2024, [https://pauseai.info/proposal](https://pauseai.info/proposal). Accesse 26 Oct. 2024.
[^9]: Elon Musk emails to OpenAI https://www.techemails.com/p/elon-musk-and-openai
[^10]: Leopold Aschenbrenner. _Situational Awareness: The Decade Ahead_. June 2024. Available at: [https://situational-awareness.ai/](https://situational-awareness.ai/). 
[^11]: U.S.-China Economic and Security Review Commission. _2024 Report to Congress_. Washington, D.C., November 2024. Available at: [uscc.gov](https://www.uscc.gov/sites/default/files/2024-11/2024_Executive_Summary.pdf)
[^12]: Anthropic's RSP https://www.anthropic.com/news/anthropics-responsible-scaling-policy
