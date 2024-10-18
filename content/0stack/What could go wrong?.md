**edit: upon further contemplation I think the objection "SGD is not sufficient optimization pressure for something to go wrong with this" might be accurate. I'm not sure. Requires more thought.**


Suppose you had an agent. And it's [[goodness(universe)|objective function]] was 
$$
J(\theta, X) = \frac{1}{N}\sum_i -y_i \log f_\theta(X_i) - (1-y_i)\log (1-f_\theta(X_i)) + ||X||^{2}.
$$
Then, you applied an optimization procedure in an attempts to find a value of $\theta$ to make $J(\theta,X)$ as small as possible. Assume $\theta$ is like 100TB of data.

> Q: What could go wrong?

I was having a nice conversation with my friend Kevin1 about this and it took me about 5-10 minutes to think of the answer. The answer is pretty simple, but I still encourage you to think about it for 5 minutes before reading the remainder of the post.

-----
----

ok, hopefully you've thought about it a bit now. 
Here's what I came up with: (note this is not original, and I had probably heard something similar before, but I don't have a specific ref).

Maybe let me start with a wrong answer:
- "change the training data to make the optimization task easier."
This is WRONG. The agent is not concerned with finding $(X,\theta)$ that minimize $J(X,\theta)$. It only cares about finding $\theta$ that minimizes $J(X,\theta)$. $X$ is a fixed reality that cannot be changed. 
Maybe the humans had some goal like "the model should generalize well" or "the model should be helpful to humans".
And, if you only apply weak optimization pressure, then maybe that's exactly what happens. 
The regularization term rewards learning a model that generalizes well, and lower loss means better capability at whatever task (e.g., next word prediction) that the computer is employed in. 
So my initial doom answer of "take over the data-center storing your training data and rewrite it" actually doesn't hold any water. The agent does not care about doing this. 

Well, actually the agent probably does care *a lot* about protecting its objective function (if you apply enough optimization pressure that is). If the training data $X$ got lost, then it'd be harder to find $\theta$ minimizing $J(\theta, X)$. So that's a reason why the agent should seize power and create lots of backups of the training data. But I digress.

Anyways, I think my favorite answer to this question so far is, if you optimize hard enough then the following strategy is pretty good:
- Obtains as many computing resources as possible.
Why is this good?
Well, then you can try a bunch more things to find better $\theta$ values.

So, hard enough optimization of this utility function results in extinction of humans. 

Now, at this point you're probably like: "SGD doesn't optimize that hard. and neither do any RL techniques".
I don't really buy this. AI can do a lot of stuff: 
- chess
- art
- write
- code
- trade (stocks)
- speak every language
- play video games
- biology (alpha fold)
- math competitions (IMO)
- any standardized test
- drug discovery
- legal stuff (e.g., predict crime rates or whatever)

After saying this someone will respond
"But it can only do these little tasks, it can't do things that require long-term planning."
"Current models can't replace scientists or business people"
But I dunno. It seems like benchmarks are defined and beaten pretty regularly. The trend is concerning.

You might also say "$\theta$ being size 100TB is not large enough to be this dangerous". 
 My current not super informed belief is that artificial neurons are about comparable to human neurons, so I don't buy this argument. But maybe. 

So maybe the tldr of this post is, don't let chatgpt buy stuff on amazon for you, because it might try to compute on AWS.

Oh sorry, I think another concern that people will have with this argument is "if AI starts doing anything sus people will get smart and turn it off".

So, you are correct: there is a strong incentive for a computer to wait until it has a decisive advantage before acting. A computer should never give humans a chance to realize that the computer is OP before the computer is strong enough to prevent humans from pressing the off switch.

Another concern that you might have with this argument is that SGD doesn't reward being patient. 
SGD is all about "just go in a direction that probably vaguely looks good."

I mean so I could make the argument that SGD will eventually explore enough of the parameter space to find some $\theta$ that is really bad for humanity. 
But I think you could make a pretty good counterargument that random walking won't ever find such a $\theta$; or at least not in the next 100 years. 

So is SGD better than random walk at finding such bad $\theta$'s?
I mean yeah probably. 
Better enough though?
My empirical guess is "I give it even odds".

I dunno somehow I instinctively feel like RL is more dangerous. RL feels very "human like" / evolution like. It's just try some stuff and see what works and try to do better. 

----
----
Maybe some related questions: 
- Does there exist a 100KB large python3.11 file that, if run, causes the world to end in less than 1 month?
	- What if you let it be 1TB large?
- Does there exist a proof of $P\neq NP$ that can be written down in less than 500 pages of LaTeX?

My guesses: yes and yes. 
Why? Code and language are very expressive. 
I mean for a program that ends the world a simple example could be a program that sends an email to a protein-synthesizing factory with instructions to synthesize "Covid25 extreme edition" and then have them ship it somewhere, and then hire someone online to pick up this package and somehow get infected. 
Maybe it'd be simpler to send the Covid25 formula to a terrorist organization.
Or maybe send it to a well meaning scientist, and tell them that it's a cure for cancer.

Probably there's also code that could do something pretty interesting on social media or in the stock market to win the game. 

Is there a short proof of P vs NP?
I mean probably. Unless maybe not.
The statement $P\neq NP$ means "finding proofs is hard", not "there are no short proofs"
