Suppose that we want to sample from an HMM with complicated transition rules, conditional on some observations. (If the transition rules are Gaussian you can use Kalman filters, and if they are discrete then you do classical belief propagation).

Let's say there is a sequence of hidden states $h_t$ and we get a sequence of observation values $v_t$.

There are some simple update rules governing the evolution of our pr dist on $h_t$ ---

$$
p(h_{t+1}\mid v_{1:t + 1}) \propto p(v_{t+1}\mid h_{t+1}) \int_{h_t}p(h_{t+1}\mid h_t) p(h_t\mid v_{1:t})
$$
We're going to maintain $N$ **particles** $h_{t}[1],\dots, h_t[N]$ and **weights** $w_t[i]$ (which sum to $1$) such that 
$$
p(h_t\mid v_{1:t}) \approx \sum_{i=1}^{N} w_t[i] \delta(h_t, h_t[i]).
$$

Given this approximation, we update as 
$$
p(h_{t+1}\mid v_{1:t+1}) \approx \frac{1}{Z} p(v_{t+1}\mid h_{t+1}) \sum_i p(h_{t+1}\mid h_{t}[i]) w_t[i].
$$
Then, ideally we'd choose some new particles $h_{t+1}[i]$ to approximate this distribution and keep going.

In reality it's not totally clear how to choose the new particles --- for instance sampling from the correct distribution might be computationally challenging. 

So we'll instead use importance sampling!

We set $q(h_{t+1}) = \sum_i p(h_{t+1}\mid h_t[i]) w_t[i]$ which we can actually sample from, and then we sample our $h_{t+1}[i]$ 's from this distribution and then we reweight them by
$$
\widetilde{w}_{t+1}[i] = \frac{p(v_{t+1}\mid h_{t+1}[i]) \sum_{j} p(h_{t+1}[i]\mid h_{t}[j]) w_t[j]}{q(h_{t+1}[i])}
$$
except you should normalize the weights to sum to one.