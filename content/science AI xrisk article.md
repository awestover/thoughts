[[main|Updated version of this post here]]


I've spent some time, e.g., [[AI xrisk|here]] writing about the risks posed by AI, and what to do about it. 

[Here's an article](https://www.science.org/stoken/author-tokens/ST-1870/full#body-ref-R3-1) that does an awesome job of discussing this. 
A bunch of famous ppl like Geoff Hinton are authors on the paper, giving some authority to their quantitative numbers claimed, and to their assesment of the field and the difficulty of the problems that we face.
Here are my notes/summary of the article.


-----

AI development fast. AI dev poses risks.
We're not on track to handle the risks yet. 
They're going to propose

> "(more) technical research \
> AND 
> proactive, adaptive governance mechanisms"

**Progress speed**:
- companies can scale more
- 2.5x gains/year from algorithms, 1.4x/year from hardware, some amount from money
- no reason to think AI's stop progressing at human level
	- in fact AI's have lots of advantages, e.g., easy to copy and high bandwidth share learning btwn AI's

Ppl are trying to build AI systems that act in the world in pursuit of goals.

>"Without R&D breakthroughs (see next section), even well-meaning developers may inadvertently create AI systems that pursue unintended goals"


> "Once autonomous AI systems pursue undesirable goals, we may be unable to keep them in check. ... To advance undesirable goals, AI systems could gain human trust, acquire resources, and influence key decision-makers. To avoid human intervention ([_3_](https://www.science.org/stoken/author-tokens/ST-1870/full#core-R3)), they might copy their algorithms across global server networks ([_4_](https://www.science.org/stoken/author-tokens/ST-1870/full#core-R4)). In open conflict, AI systems could autonomously deploy a variety of weapons, including biological ones. AI systems having access to such technology would merely continue existing trends to automate military activity. Finally, AI systems will not need to plot for influence if it is freely handed over. Companies, governments, and militaries may let autonomous AI systems assume critical societal roles in the name of efficiency. Without sufficient caution, we may irreversibly lose control of autonomous AI systems, rendering human intervention ineffective."


> "We are not on track to handle these risks well. Humanity is pouring vast resources into making AI systems more powerful but far less into their safety and mitigating their harms. Only an estimated 1 to 3% of AI publications are on safety (see SM). For AI to be a boon, we must reorient; pushing AI capabilities alone is not enough."

(I'd add that 3% is a very generous overestimate of how much useful safety work is going on).

**Proposal 1: ask companies to invest more in safety stuff** 

Capabilities plausibly just improve by throwing more compute at existing algorithms. 
Safety doesn't work like this. 
They propose "if we want safety to stay ahead of capabilities, maybe should allocate at least half of budget to safety stuff". 
I don't think that this is enough, but it's better than the current situation (my understanding is that safety gets much much less than 1/2 of  budget).


**Proposal 2: Governance measures**

I loved this section. Here are some favorite quotes. 

> **"Rapid, unpredictable progress also means that risk-reduction efforts must be proactive."**
> **"The key is policies that automatically trigger when AI hits certain capability milestones"**

> "To address international race dynamics, they need the affordance to facilitate international agreements and partnerships"

> "The most pressing scrutiny should be on AI systems at the frontier: the few most powerful systems, trained on billion dollar supercomputers, that will have the most hazardous and unpredictable capabilities."

> **"Regulators can and should require that frontier AI developers grant external auditors on-site, comprehensive (“white-box”), and fine-tuning access from the start of model development"**

For real --- the whole idea of making a super powerful super evil thing and then checking *afterwords* if it's nice is a disaster. 

The proposal to un-handicap external evaluators (who don't currently get white box access or ability to fine tune, and only get to do stuff at the end and only for very short amount of time) seems pretty good.


> **Safety cases Despite evaluations, we cannot consider coming powerful frontier AI systems “safe unless proven unsafe.” With present testing methodologies, issues can easily be missed. Additionally, it is unclear whether governments can quickly build the immense expertise needed for reliable technical evaluations of AI capabilities and societal-scale risks. Given this, developers of frontier AI should carry the burden of proof to demonstrate that their plans keep risks within acceptable limits. By doing so, they would follow best practices for risk management from industries, such as aviation, medical devices, and defense software, in which companies make safety cases ([_16_](https://www.science.org/stoken/author-tokens/ST-1870/full#core-R14), [_15_](https://www.science.org/stoken/author-tokens/ST-1870/full#core-R15)): structured arguments with falsifiable claims supported by evidence that identify potential hazards, describe mitigations, show that systems will not cross certain red lines, and model possible outcomes to assess risk.**


We're pretty excited about this tech, there are lots of examples of governments regulating stuff too hard and that being bad. I get it. But hopefully it's clear that this isn't a case where it's reasonable to put burden of proof anywhere but the companies developing this tech. If you don't think the tech is dangerous then you shouldn't oppose this policy --- just go write a solid safety case and then all the concerned people will have to shut up. 

> Regulators should clarify legal responsibilities that arise from existing liability frameworks and hold frontier AI developers and owners legally accountable for harms from their models that can be reasonably foreseen and prevented, including harms that foreseeably arise from deploying powerful AI systems whose behavior they cannot predict

For real! If you open-weight your model and someone fine tunes it to remove safety, or does something else nefarious. Then well this should just be illegal but also you should be liable.

> To steer AI toward positive outcomes and away from catastrophe, we need to reorient. There is a responsible path—if we have the wisdom to take it.
