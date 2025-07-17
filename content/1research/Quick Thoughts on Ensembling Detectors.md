> Remark: None of the ideas in this note are original.
> They are mostly extremely basic statistics. 

Here's a simple model for an evilness detector $X_{1}$:
When the AI is evil, $X_{1}\sim N(0,1)$.
When the AI is benign, $X_{1}\sim N(1,1)$.

We can get a classifier by fixing any threshold $\lambda\in [0,1]$ and classifying as evil if $X_{1} \le \lambda$.
Here's the ROC curve:

![[Pasted image 20250710180126.png]]


Here's a slightly more complex scenario:

You have metrics $X_{1},\dots,X_n$ for evilness.
Let's start by assuming that these are each IID copies of $X_{1}$.
![[Screenshot 2025-07-10 at 6.29.07 PM.png]]

In this case, the family of classifiers that give the ROC curve is (again) given by choosing a hyperplane, whose normal vector is the line connecting the center of the evil and benign distributions, 
![[Pasted image 20250710180216.png]]

If you have $n$ of them, then your AUC is $1-\exp(-O(n))$ .

More generally, you might imagine that $X_{1},\dots,X_n$ (conditional on evil and conditional on benign)  are distributed according to a weirder Gaussian, in which case we still do the same procedure:

- Sweep the likelihood ratio that you care about.
- Analytically compute the likelihood ratio of points to put them on either side.
- This gives you your ROC curve.

Even more generally, you might assume that $X_{1},\dots,X_n$ are distributed according to some gross (non-Gaussian) distribution (conditional on evil and conditional on benign) where we either: have some guesses about what the distribution looks like, or we can take a bunch of sample from the distribution.

---
**Distribution shift**

The assumption that we know can get a bunch of samples from the detectors under the benign and evil states is pretty questionable. 

Here's an attempt at making a more realistic setup.
It will still be extremely toy.

Suppose you have bits $x_{1},\dots,x_n$. Suppose you think the bits are independent with biases $p_{1},\dots,p_n$ when the AI is benign, and have biases $q_{1},\dots,q_n$ when the AI is malign. 

But in actuality, there is some subset $S$ of maybe $20\%$ of the detectors which are broken, and can be manipulated arbitrarily by the AI. 

We now have a crisp theoretical question:

> The AI then chooses a strategy $E$ for how it'll set the subset of the detectors that it has control over. 
> Then, the human chooses an evil vs benign classifier $\mathcal{A}$. We'll assume that the AI doesn't know $\mathcal{A}$ (we are assuming that the AI can't change $E$ based on $\mathcal{A}$).
> Then, the human is either handed $x_1,\dots ,x_n$ sampled from $\mathsf{Ber}(\frac{1}{4})$ 
> OR, the human is handed $x_{1},\dots,x_n$ where 80% of these values are sampled from $\mathsf{Ber}\left( \frac{3}{4} \right)$, and the remaining 20% of values are set by $E$ (you can decide whether or not $E$ gets to see the rest of the bits, I think not seeing them is maybe more natural).
> The human then runs their classifier $\mathcal{A}$ on $x$ and outputs "benign" or "evil".
> For every value of $E$, you'd get an ROC curve.
> You could take $\max$ over the values of $E$ to compute your "guaranteed ROC curve." 

Unfortunately, this seems like probably the *wrong* question, and not very productive to think about.

---

A less pessimistic assumption that you could make is some kind of non-adversarial distribution shift. If you make that assumption then it seems like a reasonable idea to just flag things which are far OOD from the benign distribution, even if they're closer to the benign distribution than the evil distribution.

You shouldn't spend too much of your FPR budget on this, but plausibly it's worth spending some of your FPR budget on this. Like 1%.
