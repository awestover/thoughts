A proposition is true iff it describes the way that things are.

There are two main flavors of propositions that I’ll discuss: 

- Propositions about the world.   
- Propositions about mathematical objects.

(Epistemic status: Pretty confused.  Thanks to Alexa, Nathan, James and Vivek for discussion.)

First I’ll discuss propositions about the world. I’m going to take a realist stance: I’m going to assume (because I want to) that a mind-independent world exists. Then I will posit that the state of the world at time t is described by a list of where all the fundamental particles are and what their momentum is; this is not correct or even coherent because of QM, but I don’t expect that talking about the universal wave function is actually going to add to this discussion so I’ll pretend that the universe is classical in this document. For convenience I’ll adopt a frame of reference with Alek at the center of the universe and call Alek's birth time 0 and adopt some coordinate axes. Then, I'll take statements like "there is an electron at spacetime location (1 km, 0, 0, 24years)" to be **primitive world propositions**. Primitive propositions are said to be true iff they are accurate descriptions of the state of the world.

I claim that propositions about the state of the world are mathematical propositions with some variables referring to primitive world propositions. I haven’t defined mathematical propositions yet, but for now pretend that what I mean is first-order logic (FOL) on ZF (a standard set theory). A proposition about the state of the world is true iff the mathematical proposition is true when you substitute in the appropriate truth values for the primitive world propositions in the mathematical proposition. This allows us to make propositions about the world like “Alek threw an apple into the air and then it fell on his head”. 

Note: my theory of truth is pretty similar to “correspondence theory”, which I’ll discuss in a bit. 

Anyways, I claim that to analyze truth it suffices to analyze truths about mathematical objects. This is not because “math is all that there is” but rather that any complex statement about the state of the world can be expressed as math \+ primitive propositions about the state of the world, and the truth of primitive propositions is taken for granted. Unfortunately, mathematical truth seems very confusing. I’ll give two attempts at operationalizing it. 

## Attempt 1\. Provable in ZF

**Definition 1:** A statement is TRUTH iff it is provable in ZF. Statements which are not provable or disprovable in ZF are said to not be TRUTH and also not be UNTRUTH. Let's call them NTNU (not truth, not untruth). Note that a statement which is NTNU will by the law of the excluded middle (which holds in ZF) be true or false in any particular model of ZF. So I'm perhaps terribly abusing notation by calling such statements NTNU. Also, the idea that some statements are NTNU is pretty unfortunate. Note that I'm talking about well-formed coherent statements, not incoherent statements like "the set of all sets that don't contain themselves" (not a coherent statement in ZF).

By “X is provable in ZF” I mean that you could write down a string (a proof), and if you parsed the syntax of that string you'd get something that was a valid way of deducing X from the ZF axioms.

Now I'll list the good, fine, and bad parts of this attempt at defining mathematical truth.

### good parts:

- We can say that 2+3=5 is TRUTH.

### fine parts:

- We can't say that the axiom of choice is true / false. idk how big a deal this is.  
  - This means that very basic statements about infinities like “Suppose you have countably infinite ppl in a room, they have black / white hats on, they each say a color, goal is to get finitely many to answer wrongly” don't have a truth value. In some sense this feels pretty bizarre if the universe is spatially unbounded. But I'm not sure how morally relevant this is.

### bad parts:

- ZF is just some random axioms that someone wrote down. I don't even know the ZF axioms, and they're moderately complicated.  
- Statements like “this TM halts” intuitively seem like they should have a truth value even though ZF can't prove them.  
- Having propositions without truth values seems pretty bad too. 

## Attempt 2\. Physically instantiated python

We only care about statements of the form "this python program halts".   
**Definition 2\.** Such a statement is true if when we run it on my laptop the program indeed halts.

### good parts:

- Checking whether a program can halt is pretty expressive. The halting problem is complete for recursively enumerable functions.

### bad parts:

- What if a cat walks on my keyboard? What if my computer runs out of batteries?  
  - These problems can be partially mitigated by just leaving some milk out for the cats to drink so that they won't mess with my computer. But then they might spill milk on my computer.  
