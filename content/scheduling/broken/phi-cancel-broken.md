# oops this is broken!!!
[[scheduling]]
Similar analysis feels relevant for [[phi-gr-kinda-broken]]; of course width zero at the moment.

Throughout the document we assume 
- *width zero*
- no OPT gaps

**Lemma**: Fix $i<n$. Suppose that $\opt$ runs $\tau_i$ in serial. Then so does $\phic$.
**proof**: Assume for contradiction that $\phic$ doesn't run $\tau_i$ in serial (ever).
Then,
**Oops, this line is bogus:**
$$\C_{\opt} \ge \sigma_{i}+t_{i} > (\phi+\eps) t_n$$
$$\C_{\opt} \ge \sigma_{i}+t_{i} > (\phi+\eps) \C_\opt-t_n$$
$$(1/\phi+\eps) \C_\opt < t_n < \C_\opt/(\phi+\eps).$$
Contradiction.

**Theorem**: $\phic$ is $\phi$-competitive.
**proof**: 
Assume for contradiction that there is some TAP on which $\phic$ has competitive ratio at least $1.62$.  Let $T$ be such a TAP of minimum length. 

Assume for contradiction that $\opt$ serializes some $\tau_{i}$ in $T$ with $i<n$. 
Then by the Lemma, so does $\phic$. 
Let $T'$ be the TAP obtained by removing $\tau_{i}$ from $T$. 
We claim that $T'$ is at least as hard as $T$ --- i.e., $\phic$ has competitive ratio $>1.62$  on $T'$ as well. Indeed, this is clear. 
Thus, it is not actually possible for $\opt$ to serialize $\tau_{i}$.

We have therefore shown that in TAP $T$, $\opt$ must parallelize all tasks. 
But then $\phic$ has competitive ratio $1$ on $T$, contradicting the assumption that $\phic$ has competitive ratio at least $1.62$ on $T$.