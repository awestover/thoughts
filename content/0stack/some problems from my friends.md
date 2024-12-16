A principle of interacting with people that is highly under-utilized is asking people for help. It's possible to go too far with this, but I suspect you aren't. The easiest way to employ this interacting with people strategy is, when interacting with someone you can think about the set of problems that you're currently dealing with and talk about them with other people. A note of caution is that it's bad to try to put responsibility for any decisions on other people -- this is not [[responsibility|responsible]]. But this is a service that most people are happy to provide and sometimes it'll be useful. In my latest instantiation of trying this I asked some people for some cool math problems, because I wanted to get some exposure to some different topics. This saved me a lot of time trying to find good problems. Thanks!

# Part 1 -- Nathan -- Complexity Theory

### info theory basic facts
Let $X^{n}$ be an rv. 
Choose $S\subseteq [n]$ independent from $X$. 
Shearer's Inequality: 
$H(X_S\mid S)\ge H(X)\min_i \Pr[i\in S]$.

This is saying, the average info you get from looking at a random subset of the coordinates of $X$ is at least the entropy of $X$ times the smallest probability of a coordinate being revealed. 

This is obviously tight: suppose that the entropy of $X$ is all due to a single coordinate (all other coords are deterministic). Then this'd be tight. 

$$D_{KL}(p||q) = \sum p(x) \log (p(x)/q(x)).$$
Cross entropy: $H_q(p) = \sum p(x) \log(1/q(x)).$
Joint entropy: $H(X,Y) = H(X)+H(Y\mid X) = -\sum p(x,y) \log p(x,y)$.
Mutual Info: $H(X,Y)-H(X|Y)-H(Y|X)$.

crisp example of DKL asymmetry: 
- $p=1/2,1/n,1/n\dots$
- $q = 1/(n/2+1),\dots$

$DKL(p||q) \approx .5 \log n$
but $DKL(q||p)\approx 1$.

if truth is $p$ then using $q$ as your code is super bad. 
not too terrible if $q$ is the truth and you use $p$ as your code. 

**NQ1:** Prove that IP = PSPACE.
note -- I didn't figure this one out -- ended up just looking at wikipedia

The direction $\mathsf{IP} \subseteq \mathsf{PSPACE}$ is simple I think -- you can search over proofs.
To prove the other direction probably we want to work with some complete problem like "determining the truth value of quantified 3CNF formulas". 

ok wikipedia suggested warming up by thinking about sharp-SAT.
The Sharp-SAT problem is, given a formula and a number $k$, check whether there are exactly $k$ sat assignments of the formula. 

Here's how we're going to do it. 
Fix a formula $\phi$. Let $q$ be an $n$-bit prime (this is way overkill, we really only need $\log n$ bit prime; you can ask the prover to generate this for you iyw although no need) where $n$ is number of variables in the formula. Define $f$ to be an arithmetization of $\phi$. Namely, we take $\phi$, trade in $x\land y$ for $x\cdot y$ and trade in $x\lor y$ for $1-(1-x)(1-y)$.
Define 
$$
f_i(x_{1},\dots,x_i) = \sum_{x_{i+1},...,x_n\in \{0,1\}} f(x_{1},\dots,x_n).
$$
The protocol is as follows: 

1. Ask the Prover for the value of $f_{0}$ --- this is supposedly the number of SAT assignment of $f$, if the Prover is being honest. The Prover hands over some number $f_{0}'$ which may or may not actually be $f_{0}$. (PS: the prime doesn't mean derivative)
2. Ask the Prover to give you coefficients for the degree $n$ polynomial $z\mapsto f_{1}(z)$; the Prover hands over some polynomial $z\mapsto f_1'(z)$ which may or may not be $f_{1}$.
	2. Check that $f_{1}'(0)+f_{1}'(1)=f_{0}$.
	3. This ensures that if $f_{0}'\neq f_{0}$ is a lie, then $f_{1}'\neq f_{1}$. 
3. Now, the verifier chooses a random $r_{1}\gets [q]$ and tells the prover about it. 
	1. Because $f_{1}'(z)$ is a low degree polynomial, the chance that $f_{1}'(r_{1})=f_{1}(r_{1})$ is pretty low. In particular low enough that we can union bound over all $n$ steps of the proof and assume that it never happens. 
