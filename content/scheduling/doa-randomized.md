
- N had a crazy idea for a more aggressive greedy algorithm that tries to do it in expectation
- But now N is more in the 2 lower bound camp. Maybe?
- I think both N and I don't have super strong opinions on this. 
- Except for the opinion: lower bounds are going to be way more
    tractable to prove, so let's try some of those first. 
- To prove lower bounds against randomized algorithms, the most promissing technique is to use [Yao's principle](https://en.wikipedia.org/wiki/Yao%27s_principle). 

```python
# here is the code that shows a 5/3 lower bound
import itertools

# we fix a randomized input sequence of tasks
# and show that any deterministic scheduler has large expected
# competitive ratio on this input sequence. 

# N is the maximum number of tasks in our random input sequence.
for N in range(5, 10):
    # The input sequence is very simple:
    # Choose a random number M from 0 to N-1
    # have tasks with (serial work 2^i, parallel work 2^{i+1}) 
    # for i = 0, ..., M

    # TODO: this is the main part you would want to update:  
    # You want to try new random task sequences, not just this one that I did.
   
    # this is the list of all possible deterministic strategies
    # 0011 means serial serial parallel parallel
    strategies = list(itertools.product(*[[0, 1] for i in range(N-1)], [1]))

    # we look for the strategy with the lowest 
    # average score on the random input sequence
    lowest_avgCR = 2
    who = (0,0,0,0,0)
    why = (0,0,0,0,0)
    for strat in strategies:
        CRs = []
        # compute the competitive ratio on each instantiation of
        # the randomness of the random input
        for i in range(N):
            COPT = 2**i
            CUS = 0
            for j in range(i+1):
                if strat[j]:
                    CUS += 2**j
            for j in range(i+1):
                if not strat[j]:
                    CUS = max(CUS, 2**(j+1))
            CRs += [CUS/COPT]
        # this is the average competitive ratio of strategy strat
        # on the random input sequence
        avgCR = sum(CRs)/N

        if avgCR < lowest_avgCR:
            lowest_avgCR = avgCR
            who = strat
            why = CRs[:]

    print(lowest_avgCR, "\n", who, "\n", why)
    print("\n\n")
```


How did I get this $5/3$ figure?
Well, based on some small examples it seems like the best strategy for handling this TAP is:
- parallel, serial, parallel, serial, ...
If you do this strategy your competitive ratios are:
$$1, 2, 1.25, 2, 1.3125, 2, \ldots$$
And basically after a second it looks like 
$$4/3, 2, 4/3, 2, \ldots.$$
So that'd be $5/3$. 
But, I haven't actually formally proved that this is the best strategy yet. 

