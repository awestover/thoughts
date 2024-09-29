In this post I will write an explanation of [some work by ARC and other groups](https://www.alignment.org/blog/formal-verification-heuristic-explanations-and-surprise-accounting/) on provable guarantees for neural networks. My main goal in writing this is to personally understand what they are working on. But hopefully by the end I will have some comments and ideas for future work.

### Background:
- "Compact Proofs of Model Performance via Mechanistic Interpretability"
- "max of k Heuristic Estimator"

These papers consider learning the following function: 
$$f:[n]^{k}\to [n], f(x_{1},\dots,x_k) = \max(x_{1},\dots,x_k).$$
Gabe trains a 1-layer attention-only transformer to do compute this function.
We are interested in finding a heuristic explanation $\pi$ to explain the model's performance (soft accuracy). 

In his writeup Gabe gives a description of what such a transformer looks like:

1. `input` kxn one-hot encoded input
2. res = input * Embedding + Positional encoding 
3. attn_presoft = res * query * key transpose * res transpose
4. attn_prob = masked softmax(attn_presoft)
5. attn = attnprob * res * value matrix * output matrix
6. logits = (res + attn) * U
7. output = softmax(logits across appropriate dim)

So I think the basic idea of Gabe's paper is as follows:
A priori you might think that in order to prove that the model is pretty good at outputting `max` you'd have to just evaluate the model on all possible $n^{k}$ inputs. 

But by using the heuristic of independence in a good place, Gabe is able to cut this cost down to more like $O(n^{3})$ (basically this amounts to doing some MM's and aggregating data in some clever way).

I think the "Compact Proofs of Model Performance via Mechanistic Interpretability" paper maybe is actually able to give some provable guarantees. Like they give an efficient algorithm that has some error bound.

But ARC feels that such provable guarantees aren't really scalabe, whereas they think that the defeasible arguments are more scalable.

### open questions from Gabe
- suprise accounting
- analog of wick propagation
	- not really sure what this means yet -- need to do some more reading.

Jacob talks about surprise accounting in his blog post.
There are two parameters here
- How surprising is the explanation (i.e., how many bits does it take to specify)
- How surprising is the data given the explanation. 

