Note: 
These pages are some thoughts about [fun math problem](https://arxiv.org/abs/2405.11986) invented by Bill
Kuszmaul. At some point, once we have some results that tell a
nice story, we should publish this stuff. If you find this
problem interesting and want to work on these problems please
reach out to me at alekw dot mit dot edu. See [Alek Westover](https://awestover.github.io) for more about me. Many of
these ideas were thought of in collaboration with
[Nathan](https://nathan-sheffield.github.io/jekyll/update/2024/02/24/lost-in-space-ii.html).
Conversations with Ruth, Tara Westover and Brandon Westover were
also quite helpful. 

---
Key: *"\*" = improving this is a big open question*
### awake time:

| model                                              | lower bound                                                          | upper bound                                        | link                                                                             |
| -------------------------------------------------- | -------------------------------------------------------------------- | -------------------------------------------------- | -------------------------------------------------------------------------------- |
| vanilla                                            |                                                                      | 2*                                                 | [[batch]]                                                                        |
| vanilla, <br>width zero                            | $\phi$*<br>got $\sqrt{3}$ lower bound <br>against original $\phi$-gr | 1.84 -- broken, <br>$\sqrt{3}$ -- even more broken | [[phi-gr-width-zero]]<br>[[crazy-lower-bound]]                                   |
|                                                    |                                                                      |                                                    |                                                                                  |
| cancelling                                         |                                                                      |                                                    |                                                                                  |
| **cancelling, <br>width zero**                     | $1.5$                                                                | $\phi$*                                            | [[phi-cancel-width-zero]], <br>[[cancel-lower-bound]]<br>[[1.5-cancel-thoughts]] |
| cancelling,<br>randomized                          | $1.25$                                                               |                                                    | [[random]]                                                                       |
|                                                    |                                                                      |                                                    |                                                                                  |
| **decide on arrival**                              | 2                                                                    | 2                                                  | [[doa-2]]                                                                        |
| doa, randomzied                                    | $5/3$ (?)                                                            | 2                                                  | [[doa-randomized]]                                                               |
|                                                    |                                                                      |                                                    |                                                                                  |
| parallel work oblivious,<br>single arrival time    | 2                                                                    | 2                                                  | [[parallel-work-obliv]]                                                          |
| parallel work oblivious                            |                                                                      | 4                                                  | (batch above result)                                                             |
|                                                    |                                                                      |                                                    |                                                                                  |
| restrict to tasks with<br> $\pi/p < \lambda\sigma$ | $\frac{1+\sqrt{3}}{2}$                                               | $2$                                                | [[bounded-parallelism]]                                                          |
| decide on arrival and<br>parallel work oblivious   | $\Omega(\sqrt{p})$                                                   | $O(\sqrt{p})$                                      | [[doa_pwo_awake]]                                                                |

>[!info] Remark
>A general lemma is that you can just think about completion time of TAPs with no OPT-gaps and we can boost this to an awake time result. The reason is if you're competitive on each individual TAP then smashing them together can only be good for you. I'm not saying you can use the same strategy that you otherwise would, you should probably pause the first TAP and finish it later. But hopefully this is clear.

### MRT:

| model                                   | lower bound       | upper bound |
| --------------------------------------- | ----------------- | ----------- |
| O(1) speed                              | ??                | O(1)        |
| no speed                                | ??                |             |
| decide on arrival                       | ??                |             |
| parallel work oblivious with O(1) speed | $\Omega(p^{1/4})$ |             |
| non-preemptive                          | $\infty$          |             |
### Misc:

| model                            | lower bound                 | upper bound                                                                                                                                                     | link              |
| -------------------------------- | --------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------- |
| dependencies online              | $\Omega(\sqrt{p})$          | $O(\sqrt{p})$                                                                                                                                                   |                   |
| awake time offline               | Maybe hard?                 | PTAS                                                                                                                                                            | [[offline]]       |
| awake time offline, width zero   |                             | $\widetilde{O}(n)$ time exact algorithm                                                                                                                         |                   |
|                                  |                             |                                                                                                                                                                 |                   |
| dependencies offline, width zero | exact: <br>strongly NP-hard | Polynomial time $1+\varepsilon$ <br>approximation for DTAPs with<br>constant sized connected components, or DTAPs which consist of a constant number of chains. | [[dtaps-offline]] |
