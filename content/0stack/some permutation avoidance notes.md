I've been thinking about this permutation avoidance question a bit lately. 
I was pretty excited about it today, because a friend and I came up with a neat win-win analysis idea for a special case. The thing that made me excited was a feeling of getting a tiny bit of traction on the problem (maybe) and making a cool high-level plan for how to execute the plan. 
As discussed [[goodness(universe)|elsewhere]] "making cool plans" is one of my terminal values. I guess I'm just writing this down as a reminder to myself as a reminder that there are lots of good and beautiful things in the world. And a reminder to strive to increase the number of good and beautiful things in the world. 

This reminds me of an interesting vein of ethical(?) questions:
> "Suppose you had a friend that wanted to work somewhere evil (e.g., TikTok). Is it right to dissuade them?"\
> "Suppose you lived in a future where scarcity isn't an issue (because the robots are pretty efficient), and maybe there are just no serious issues. You have a friend that wants to play video games all day. Should you tell them not to?"

Or maybe the question I'm really trying to get at is\
> "If you think someone's doing something wrong, do you have the responsibility to tell them so? Is it even good for you to tell them so? Or should you just respect their decisions?"

Well, this post has gone really far afield. 
But, I think these are important questions so let me spend 10 secs to give gut instinct answer: 

- I'm super into the idea of responsibility/deontology. So I won't say you have a responsibility.
- But probably it's good to do so. 
- I mean, you should probably be somewhat delicate how you go about it. 
- And you should be pretty willing to accept that ppl don't see things the same way as you , and conditional on that just give up the endeavor. sometimes you might even have a strong enough prior in this direction to justify no action.

Anyways, I originally wrote this post as an excuse to share the following humorous dialogue. 
If you can't see the humor then that's really too bad. 
If the statement being proved seems obvious, then that's called hindsight bias.

---

**Averaging  argument:** (Proof by contradiction)
"I have a length $n$ array with $\Omega(n)$ disjoint copies of $\pi$ in it", Alek said.
"And, let $\pi = \phi \psi$ where $\phi,\psi$ are length 2 permutations", Alek continued. 
"And assume that these copies of $\pi$ where in the M2 case, by which I mean the $\phi$ part of the permutation occurs in one connected component (just assume 1 box for now) and the $\psi$ part of the permutation occurs in a different connected component".

"Well in that case, I think it'd be pretty easy to find one of your $\pi$'s!", JJ exclaimed.

"Ah!", the liar said, "that's where you're wrong!" 
"Hmm", JJ  said. "I'm inclined not to believe you.
"Let's call a box $B$ **$\phi$-good** if $\Omega(n/m)$ many of your $\pi$'s have their $\phi$ part in $B$. Define **$\psi$-good** boxes similarly."
"Now," JJ said in a very reasonable tone, "If you had a $\phi$-good box and
there was a $\psi$-good box such that when you put these two together you got a
$\pi$, well then I think I could find a $\pi$ pretty easily."

"And just how would you do that!!" Alek challenged.
JJ: "well, I'd just iterate over the boxes --- remember Marcos and Tardos told us that there weren't too many --- and check each one for being $\phi/\psi$-avoiding. This takes time like $O(m)$ ish."

"Well, well, fair enough" Alek admitted. 
"But I think you'll have a bit more trouble with my array!"
"Because my array has the following properties:"
- The number of disjoint copies of $\pi$ that start in a $\phi$-good box is $o(n)$

"Hold on..."
- Fix some box $B$ which is not $\phi$-good.
- The number of disjoint $\pi$'s that start in $B$ is at most $o(n/m)$ --- this is just by the definition of what failure to be a $\phi$-good box means.
- So the total number of disjoint $\pi$'s that start in non-$\phi$-good boxes is at most $m\cdot o(n/m) = o(n)$.
"So, you definitely do have $\Omega(n)$ disjoint copies of $\pi$ that start in $\phi$-good boxes."

Alek: "oh, you're right, I must have miscounted. No matter!"

JJ: "I'm not sure what you mean. I think you're pretty screwed at this point."
"Specifically, you just told me that the number of disjoint copies of $\pi$ that start in a $\phi$-good box is $\Omega(n)$"

Alek: "AND?"

JJ: "Well, then by averaging argument there is a box $B$ such that $\Omega(n/m)$ of these disjoint copies of $\pi$ that start in $\phi$-good boxes end in $B$. Remember this is because there are just $O(m)$ total boxes by Marcos Tardos. So whatever this box $B$ is, it's looking like a $\psi$-good box."

Alek: "So? what're you gonna do about it?"

JJ, with much patience "Well, I mean this is just exactly what I said I wanted a couple lines earlier. It's a $\phi$-good box which is completed by a $\psi$-good box. I told you earlier how I would find this."

Alek: "ok, I guess you got me."

JJ: "It could well be said that this conclusion was un*avoidable*."

---
---

**Appendix**
I've been thinking about how to make $3412$-av detection faster than $2^{O(\sqrt{ \log n })}$.
It seems a bit tricky, but I think I was able to solve it if we assume that the *range* is small.

Specifically, let's assume that the range is $[R]$ for some $R\ll n$.
Then we can do $O(R^{3}\log n)$. 
Here's how:

Let's say that $y\in [R]$ is **good** if at least $\Omega(n/R)$ many disjoint $\pi$'s start at height $y$. 
Note that there exists a good $y$.

Now, fix such a $y$.
Choose a random $x\in [n]$.
There is like a $1/R$ chance that this $x$ does a nice job of splitting the $\pi$'s that started at $y$.

Now, we look for ascending patterns which are "before $x$ and above $y$" and also ascending patterns which are "after $x$ and below $y$".

Let $k$ be the number of points which are "before $x$ and above $y$".
Then our "$\varepsilon$" for this is like $(n/R)/k$
And the number of samples per real sample is like $n/k$.
Overall, the avoidance testing should work in time $(k/(n/R)) \cdot n/k = R$.

So, our overall time is 
- $R$ for iterating over $y$'s
- $R$ for trying $x$'s
- $R\log n$ for the length-2 avoidance testing (because of epsilon deterioration / ER stuff).

Overall it is $O(R^{3}\log n)$

**Alek's Conjecture**
Probably you can do $R\log n$.

**Bigger picture thoughts:**
- I think the idea of "repairing" things has potential for helping us beat  $2^{\sqrt{ \log n }}$ for $\pi=3412$
- But sometimes you can't repair things. 
- So it has to be a win-win analysis
	- either we can repair things
	- or we win for some other reason.
- Note that the recurrence that we don't like is $T(n) = m + 2T(n/m)$.
	- So actually it seems really hard to improve this recurrence. 
	- So maybe we should just do completely different approach.
	- Even though the simple splitting stuff that we were trying didn't work, maybe there's still something we can do.