4. Anyways we kind of just continue on like this. 
	1. The prover sends us $z\mapsto f_k'(r_{1},\dots,r_{k-1},z)$. 
	2. We check for consistency with $f_{k-1}'$.
	3. If $f_{k-1}'$ was a lie, so if $f_k'$.
5. Finally at the end the prover will tell us some value $f_n'(r_{1},\dots,r_n)$ and we can literally just check whether it's true or not. 

### TQBF is in PSPACE.

We're going to do something pretty similar, with a minor twist. 
Let's arithmetize the formula again. 
We're going to define $f_i$ pretty similarly to last time. 
That is, we'd like for any boolean inputs $x_{1},\dots,x_i$ for it  to  be the case that 
$$
f_i(x_{1},\dots,x_i) = Q_{i+1} x_{i+1} \cdots Q_n x_n \varphi(x_{1},\dots,x_n).
$$
In order to get this what we can do is define
$$
f_i(x_{1},\dots,x_i) = f_{i+1}(x_{1},\dots,x_{i},0) \cdot f_{i+1}(x_{1},\dots,x_i,1)
$$
if the quantifier in question was a $\forall$ guy, and do an OR thing if it was an OR. 

This is great but it doesn't quite work because the degree blows up too much. 

Luckily there is a way to reduce the degree. 
After every quantifier we intersperse these things which are like 
replace 
$$
f_i(x_{1},\dots,x_i) = (1-x_i) f_i(x_{1},\dots,x_{i-1},0) + x_i f_i(x_{1},\dots,x_{i-1},1)
$$
not just for $x_i$ but for like everyone. 
Anyways this is kind of fine because it's only polynomial overhead, and it ensures that the degrees never get too crazy. 

nice. 

**NQ0:** Prove the time hierarchy theorem + prove halting problem is undecidable.
pf: Diagonalization

**NQ2 Claim:** $\mathsf{NP}\neq \mathsf{TIME}(2^{O(n)})$.

**Proof**: 
Standard padding argument + Time hierarchy theorem.

Assume the claim is false, i.e., $\mathsf{NP}=\mathsf{TIME}(2^{O(n)})$.
Take $f\in \mathsf{TIME}(2^{n^{2}})\setminus \mathsf{TIME}(2^{O(n)})$ --- exists by a diagonalization argument.
Create $g$ which runs $f$ on the first $\sqrt{ n }$ bits of its input.
Then $g\in \mathsf{TIME}(2^{n})$, so by assumption there is an $\mathsf{NP}$ algorithm for $g$. 
If $g$ on $n$ bit input has a certificate of length $n^{c}$, then $f$ on an $n$ bit input has a proof of length $n^{2c}$ --- just by padding its input. In other words, $f\in \mathsf{NP}$.  But then by assumption  $f$ has an algorithm running in time $\mathsf{TIME}(2^{O(n)})$, contradiction.


**NQ3 Claim 1:**
Show that every function $f:\{0,1\}^n \to \{0,1\}$ has a circuit with $O(2^n/n)$ gates.
**Proof**
Note: The below proof just uses gates of constant fan-in!

There's a pretty simple way to make a circuit with $O(2^{k})$ gates for any function on $k$ bits ---  basically you just do a tree where you split $k$ times and have one leaf corresponding to every possible variable assignment. But we'd like to do a bit better than this. So we'll use the polynomial method (cite: RW). The number of boolean functions on $\log n - 4$ bits is $2^{n/16}$.
Using the simple above method we can write down a circuit for every function on $\log n-4$  bits using $O(n \cdot 2^{n/16})$ gates.
Now we can do the tree thing again, but stop when we reach the final $\log n-4$ bits. 
For these bits we can then just wire in one of the circuits that does the right thing. 
The total number of gates that we use in this scheme is 
$$
O(2^{n-\log n} + n\cdot 2^{n/16}) = O(2^{n}/n).
$$
**NQ3 Claim 2**: 
Most functions need at least $2^{n/2}$ gates. 
**Proof**
Number of functions you can make with $k$ gates is probably at most $2^{k(n+k)}$.
Hence in order to make $2^{2^{n}}$ functions, it seems like we need to make $k\ge \sqrt{ 2^{n}}$.

