> Fix $n$. Let $X\subset [n^{5}]$ have size $n$. Consider the following problem: There is a circular race track of circumference $1$. $n$ people, with (distinct) speeds $X$ start at the start of the race track, and run around at their speed, until a random time $a\sim [0,1]$. After this, the runners are at locations $aX \bmod 1$. I then cut the race track into $n$ contiguous chunks, each of length $\frac{1}{n}$ and count how many runners are in each chunk. Let $M$, the "**maxload**", denote the number of runners in the fullest chunk. This is a random variable depending on the time when I stop the runners. In this document I will analyze $\mathbb{E}[M]$.

![[Screenshot 2025-09-27 at 4.33.21 PM.png]]

$\newcommand{\Z}{\mathbb{Z}}$ $\newcommand{\E}{\mathbb{E}}$  $\newcommand{\lcm}{\text{lcm}}$
## Relation to prior work 
**TFAE:**
- $\bmod 1, a\sim [0,1]$
- $\bmod n^{100}, a\sim [0,n^{100}]$ (by rescaling distances).
- $\bmod n^{100}, a\sim [n^{100}]$ ($n^{100} \gg x$ for all $x\in X$, so $ax,(a+1)x$ are very unlikely to be in different buckets). 

In other words, the problem considered in this post, which I'll call "**real hashing**" is equivalent to a special case of the linear hashing problem. I think that real hashing is basically just as interesting as the normal linear hashing problem, and am quite happy to study it.

Because real hashing is a special case of the normal linear hashing problem, Knudsen's argument [here](https://arxiv.org/abs/1706.02783) shows that $\E[M] < n^{1/3+o(1)}$. Getting stronger bounds on $\E[M]$ is my favorite  open math problem. Ultimately, I believe a bound of $\log^{O(1)}n$. In this document, I'll endeavor to get something better than $n^{1/3}$ for real hashing.
## Defining triple collisions
Define $C_{x,y,z}$ to be the probability that $x,y,z$ all end up in the same bin.

Observe:
- $C_{x,y,z} \approx C_{0,x-z,y-z}$.
- $C_{0,kx,ky} = C_{0,x,y}$

In what follows, I'll write $\varepsilon$ instead of $\frac{1}{n}$ to denote the bin size for notational convenience and clarity.
## The triple collision lemma 
**Lemma**
Let $x\perp y$. Then, 
$$
C_{0,x,y} \le 4\left( \varepsilon +\frac{1}{x} \right)\left( \varepsilon +\frac{1}{y} \right).
$$
**Proof:**
By rescaling the length of the circle we have:
$$
C_{0,x,y} = \Pr_{a \sim [0,xy]}\left[a\in y\Z \pm y\varepsilon \cap x\Z \pm x \varepsilon \right].
$$
Integers are more favorable than arbitrary reals, so we can switch to taking $a$ an integer:
$$
C_{0,x,y} \le \Pr_{a\sim[xy]} [a\in y\Z \pm y\varepsilon \cap x\Z \pm x\varepsilon].
$$
Now, here's a helpful fact from number theory:
> The equation $a\equiv \eta \mod y, a\equiv \beta \mod x$  has a unique solution in $[xy]$.

There are $2\lfloor y\varepsilon \rfloor+1$ relevant equations $\bmod y$ and $2 \lfloor x\varepsilon \rfloor+1$ relevant equations $\bmod x$. Overall we get:
$$
C_{0,x,y} \le 4\frac{(1+y\varepsilon)(1+x\varepsilon)}{xy}.
$$
$\square$

**Remark:**
Intuitively, you'd have guessed $C_{0,x,y} \approx \varepsilon^{2}$. However, this isn't always true. For instance, $C_{0,2,3} \approx \varepsilon$.
## The "all numbers are sufficiently coprime" lemma
**Lemma:**
$$
\sum_{x,y \in X} C_{0,x,y} \le (n\varepsilon)^{2} + n^{1+o(1)} \varepsilon + \log ^{O(1)}n.
$$
**Proof:**
By the triple collision lemma:
$$
\sum_{x,y\in X} C_{0,x,y} \le \sum_{x,y\in X} \varepsilon^{2}+\frac{\gcd(x,y)\varepsilon}{x}+\frac{\gcd(x,y)\varepsilon}{y}+\frac{\gcd(x,y)^{2}}{xy}. 
$$
The $\varepsilon^{2}$ term gives us $(n\varepsilon)^{2}$ .

