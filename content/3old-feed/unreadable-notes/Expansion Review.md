Credit to:
- Dor Minzer's lecture notes
- [Max Hopkins lecture notes](https://www.wisdom.weizmann.ac.il/~dinuri/courses/22-HDX/L13.pdf)
- O'Donnel's book

---

Edge expansion of a weighted graph is defined  as:
$$
\Phi(S) := \Pr_{x\in S, y\sim_w N(x)}[y\not\in S].
$$
We say a graph is an expander if, for all $|S|<|V|/2$ we have $\Phi(S)\ge c$. for some large $c$.


**Small set expansion**:
For any $S$ with $|S|<\delta n$, we have $\Phi(S)>1-\epsilon$.

**Theorem 2.3 (The hypercontractive inequality).** 
For all $f: \{\pm 1\} ^n \to \R$, if $1\le p\le q$ and $0\le \rho  \le \sqrt{(p-1)/(q-1)}$, then 
$$
||T_\rho f||_q \le ||f||_p.
$$

**Theorem:**
The $\rho$-noisy hypercube is a small set expander. 

**Proof**:
The key step will be to use the hypercontractive inequality.
This is because we can express the expansion by the following inner-product:
$$
\langle 1_S, T_\rho 1_S \rangle = \mu(S)\overline{\Phi(S)}.
$$
Now using Holder and Hypercontractivity: 
$$
\langle 1_S, T_\rho 1_S \rangle \le ||1_S||_{\rho + 1}||T_\rho 1_S||_{1+1/\rho} \le ||1_S||_{\rho+1}^2 \le \mu(S)^{\frac{2}{\rho+1}}.
$$
We conclude that:
$$
\overline{\Phi(S)}  \le \mu(S)^{\frac{1-\rho}{1+\rho}}.
$$


---

Some other good things to remember:
- concentration, anti-concentration for low degree functions
- Junta's are the classic example of non-expanding sets in the hypercube. 
- Hypercontractivity: says stuff about the noise operator, and about relationships between different norms
- Small influence --> you're a Junta (Freidgut Junta theorem)
- KKL gives you a coordinate with high influence
- Expansion could be pretty good. It's best if you get a graph that people already know about, or a Cayley graph. But in a pinch $\lambda_{2} = \max_{x\perp 1} |Ax|/|x|$  might be useful. 
