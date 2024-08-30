In this post I aim to give fairly concise arguments for the following two claims.

**Claim 1** There is a larger than 75\% chance of highly capable AI being created by 2040.
**Claim 2** There is a larger than 75% chance of all humans being dead within 2 years of the development of highly capable AI.

First some notes: 
- There are reasons why you will want to dismiss these claims without serious consideration. 
- Here is a list of rationalizations that I engaged in, in an attempt to refute claims 1 + 2.
	1. These feel like a pretty big deal. Someone *else* would have noticed and done something about this if it were really posed to be a problem. 
	2. It's not my responsibility. 
	3. The lack of substantial restrictions from governments, lack of concern among companies pursuing intelligence, and lack of concern among the general population indicate that this isn't really a problem. 
	4. Maybe highly capable AI isn't achievable. 
	5. Maybe highly capable AI will be nice by default. 
	6. This can't be a real concern. Surely people would stop before things actually got dangerous.
	7. Humans are pretty resilient. We could handle it. 
	8. Hmm, this seems uncomfortable, i'd rather not think about it.
- These rationalizations basically boil down to: (1) bystander effect / refusal to take responsibility for something, (2) motivated reasoning, (3) conformity pressures. 
- I find these rationalizations un-enticing.

In any case, now I proceed with arguments for claims 1 and 2.

**Claim 1**
Seems almost obvious for a couple of reasons. 
- ChatGPT4 etc are already super-human at many tasks. 
- It doesn't seem like we've plateaued in any way: Scaling up model sizes is likely to just make them better.
- [some report from the EU](https://www.europarl.europa.eu/RegData/etudes/ATAG/2024/760392/EPRS_ATA(2024)760392_EN.pdf) indicates that investment in AI will probably be 10x larger by 2030. 
- It seems like there is constantly a large amount of AI hype which drives money and excitement into the field.
There are two main objections made to claim 1 that I have heard. 
1. "Yes, computers are good at some things, but humans will always be better at some things." Relatedly "Just look at this example of ChatGPT doing something stupid." "ChatGPT isn't actually *that* smart, it's just a parrot."
2. Once AI starts becoming dangerous people will start noticing and do something about it.

Here's why I think (1) is wrong:
The data just doesn't support this. So far computers can beat humans at lots (most?) strategic games (e.g., chess), and are outperforming humans on lots of standardized tests. There is a clear trend of computers getting better at this over time (i.e., as you throw more compute at the problem). It feels nice to think that humans are special, and I think humans *are* special. In particular I much prefer humans to paperclip optimizers. But I don't think we're *special* in the relevant sense of being invincible or being really good optimizers. 
I think (2) is also wrong.
- First off, the coordination required for this is really large. You don't just need OpenAI, google, meta, and anthropic to stop. You need everyone to stop. Facts like "llama is open source" and "compute is ubiquitous" and "compute required to train something powerful decreases over time" make this really hard. 
- Just empirically, (e.g., covid-19) coordination on this scale is unlikely.
- The incentives are unfortunate: it feels like an arms race, and kind of is. Maybe US is too scared to stop because then some other country would become more powerful. 
- Exacerbating the "people will do something about it" issue, is that as we become reliant on powerful AI systems, it's harder to do something about it. For instance, imagine a policy of "everyone needs to get rid of their phones and computers". This is just unthinkable to most people at this point. 
- In a similar vein, AI systems will have lots of positive effects (before annihilating humans), which makes it harder to shut it off. For instance, improved healthcare. 
Another reason to worry about (2) is the possibility (and maybe likelihood) that there is no "warning shot". That humans don't even get a chance to react because the AI seems safe until the moment it annihilates humans. I generally find that this "coming up from second" is highly effective in strategic card/board games. This called "the treacherous turn".

Before discussing point two, we need some preliminary lemmas. 

**Lemma 1**: The ["Orthogonality Thesis"](https://www.alignmentforum.org/tag/orthogonality-thesis)  (due to [Nick Bostrom](https://nickbostrom.com/) or maybe Eliezer Yudkowsky)\
Basically any level of intelligence can be paired with basically any objective function.
In other words, being super intelligent is orthogonal from being super moral.

**Argument**:
"Intelligence level" probably is more accurately described as "level of capability" or "level of skill at getting what you want (normalized by how hard it is to get what you want)". 

An agent is just some entity that has a space of possible actions, and a utility function over possible world states. The ability to evaluate larger action spaces, and make more powerful causal inferences 

**Lemma 2** "Instrumental convergence" (also Nick or Eliezer)
Activities like *power seeking* and *resource acquisition* make sense across a broad range of possible utility functions.
**Argument**
power and resources help you achieve your objectives. 
this seems pretty clear: to the extent to which you are not in control there is risk that something external causes you to be unable to achieve your goal.

**Lemma 3**
AI will have an extremely large attack surface area (this is probably not requisite for AI to be harmful, but makes it more obvious how it could be harmful).
**Examples**
Emailing people, doing something on social media, doing something with money, writing arbitrary code for like any purpose. These are things that current AI systems are already capable of doing, and as they progress people will continually happily offload more and more to the machines. 
My primary concern here is not even "is mechanistic interpretability possible" (although this seems like a rather challenging question). The question is more like: if chatgpt could come up with some complicated scheme to make you a bunch of money would you just copy-paste it into a python script without reading it and run it? / would you authorize it to do so?
To which the answer I feel is "yeah, seems pretty likely".
The experimental mindset of "let's just test things and see if they work and if it breaks then we'll fix it" is exactly what's so dangerous. 

----

OK, well, what to do about it?
One simple (but still pretty hard because it requires researching the issue to form an opinion yourself, and then putting your reputation on the line for believing something somewhat radical) thing you can do is spread awareness of the problem.
I'm pretty sure this is a net positive? The potential downside is that it could cause people to experience negative emotions. The potential upside is that you can better optimize when you have more information about the constraints you're working under. 
This is discussed a bit more here: [[5 years]].


---
#### further reading

- [Richard Ngo on Alignment difficulties](https://arxiv.org/pdf/2209.00626)
- [Here are some more resources on ML / alignment](https://course.aisafetyfundamentals.com/alignment)
- [Some of Paul Christiano's thoughts on alignment difficulty](https://www.lesswrong.com/posts/CoZhXrhpQxpy9xw9y/where-i-agree-and-disagree-with-eliezer)
- [Deepmind thoughts on difficulty](https://www.lesswrong.com/posts/qJgz2YapqpFEDTLKn/deepmind-alignment-team-opinions-on-agi-ruin-arguments)
- [Eliezer views on difficulty](https://www.lesswrong.com/posts/uMQ3cqWDPHhjtiesc/agi-ruin-a-list-of-lethalities)