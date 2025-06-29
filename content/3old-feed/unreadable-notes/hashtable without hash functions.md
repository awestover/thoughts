https://arxiv.org/abs/2209.06038

So, first, what's the problem?

Universe $U$ of size $n^{d}\le n^{O(1)}$.
You get an $n$-element subset
You want to be able to do queries and inserts.

First, there's this pretty silly way of implementing a hash table:
A radix tree with radix $n$. 

Pros:
- deterministic
- really fast
Cons:
- quadratic space RIP

Ok, so then Bill does something pretty awesome. 
He defines this thing called "rotated radix trie"
You get it by taking the radix tree thing and then super imposing rotated copies of everything all at once. 

Like the idea is the radix tree had a bunch of super sparse arrays so let's just put them all on top of one another.

You'd like to say that there are not too many collisions.

If so, then there is this thing called fusion node that can win. 

Turns out maybe there are some collisions. 

So for the overflowing guys you might do something else. 

but in general this is pretty good.