**q1**
see [[low vs high stakes alignment|high stakes alignment notes]] for context.

Let $\mathcal{F}$ be some class of boolean functions on $n$ bit inputs. 
For example "$n^{2}$ node decision trees" or "$n^{2}$ gate circuits".

Let $C$ be some "non black box" "pretty simple" function.

Let $\mathcal{F}'$ be the set of $f\in \mathcal{F}$ such that $\forall x, \neg C(f,x)$.

You get $n^{3}$ training queries.
You wanna find $g\in \mathcal{F}'$ which is competitive in terms of reward with the best $f\in \mathcal{F}'$.

TODO: 
flesh out -- 
how can C both be simple and non-black box.
if its simple and you just know it then the problem trivializes 

why arent we allowed to just distill C

not quite sure how to spell this out. 

also not sure if defining C is actually secretly the main difficulty. 

enough for now.