> research methodology aside: 
> - First make vague high level questions 
> - Then try to cash these out as mathematical conjectures or empirically testable hypotheses!

goal1: in the next half hour, write down 5 interesting "big questions"
goal2: in the next half hour, turn 1 of these into a math or coding question

q1: How would strong deceptive alignment arise? (give a training story where it feels very plausible or where we already have empirical evidence)

q2: Are there training regiments that feel unlikely to result in deceptively aligned AIs?

q3:  Conditioning predictive models paper had a lot of oqs about testing empirically whether or not models: 
1. seek convergent instrumental goals
2. are non-myopic due to weird decision theory reasons

q4: mark and Evan ask 
"Can you get an agent that implements DDT?"
I might ask -- is o3-mini not a DDT-er?

q5: re Rubi's paper -- can we incentivize accurate predictions that don't change probabilities much?
is this even a reasonable thing to ask for? a good thing to ask for?

q6: re Rubi's paper -- 
They were only able to get some extremely limited results in the binary prediction case, and even then the results were extremely problematic due to requiring exponential precision about some number. It seems like much better things should be possible. The plan would be to come up with my own models instead of looking at their models and just choose the models such that I get nice results.

q7:
empirically solve the simple MAD problem proposed in [[MAD Agenda]]

q8:
Define ELK 
think about it a bit

q9: How could you inefficiently test how good an explanation is?
	 see [[MAD Agenda]], [[Causal Scrubbing Notes]]

q10: (DONE)
re [[backdoors take 2]] -- 
We had that problem that was always statistically possible, but probably often computationally intractable. Is it computationally intractable in NP?

q11: 
"Beat $1/T$"
Maybe ARC has some reasonable approach to doing this? I can't tell.
Also lots of other things you could try. 

q12: 
formalize what I mean by "if statement" or "random" triggers for deception.
and then try to go empirically find them. 
I think adamj had an idea somewhere called "LAT" which is like RAT but not quite. 
NS and I have discussed LAT before and adamj came to similar conclusion that it's not exactly clear how to define this but it sounds pretty interesting. 

~~q13: ~~
[Read Rubi's latest post on myopia](https://crossingtherubicon.substack.com/p/myopic-goals-without-myopic-capabilities) -- try to do something with it.

ok i just read this and...
it seems kind of problematic
seems like his suggestion is to condition an AI on not being shut down
this seems not workable

im not even sure corrigibility is a desirable thing 

anyways id like to think about myopia first seems v subtle

q14: 
N points out "maybe you don't want your AI to defect at prisoner's dilemma" -- we'd prefer nice AIs not ruthless ones. Is this true? how to resolve tension with the desire that ais dont coorindate against humans.

----

ok, which questions am I the most excited about?

- q5 + q6 --- also feel like these are pretty close to being math questions, and possibly extremely tractable over the scale of 2 weeks.

- q7 --- feels plausibly tractable over the scale of 2 weeks.

- q3 -- seems pretty tractable eg scale of 1 week. If it only takes this long I'm not too distressed if someone has done something similar in the past

- q4 -- seems extremely ambitious and hard to define. q3 is a prereq. it sounds exciting though. more like 1 month scale of a project.

- q9 -- also sounds very cool and a bit hard to pin down. possibly is like a 1 month scale project.

- q10 -- maybe month scale. exciting

- q11 -- month scale. exciting

---

plan: 

let's start with q10.
reporting back in 1 hr.

report: it's not obviously possible or obviously impossible. 
it's now re-cached. nice! keep going after lunch.
carp that was bad pizza. ok, continuing regardless.

---

ok, lets think step by step.

I'm going to prove something moderately ridiculous and then improve it. 

