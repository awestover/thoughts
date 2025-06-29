Often, I'll send person X a message like "hey do you want to do activity Y?". You can imagine situations where you'd like to only give this bid to person X if their answer is yes. In cryptography this is called "ZK-dating" (ok actually I just made this name up).

Formally, the problem is that Alice and Bob want to compute $A\land B$ together but such that each party learns nothing more than $A \land B$. So basically the question is "hey do you want to go on a date" A is Alice's answer B is Bob's answer. If Bob says no then he learns nothing about Alice's preference. If Bob wanted to learn Alice's preference then I guess he could lie and say yes, but this seems like it'd be pretty awkward in most situations where you'd want to apply this, so I'll assume that this incentives honest behavior. 

This has at least two problems:
1. If Alice tells Bob "hi can you answer this question Q: do you want to go on a date" --- this might give Bob some information -- like it's moderately weird for Alice to send him this if her answer is no. 
2. Bob needs to trust that Alice's crypto scheme is set up such that his data is correctly private from her. 

(2) could obviously be solved by having a trusted 3rd party setup the service, but I'll assume this is off the table. 
Another way to solve (2) is Alice can open source her code, show that the open sourced code doesn't allow her to find Bob's responses (they are not stored in plain text and for some reason Alice literally just cannot learn them) --- wait actually this kind of sounds impossible...
like if Alice is allowed to try both responses on Bob's thing...
ok anyways that sounds like a maybe solvable technical problem? moderately unsure. 
Let's move on for now I'll think about that more tonight. 

But what can Alice do about (1)?

Here's a possible scheme: 
 - Alice has some larger form thing that just exists on her website. 
 - This form has multiple inquiries, and at least one of them is pretty likely to apply to Bob
	 - like an inquiry could be "do you want to get lunch sometime" which most ppl would probably say yes to.
 - This (hopefully) makes it not awkward for Alice to send this to a variety of ppl.
	 - Like the form has to be sufficiently general such that Bob receiving the form is receiving very little information.

----
note: these things should probably have timers on them. 
like Bob and Alice need to fill out the forms within a week of each other to get the answers, bc ppls answers may change over time. 

----

so it feels like there are probably ways to overcome problems (1) and (2).

at this point there is another problem:
3. Is there a way that Alice can make the form customizable to a specific person?

I think the short answer is, no doing this would sacrifice the privacy guarantee. 

some more answers:
- If Alice just has a large number of possible activities on her list then maybe she can just cover any particular activities. Seems fairly plausible.
- Alice and Bob could write free form what they'd like to do together -- a trusted third party or LLM could look at their answers and find overlap 
	- this is pretty limited though bc if proposing specific things low pr they think of the same things, and maybe Bob would love X if he knew about it but just didn't think about it.

so probably this type of system is not going to solve all problems of this form but can solve a useful subset

----
What are the pros and cons of this proposed technology?

pros: 
- this makes bids for social stuff more efficient 
	- I suspect that many ppl, at least me, underbid on social things bc 
		- they're just bad
		- they do things "one at a time" bc this is a social norm
		- but it might be more efficient to batch.
- This type of thing can incentivize honesty, somewhat similarly to "anonymous voting" 
	- Ppl aren't worried about how a yes answer will reflect on them.
- This kind of thing gives a good excuse to have a conversation "what would you like our relationship to look like" -- this kind of thing strongly doesn't happen with casual acquaintances, and I do this very rarely with ppl, but it seems possibly really valuable to do.
- I think the fact that it makes it easier to **say no** to things you're not interested in is a big deal!
	- In particular, this makes Alice a lot more confident that if Bob says yes to going to the art museum it's because he actually wants to go to the art museum not just because he's bad at saying no to things.
- **like this doesn't preclude other ways of making connections with ppl, it's just an additional bonus way.**

cons:
- ok maybe a con is that it might take me 5hrs to set up infrastructure for this, and might cost $5/month to host it
	- I might be willing to pay that to put comments on skyspace3 anyways...
	- probably lots of things i would do with a little droplet
	- i could at least pilot for a month or two at pretty low cost. 
	- maybe i could let other ppl use this tool too and ask them to pay a dollar or two for server costs. with a $20 bonus if they make their new best friend or get married through this app. 
- one other concern you might have is, for instance, Bob might think a lot more seriously about going on a date with Alice is she actually asks him, rather than sending him some long form.
- A related issue is that if there is a long list then you need to make choices, and choices are hard.

#### conclusion
This idea seems pretty promising --- maybe I'll code it up sometime. 
But probably not anytime soon.

Before coding this up it's probably worth considering if there aren't easier solutions to this problem.
oooh... off the top of my head, here's an easier solution:

Alice sends some string to Bob (which is clearly some encrypted answer thing)
Then Bob gives his answers and just runs this whole thing on some python program on his computer. (or something)

Like basically I'm saying that the most annoying part of building this seemed to be that it'd require a database, but maybe I don't actually need a database. 

some other "ZK" things you could do (maybe): 

Alice says "I would like to cook 3 times a week with you Bob"
And Bob says "I would like to cook 5 times a week with you Alice"
and then they both learn the **min** of the two times.

I'll still have to think through the math, but a priori it seems pretty promising.
another thought is that I wish it were possible to make it unlockable only once.
I wonder if this is possible by forcing the user to have some state on their machine -- ah but they could reset it darn. yeah no it seems like "honor system" is the best way to go here.

#### other random thoughts: 
- im excited about sending ppl letters and getting letters from ppl --- wonder if there's a way to facilitate this?
