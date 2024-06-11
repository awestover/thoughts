
- There's a crazy idea for a more aggressive greedy algorithm that tries to do it in expectation
- N is pretty convinced that you should be able to beat 2

In terms of lower bounds, we are Yau'ing. 
One thing you could try is 
- "randomly choose the length of the TAP in the lower bound that we already have"

```python

# 1.6342 is the record so far
# a calculation indicates that 5/3 is probably the right answer

import itertools

for N in range(5, 10):
    strategies = list(itertools.product(*[[0, 1] for i in range(N-1)], [1]))
    # 0011 means ser ser par par

    lowest_avgCR = 2
    who = (0,0,0,0,0)
    why = (0,0,0,0,0)
    for strat in strategies:
        CRs = []
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
        avgCR = sum(CRs)/N
        #  print(strat, CRs, avgCR)

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
But, I haven't actually proved that this is the best strategy yet. 


#todo: N suggests trying other probability distributions