We'll now analyze the second term.

I think you can do better than the analysis that I'm about to do, using a technique from [Alek's paper on LH](https://arxiv.org/pdf/2307.13016). But the below method suffices for our needs. 

Partition $X$ dyadically into sets $X_{1},X_{2},\dots,X_{O(\log n)}$.

We have:
$$
\sum_{x,y \in X} \frac{\gcd(x,y)}{x} \le O(\log ^{2}n) \max_{i,j}\sum_{x\in X_i,y\in X_j} \frac{\gcd(x,y)}{x}
$$
Fix any $X_i,X_j$. 
Let $\hat{x}=\min(X_i)$, $\hat{y}=\min(X_j)$.
Because we dyadically partitioned things, $X_i \subseteq [\hat{x}, 2\hat{x}]$, $X_j \subseteq [\hat{y},2\hat{y}]$.

We have:
$$
\sum_{x \in X_i, y \in X_j} \frac{\gcd(x,y)}{x} \le \frac{1}{\hat{x}} \sum_{y\in X_j} |\{d: d\mid y \land d\le 2\hat{x}\}| \cdot \max_{d\mid y, d\le 2\hat{x}}\left(d \cdot |\{x \in X_i:\gcd(x,y)=d \}|\right) = \star.
$$

There are at most $\hat{x} / d$ many $x\in X_i$ with $d\mid x$. So $|X_i \cap d\Z| \le \frac{\hat{x}}{d}$.
Also, trivially $|X_j|\le n$.
Recall the fact that for any number $k$, the number of $k$ is at most $k^{o(1)}$.

Thus, 
$$
\star \le \frac{1}{\hat{x}} \sum_{y \in X_j} n^{o(1)} \max_{d\in[2\hat{x}]}(d\cdot |X_i \cap d\Z|) \le \frac{n^{1+o(1)}}{\hat{x}}\max_{d\in[2\hat{x}]}\left( d\cdot \frac{8\hat{x}}{d} \right).
$$
Thus, 
$$
\star \le n^{1+o(1)},
$$
as desired.

Overall, this means that we've bounded the second and third terms of the big sum that we care about for the lemma by $\varepsilon n^{1+o(1)}$.

To finish the lemma we bound the fourth term.

We use a similar set of tricks to the ones that we used in bounding the second term. We dyadically partition in the same way. The fourth term is then at most
$$
O(\log ^{2} n)\cdot 
\max_{i,j} \sum_{x \in X_i, y \in X_j}\frac{\gcd(x,y)^{2}}{xy}.
$$
Fix any $i,j$, and define $\hat{x}=\min(X_i),\hat{y}=\min(X_j)$. Then,
$$
\sum_{x \in X_i, y \in X_j}\frac{\gcd(x,y)^{2}}{xy} \le \frac{1}{\hat{x}\hat{y}}\sum_{y\in X_j}\sum_{d\mid y} |\{x: (x,y)=d\}| d^{2}.
$$
This last expression is bounded by
$$
\frac{1}{\hat{x}\hat{y}} \sum_{y \in X_j} \sum_{d\mid y} 2\frac{\hat{x}}{d}d^{2} \le \frac{1}{\hat{y}}\sum_{y\in X_j}\sum_{d\mid y}d.
$$
A famous result in number theory is that 
$$
\sum_{d\mid k} d \le O(k\log \log k).
$$
This is obviously way overkill, but anyways, applying this we bound the expression that we're working on by
$$
\frac{1}{\hat{y}}\sum_{y\in X_j} O(\hat{y} \log \log \hat{y}) \le O(\log\log n).
$$
And, we're done.
$\square$
## A simple bound on maxload
**Theorem**:
$$
\E[M]\le n^{1/3+o(1)}.
$$
**Proof**
Clearly, $\binom{M}{3}$ is a lower bound on the total number of triple collisions.
By Jensen's inequality we have:
$$
\E[M] \le \left(\sum_{x,y,z} C_{x,y,z}\right)^{1/3}.
$$
Recalling that we are basically ok to replace $C_{x,y,z}$ with $C_{0,y-x,z-x}$ we get:
$$
\E[M] \le \left(\sum_{x} \sum_{y,z}  C_{0,y-x,z-x} \right)^{1/3}.
$$

Applying the "all numbers are sufficiently coprime" lemma and setting $\varepsilon=\frac{1}{n}$ we get: 
$$
\E[M] \le \left( \sum_x  n^{o(1)}\right)^{1/3} \le n^{1/3+o(1)}.
$$
$\square$
## Author's note
I'm pretty sure that the above proof goes through, using the "quantum dinosaur" technique for  normal linear hashing of the problem. But it's probably at least a moderate amount of additional work. (The quantum dinos technique is just expertly using the fact that any $x\in \mathbb{F}_p$ can be expressed as $\sigma m^{-1}k$ where $m\in [n], k\in [p / n], \sigma \in \pm 1$).
## The path forward
Ultimately, we'd like to prove some stronger bounds on $\E[M]$. It might be easier to instead prove sth like $\Pr[M > n^{.251}]< .01$, and it's roughly equally cool, so I'll focus on proving this slightly weaker statement.

Here's the rough plan for how to prove this stronger bound:
1. Eat $\frac{1}{n}$ of the times around each fraction with denominator at most $\sqrt{ n }$.
2. Figure out the quadruple collision lemma, and do some vibe checks on it. It's going to be much trickier than I originally thought. It's going to need to take into account that we ate some stuff.
3. Show that the expected number of quadruple collisions, **over times that were not eaten** is at most $n^{.251}$, by using the quadruple collision lemma and basic number theory.

## Trying to prove a quadruple collision lemma
Let's assume --- bc idk how to solve even given this assumption, and simpler problems are nice --- that $X$ is a set of prime numbers and $X \subseteq [n^{1.01}]$.

Let $\mathcal{A}$ be the set of $a\in [0,1]$ such that $a$ is at least $\frac{1}{n}$ away from any fraction with denominator smaller than $\sqrt{ n }$. We aim to bound:

$$
C_{0,x,y,z} := \Pr_{a\sim \mathcal{A}} a \in \frac{\Z \pm \varepsilon}{x} \cap \frac{\Z \pm \varepsilon}{y} \cap \frac{\Z \pm \varepsilon}{z}.
$$

Naively, you'd hope that $C_{0,x,y,z} \le \varepsilon^{3}$. If you're a bit more skeptical, you'll notice that $C_{0,1,2,3}$ is quite large, and you might instead conjecture sth more like

$$
C_{0,x,y,z} \le \frac{\varepsilon x+1}{x} \frac{\varepsilon y+1}{y} \frac{\varepsilon z+1}{z}.
$$
Honestly, idk if I even expect this thing to be true.

Whatever happens, it's all going to crucially depend on the fact that we're taking $a \sim \mathcal{A}$.

Note that the weaker conjecture with all the $+1$ stuff going on would be sufficient to cary the day, given our assumption that $X$ consists solely of primes (and it'd probably be fine regardless, but no point taking chances).

---

It feels very productive to write 

$$
a \equiv \chi+x \beta\mod xy
$$
$$
a \equiv \chi+x \eta \mod xz
$$
for some $\beta<y/n, \eta < z / n, \chi \in [x]$.
Then,
$$
a-\chi\mod yz \equiv x(\beta z \frac{yk+1}{z}+\eta y \frac{z \ell+1}{z}) =: f(\beta,\eta).
$$
I'd like to argue that in the square $(\beta,\eta) \in [y/ n] \times [z / n]$, there are at most $\frac{yz}{n^{3}}$ many  $\beta,\eta$ points such that 
$$
f(\beta,\eta) < \frac{yz}{n} \bigwedge f(\beta,\eta) \notin xyz\cdot \mathcal{A}.
$$
This would be enough to win.

Note that banning $xyz\mathcal{A}$ is certainly going to be necessary, and it's not clear that it's going to suffice.

> The remainder of the proof is left as an exercise to the reader. 

By which I mean, I'll think about this more next weekend probably. 

tag:math