**NQ4**
recall that in an arthur-merlin protocol, arthur first chooses some random bits to send to merlin, then merlin responds with a proof, which arthur verifies through some probablistic polynomial time algorithm. a language is in AM if there's such a protocol with two-sided error 1%. you could also, for any k, define $AM[k]$ to be the k-round version of this, where arthur sends some random bits to merlin, merlin responds with the first part of the proof, arthur sends more random bits, etc. show that $AM[k]$ = AM for any constant k.

**Proof**
For convenience I'm going to work with a complexity class which I call $\mathsf{AM}'[k]$ instead. 
Specifically in $\mathsf{AM}'[k]$ Arthur is a deterministic function of $\{\pi_i, r_i\}$. Then we have:
$$
\mathsf{AM}'[1] \subseteq \mathsf{AM}[1] \subseteq \mathsf{AM}'[2] \subseteq \mathsf{AM}[2]\subseteq \cdots.
$$
 I'd like to show that $\mathsf{AM}'[1]=\mathsf{AM}'[100]$. To show this, it suffices to show that $\mathsf{AM}'[2] \subseteq \mathsf{AM}'[1]$.
Just for maximum clarity here's what I'm talking about. 
```
Procedure 1:

Arthur sends randomness r1
Merlin responds with proof pi1
Arthur runs a (deterministic) procedure on r1,pi1 and outputs accept / reject.  

Procedure 2: 
Arthur sends randomness r1
Merlin responds with proof pi1
Arthur sends randomness r2
Merlin responds with proof pi2
Arthur runs a deterministic procedure on r1,r2,pi1,pi2 and outputs accept / reject.  
```

Okay, how do we do it?! 

Arthur's procedure is as follows:
```
Let n^c be the maximum allowed length of pi1 in the two round protocol.  I can assume that this is polynomial because otherwise Arthur can't even recieve pi1! 
I guess you could think of some model where Arthur just gets random access to pi1? But I think we're not doing that. 

Anyways, send Merlin r1, r21,r22,r23...,r2,n^c (all at once)
And request merlin to produce pi1, pi21,pi22,...,pi2n^c
such that in the 2 round procedure we would've accepted this stuff

Arthur then runs the 2 round protocol on pi1,pi2k,r1,rik for each value of k. 
Arthur accepts if more than half of the 2 round protocol things accept, and rejects else.
```

> [!tip] Observation 1
> For a statement **in the language**, say that $r_{1}$ is **misleading** if there aren't any $\pi_{1}$ such that it's 90% likely over choice of $r_{2}$ that there's a proof $\pi_{2}$ such that Arthur accepts $r_{1},r_{2},\pi_{1},\pi_{2}$. 
> 
> For a statement outside of the language, say that $r_{1}$ is **misleading** if there is some $\pi_{1}$ such that there's more than a $10\%$ chance over choice of $r_{2}$ that there exists $\pi_{2}$ so that Arthur accepts $r_{1},r_{2},\pi_{1},\pi_{2}$.
> 
> Theorem:  the chance that $r_{1}$ is misleading is at most $10\%$.

**Proof**: Otherwise it'd violate our two-sided error bounds.