- It kind of feels like statements of the form “exists x such that for all y exists z such that for all w we have HALT(M(x,y,z,w))” should either be true or false. But this theory doesn’t agree—it refuses to assign truth values to such statements. Maybe these aren't the most important statements to consider though—if it had a truth value we'll often be unable to know this truth value.

Despite these issues this is the best formulation of truth I have so far.

# Neo-classical accounts of truth

In this section I’ll present neo-classical accounts of truth. 

## Attempt 3: Correspondence

"A statement is true iff it corresponds to some fact or state of affairs in the world".

### good

I think this arguably can handle Delta4 statements. It just feels like having quantifiers over stuff is still legit to call the state of affairs in the world. 

It's fairly intuitive.

How it handles 2+2=4:

- If you put 2 cats in a box and then put 2 cats in a different box and merge the boxes you get a box with 4 cats in it. Seems good. 

How it handles this program halts:

- If you ran this program it’d halt. 

### bad

- I don't like the word "fact"—how are facts different from truths?   
- I don’t think this theory is well equipped to assign truth values to things which aren’t “states of affairs in the world”, which might be sad. 

This is my favorite neoclassical theory.

## Attempt 4: Coherence

"A proposition is true if it coheres with the rest of one's belief system"

### good

### bad

- False things can be coherent. This seems pretty damning for this theory.  
- You can try to patch it by saying “you have to use some nice axioms (like ZF)”, in which case it seems like you should’ve just defined truth as things provable in ZF, or you could patch it via empirical grounding in which case you should’ve just done correspondence IMO.   
- You could also try to patch this by being anti-realist about truth? I don’t like this tho. 

## Attempt 5: Pragmatic

"A proposition is true if it works i.e. leads to successful predictions, good actions, or would be agreed upon in the limit of inquiry".

### good

### bad

The "what people would converge to upon reflection" thing seems unfortunate imo. Some issues:

- it denies that certain first-order statements about the integers have truth values, (bc the halting problem indicates that reflection won't converge)  
- I think the agents that Peirce is imagining seem really hard to define\!  
- it seems like some things should not be stable under reflection. For instance suppose Alek reflects and tries to answer the question "Will Alek answer no to this question?".Suppose I answer yes, I'm wrong. Suppose I answer no, I'm also wrong.

The “truth is what works” theory has the obvious problem that sometimes truth doesn't work. Even though it really is dumb to buy a lottery ticket you could still win the lottery. I think they try to patch these holes by saying that you have to win in the long run or sth but I don’t like it. A strawman of this position would say that 2+2=5 is true if you are being tortured by Big Brother. 

## Attempt 6: Tarski’s theory of truth

Here's a moderately troubling sentence: "This sentence is not true"

Does this sentence have a truth value?

Tarski tries to handle this by saying:

- Truth is a property defined in a metalanguage for sentences about objects in the object language.

Besides this he basically says "S" is true iff S.

This basically is the same move that people play in set theory to get around "the set of all sets that don't contain themselves" being undefined.

### good parts:

Now consider the sentence "This sentence is not true" We can analyze it in the metalanguage. The sentence "This sentence is not true" is not a legal sentence because truth is not allowed to be defined in the object language only in the metalanguage. This is a good outcome: it's nice that it avoids paradox.

However, it’s not clear to me that this was really a problem in the first place. I think that correspondence theorists would not have felt a need to say that the sentence is truth-apt.

### bad parts:

It’s pretty vague. My best guess is that when he says “S” is true iff S, he maybe wants to invoke correspondence theory if we’re talking about statements about the world, and maybe some arbitrary set theory like ZF if we’re talking about logical statements. But I don’t really know. 

A general problem is that it feels like we can say: 

- Things are true iff they’re true in all models of ZF (i.e., if they’re provable in ZF). But this seems really bad, because we should allow true statements which aren’t provable. And it’s not clear why we are saying that things like choice are false.   
- Or, we can fix a particular model of ZF and say that things are true iff they are true in this model. But I don’t know that we can really point to a particular model of ZF (I think it’s probably impossible). So this seems problematic as well.