**Theorem**: $s$ space and $t$ time with $s t< n^{2}$ does not suffice to sort a list.

**Simpler theorem**:
$n^{.5}$ space and $n^{1.4}$ time does not suffice to output "which of the numbers between $1$ and $n$ appear exactly once in my list".

Exercise: Why does the simpler theorem basically imply the main theorem?

**Pf of simpler theorem:**

We can think of a list sorting algorithm as a "branching program"

![[Screenshot 2026-08-09 at 3.55.52 PM.png]]

Each state is a configuration that your memory could be in. At each time step, you're at some memory state and then you read an input bit and transition to one of two memory states on the next time step. 

We say that a vertex $v$ in the branching program is good for list $L$ if: 

> Starting from $v$ and running the branching program on $L$ for $\frac{n}{100}$ steps (i.e., following the arrows that $L$ dictates) results in outputting at least $n^{.59}$ distinct unique values in $L$, and no non-unique values.

**Define** $\mathcal{D}$ to be the following distribution over lists: 
Each element of $L$ is a random element of $\{1,\dots,n\}$.

**Lemma:** 
Fix vertex $v$. $\Pr_{L\sim \mathcal{D}}[$ $v$ is $L$ good$]\le.99^{n^{.59}}$.

**Proof**:
You're only looking at $1\%$ of the list, so you really have no basis for any of your guesses that elements are unique. Each time you guess that an element is unique, there's at least a 1% chance (probably really like 30%) that you're wrong, and these are all independent.

Split the branching program into length $\frac{n}{100}$ chunks. Let $X$ be the vertices at the first time slice of each chunk.

**Lemma:** For most $L\sim \mathcal{D}$, all vertices in $X$ are $L$ bad.

**Proof:** $X$ contains $2^{n^{.5}}\cdot 100n^{.4}$ vertices. Each vertex is $L$ good with probability at most $.99^{n^{.59}}$. So the chance that any vertex in $X$ is good is at most $2^{n^{.5}}\cdot 100n^{.4}\cdot .99^{n^{.59}}$, which is tiny (for large $n$, which is what I care about).

**Lemma:**
Fix a list $L$ with at least $\frac{n}{100}$ unique elements. Suppose that all vertices in $X$ are bad for $L$.
Then, the branching program doesn't correctly output the unique elements of $L$.

**Proof**: If all vertices in $X$ are $L$ bad, then we output at most $100n^{.4}\cdot n^{.59}=100n^{.99}$ values. This isn't enough.

**Lemma:** Most $L\sim \mathcal{D}$ have at least $\frac{n}{100}$ unique elements.

**Conclusion:**
Combining all the Lemmas, we find that most $L$ have a bunch of unique elements, but the branching program doesn't output them all. That is, the branching program usually outputs the wrong answer. So, we didn't have enough space / time.

qed.

tag:math