> [!tip] Lemma 2
> Fix $r_{1}$ which isn't misleading for statement X, and fix some $\pi$. If you generate $k$ many $r_{2,i}$'s and ask for $\pi_{2,i}$'s with respect to all of them, then if X is true with $1-\exp(-k/100)$ probability we'll have at least $80\%$ of the $r_{1},r_{2,i},\pi_{1},\pi_{2,i}$ pairs are accepted by Arthur (if Merlin's on the ball) and if X is false then with $1-\exp(-k/100)$ probability we'll have at most $20\%$ of the proofs accepted by Arthur, no matter how hard Merlin tries. 

**Proof**: Chernoff bouund

Now we union bound over all proofs $\pi_{1}$.
What we get at the end is, with probability 90%, the r1 is not misleading, in which case we have exponentially good probability that the fraction of proofs arthur accepts will be on the right side of $1/2$. Namely, if Merlin is honest and the statement is true then Arthur will accept more than half of the proofs, and if the statement is false then even if Merlin is "crooked" Arthur won't be tricked more than half the time.

In conclusion this means that we have a procedure that is great and it has 2-sided error $11\%$ but we can repeat it (in parallel) 100 times to get this back down to $1\%$.

Note that the blow-up in proof complexity is like, if the proof complexity before was like $n^{c}$ then now we're something like $O(n^{2c})$.
Anyways, if you just do this once or twice it'll be fine. 

But we haven't ruled out the possibility that $\mathsf{AM}[n]$ is much much stronger than $\mathsf{AM}[1]$.
I think $\mathsf{AM}[n]$ is $\mathsf{IP}$ btw.


**NQ5**
You could define a k-round interactive proof to use private randomness. That is, $\mathsf{IP}[k]$ consists of languages with a protocol similar to an Arthur Merlin protocol, but where instead of Arthur just sending all his random bits, he's allowed to compute some arbitrary function of his random coins and send Merlin only that function. show that $\mathsf{IP}[k] = \mathsf{AM}[k+2] = \mathsf{AM}$ for any constant $k$.



# Part 2 -- Alexis -- information theory

**Q1**
Fix directed graph $G$. We say that $(x,y,z)$ is a **triangle** if they form a directed 3-cycle $x\to y \to z \to x$. We say that $(x,y,z)$ is a **wedge** if we have the edges $x\to y, x\to z$.
**Theorem**: num triangles less or equal to num wedges. 

**Proof**
Let $(X,Y,Z)$ be uniformly random triangle. 
Then, $H(Z\mid Y) = H(Y\mid X)$. This is because the following both things correspond to the amount of additional surprise when you get if you choose a random cycle, and after already having revealed one of the vertices in the cycle, reveal the vertex following that one in the cycle. 
Therefore,
$$
H(X,Y,Z) = H(X) + H(Y\mid X) + H(Z\mid X,Y) \le H(X) + H(Y\mid X) + H(Z\mid Y) = H(X)+2H(Y\mid X).
$$
Let $(A,B,C)$ be a uniformly random wedge. 
We claim that: 
$$
H(A,B,C) \ge H(X) + 2H(Y\mid X).
$$
To show this we'll demonstrate a distribution on $A,B,C$ with entropy $H(X)+2H(Y\mid X)$. 
The conclusion follows because the uniform distribution on a set is the maximum entropy distribution. 
To prove the inequality, here's a distribution on wedges:
1. Sample $(X,Y,Z)$ and $(X,Y',Z')$ to be uniformly random wedges, but they are correlated (we condition on their first vertex being the same).
2. Output $X,Y,Y'$. This has the required edges $X\to Y, X\to Y'$.
3. And it has the required entropy. 

**Q2**
Let $X_{1},\dots,X_n \sim P$ iid and let $Y_{1},\dots,Y_n \sim Q$ iid. 
Let $X_{1}',\dots,X_n'$ be the sorted version of $X_{1},\dots,X_n$. Def $Y'_i$ similarly. 

Then 
$$
D_{KL}(X_{1}',\dots||Y_1',\dots) = \sum_{z_{1}<z_{2}<\cdots<z_n} n!\Pr_X(z_1,\dots) \log \frac{\Pr_X(z_{1},\dots)}{\Pr_Y(z_{1},\dots)} = D_{KL}(X_{1},\dots||Y_1,\dots).
$$
This is because $\Pr_X(z_{1},\dots,z_n)$ is the same for all permutations of $z_i$'s.

Finally, 
$$
D_{KL}(X_{1},\dots,X_n|| Y_{1},\dots) = nD_{KL}(X_1,Y_1)
$$
by writing down the formula for divergence or thinking of divergence as suprise. 

Here's a cute application of this fact: 

**Q2.b** Find $D_{KL}(Bin(n,p)|| Bin(n,q))$.

Observe that $Bin(n,p)$ is basically the same thing as $\mathsf{Sort}(\mathsf{Ber}(p)\otimes n)$
So we have
$$
D_{\mathsf{KL}}(\mathsf{Sort}(\mathsf{Ber}(p)\otimes n), \mathsf{Sort}(\mathsf{Ber}(q)\otimes n)) = nD_{\mathsf{KL}}(p,q)
$$ 
by the problem about sorting. Neat.

**Q3**
For some reason I had trouble finding a nice definition of mutual information so I'm putting it here for all future people so that you can find it easily. It's quite intuitive, it's just the shared randomness amongst all three rvs. 
![[Pasted image 20241130142553.png]]
Anyways, on to the question.

**Q3a**
Suppose $(A,B,C)\sim \mathcal{N}(0,\Sigma)$.
Show that if $I(A;C)=0$ and $I(B;C)=0$  then $I(A,B;C)=0$. 

(Note the difference between commas and semi-colons.)

For this problem I thought it'd be helpful maybe to know what the entropy in a Gaussian is. 
You can do some integral to find it -- I just googled it. 
$$
H(\mathcal{N}(0,\Sigma)) = \frac{d}{2}(1+\log (2\pi)) + \frac{1}{2}\log \det \Sigma
$$
where $d$ is the dimension. Morally speaking, it's just the determinant of the covariance matrix plus some constant term depending on dimension.

---

ok that didn't directly help me I guess. 
anyways, I had a long detour about normal random variables. 
Apparently if $(X,Y)$ are jointly normal, meaning that they are of the form $(X,Y) = M\cdot Z$ for some matrix $M$ and independent normal rvs $Z$, then if $Cov(X,Y)=0$ it follows that $X\perp Y$ .
I had a really hard time believing this because of the following example:

$$
X = N_{1}+N_{2}, Y = N_{1}-N_{2}.
$$
Where $N_{1},N_{2}$ are indep standard normals. 
It seems pretty counter-intuitive to me that $X,Y$ are independent. 
But if you write down their joint distribution it factors so what are you going to do.

Another thing I realized is that it's hard to sqrt the covariance matrix. not literally hard, I think there's a nice algo for it. but just hard for me. 

---

Here's another way of thinking about it. $I(A;C)=I(B;C)=0$ means that $(A,C)$ are independent and $(B,C)$ are independent. This implies that the covariance matrix between $(A,C)$ is diagonal, and similarly for the covariance matrix between $(B,C)$. 

---
Anyways I kind of give up on this problem for now. 

**Q3b**
The most classic question ever -- give example of $A,B,C$ where $A\perp C, B\perp C$ but $(A,B)\not\perp C$.
My example is just $A=X, B = Z\oplus X, C = Z$ where $X,Z$  are independent random bits. 

**Q3c**
Give an example where $A\perp C, B\perp C$  but $(A,B)\not\perp C$ and also $\Pr(a,b,c)>0$ for all $a,b,c$ (we're in discrete land).
um ok
My example is just $A=X, B = Z\oplus X, C = Z\oplus H$ where $X,Z$  are independent random bits, where $H$ is $\mathsf{Ber}(.0001)$. Fine.


----
# some more information theory

Here's an interesting question. I've done the computation but don't get what it means yet. 

Suppose you have a noisy channel.
Specifically it works like this: 

- $1\to 1$ always.
- $0$ goes to a random output.

Q: What is the capacity of this channel. I.e., how much information can you send in this channel?

It turns out that the right input distribution is as follows:

$X = \mathsf{Ber}(3/5)$. This makes $Y$, the channel output, be $\mathsf{Ber}(4/5)$.
This distribution achieves $I(X;Y) \approx .32$.

A curious fact about this choice of $X$:
This choice of $X$ results in 
$$
D_{KL}(P_{Y\mid X=0}||P_Y) =  D_{KL}(P_{Y\mid X=1}||P_Y) = I(X;Y).
$$
Question: explain why this needs to be the case. Like:
1. Why does the best $X$ balance these expressions
2. Why is the mutual info captured by this KL divergence?

okay actually there's a pretty simple explanation for all of this: 
First of all, 
$$
I(X;Y) = D_{\mathsf{KL}}(P_{X,Y}||P_X\otimes P_Y)
$$
(you can see this identity by just writing out the definition of KL divergence). 
Anyways, this implies, by splitting up a sum, 
$$
I(X;Y) = \sum_x \Pr(X=x) \cdot  D_{KL}(P_{Y|X=x}||P_Y)
$$
So at this point we at least understand why, if the $KL$-divergences were balanced, then the MI would be equal to each of the KL divergences. 

I still don't have a great understanding of why we'd want the surprises to be the same...


---
digression
TV distance:
1. $\sup_A |P(A)-Q(A)|$
2. $\frac{1}{2}\sum_x |P(x)-Q(x)|$
3. $\inf_{\mathsf{couplings}}\Pr[P\neq Q]$
----

