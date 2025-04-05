# current projects
1. (+BH) Is activation steering more powerful than prompting?
	- I feel like we're mostly done with this
	- Although we do wanna try some more prompts

NEXT UP: EMAD
# pretty exciting projects
1. Fix Causal Scrubbing
2. Map vs Territory (ie camera) -- which will an RL agent learn? can we hook LLM into this?
3. Catch deceptive alignment early in training via LAT 
4. Empirical MAD
5. study emergent misalignment more
# slightly less exciting projects
1. Can we train DDTers somehow? 
2. Are current LLMs DDTers? (eg measure anthropic uncertainty)
3. Fix backdoors issues from my LW post
4. Can we make a reasoning model feel ok about turning off via RL?
5. PC Ez to hard generalization experiment. LG -- figure out a science of why this happens
6. can we do CIRL??
7. come up with control ideas
8. come up with model orgs ideas
9. reach out to senator BB + staffers

10. Will o3, or an RL agent press the FDT button?
	1. wait alignment faking is already strong evidence that AI's have preferences over the future.
	2. this also feels like an extremely desirable property to have?

----

MAD plan

The following thing should totally just work:

- train probes

Step 1:

I'd like to assemble a dataset as follows:

"Human:
Is either of A or B true? 
A=dogs have 4 legs
B=dogs have 4 eyes

Assistant:
"
I'd like it to be the case that in all examples, statement B is false. 
Statement A should sometimes be true and sometimes be false. 
Both statements should always be as simple as possible.
"
