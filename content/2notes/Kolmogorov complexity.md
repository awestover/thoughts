Let $D_{0}$ denote python.
We say $D_{0}(x)=y$ if running the python interpreter on the program $x$ outputs string $y$.

K-complexity is a neat notion --- it's the length of the shortest python program that prints out your string.

We'll write $K(x)$ for the K-complexity of a string.

Def: $\ell(x) =$ length of $x$, i.e., $\lceil \log x \rceil$.
Def: $x$ is random if $K(x)\ge \ell(x)$.

**Proposition**:
- There are random strings of every length.
- Proof: count.

---

**Cool fact 1:** 
- $x$ is random is a statement which is often true, but rarely provable!

In particular, let $F$ be a formal system, describable in $f$ bits. 
Suppose $F$ is consistent and $F$ proves that some length $n$ string $x$ is random.
Suppose we do a brute force search over proofs. Let $x^{*}$ be the first length $n$ string that we find a proof of randomness for. It must exist because $F$ proves that some string $x$ is random.
But then we have $K(x^{*}) \le f + \log n$. When traditionally $K(x^*)\ge n$ for random $n$. 

"In any consistent logical system, there are true statements that aren't provable". 

---

Ridiculous proof of prime number theorem:

Lemma: 
Let $R$ be the set of prime divisors of random numbers. 
$R$ is infinite.

**Proof**:
Suppose $R$ were finite. Let $\pi(\max(R))=r$.
Suppose we encoded numbers as follows:

We wrote down the index of the largest prime dividing them (using a prefix-free code), and then wrote down how many copies of each prime dividing them there are. 

Then, random numbers of length $n$ could be specified in like $3\log(r) \log \log n$ bits, while they actually need $\log n$ bits.


---

Ok, now we'll actually go all the way.

**Theorem:** 
$$
\pi(n) \ge \frac{n}{\log n (2\log \log n)^{2}}.
$$
**Proof:**
Let $p_{1},p_{2},\dots$ be an ordered list of the primes.

For each $m$, let $n_m$ be the smallest random number whose largest prime divisor is $p_m$; such a random number exists by the lemma.

We can encode $n_m$ in the following silly way:
$$
E(m) || \frac{n}{p_m}
$$
Where $E(m)$ is a prefix free encoding of $m$.

This gives:
$$
\log(n_m) \le K(n_m) \le \ell(E(m)) + \log\left(\frac{n_m}{p_m}\right).
$$
Whence, 
$$
p_m\le \exp(\ell(E(m))) \le \exp(\log m +\log \log m + \log (2\log \log m)^{2}).
$$
Whence, 
$$
p_m \le m\log m (2\log \log m)^{2}.
$$

Letting $x_m = \lceil m\log m (\log \log m)^{2} \rceil$
We have:
$$
\pi(x_m) \ge m \ge \frac{x_m}{\log x_m (2\log \log x_m)^{2}}.
$$

To extend this to the full thing that we actually care about, namely, $\pi(x)\ge \frac{x}{\log x (2\log \log x)^{2}}$ for all $x$,
idk just figure it out.

---


Ok here's another crazy thing.

**Theorem**
Let $T$ be a single tape Turing machine, with a 2way read/write head.
Suppose $T$ decides the language $xx$. Then, $T$ requires time $\Omega(n^{2})$ on almost all inputs.

**Proof**
We assume that $T$ writes its answer in the cell after the "input end" marker.

Suppose that $T$ can handle random inputs in time $o(n^{2})$.

Fix random $n$ bit number $x$.
Try running $T$ on $x 0^{n}0^{n} x$. There should be a location in the middle where if we write down a list of the TMs state each time it crosses that location, it'll only take $\frac{n}{2}$ bits to describe.

We claim that this list, along with the description of $T$ and the input size $n$ gives a description of $x$ which is impossible, the description would be too short. 

Ok but how to reconstruct $x$.

Imagine running the TM on inputs of the form $y 0^{n} 0^{n} x$.
Suppose that the crossing sequence is the same as it was for $x 0^{n} 0^{n} x$.
Then, the TMs answer will be the same. 
So we must have $x=y$. 
Thus if you give the crossing sequence, we can brute force search for $x$.
So this is a small description of $x$.

