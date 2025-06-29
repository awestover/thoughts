Trying to solve AI safety is hard. 

In particular, it's easy to get sucked into one of the following poles: 
- "Alignment by default"
- "Alignment either happens or doesn't, we can't change this"
- "Alignment is impossible"

This feels vaguely reminiscent of HPMOR:TSPE --- pessimism can be a self-fulfilling prophecy, 
whereas motivated reasoning / thinking that things will be fine by default is not smart.

> Q: How to avoid this?

 My current proposal is to have explicit separate modes of discussion. 
 
**Butterfly mode**:
- For proposing very not-fleshed out ideas and not ever saying the word *inefficient* or "the government will never do that!".
- And basically only *adding* ideas, no removing of ideas.
- remark: There's some LessWrong post about "Butterfly Ideas" that captures this notion / proposed algorithm (I think).

**Pruning mode**:
- Once there are a bunch of ideas, (verbally explicitly) switch to pruning mode
- Where the goal is to find some problems with the current approaches / voice concerns.
- (And in the next butterfly mode you add assumptions or whatever to resolve the counterexamples.)

I don't think implementing this approach requires conversation partner to have read this blog post. 
Here's an imagined scenario:

- Alice: I have this fix for AI safety! You do XY, then Z happens and then you do W. 
- Bob: Wow Alice, it's pretty cool that you are thinking about this, the idea sounds interesting, can you tell me more?
- Alice: Well yeah, X could be good and then we'd do Y and eventually T happens!
- Bob: okay, cool. Let me see if I've understood. I think I'm still a bit confused about what X means.
- Alice: yeah me too, but its some vibes at least. 
- Bob: okay sure. I have a few potential concerns. Is this idea at a stage where you're interested in getting feedback / criticism, or is it not really ready for that yet?
- Alice 1: mmm, if it's alright let's schedule a time for that later this week. I want to write this up some more and flesh it out a bit. I don't think it's in a place where it's ready for feedback yet. 
- Alice 2: Sure! I'd be interested in cutting back some less promising approaches and identifying the path forward. 

Of course this kind of bouncing between proving algorithms and articulating barriers is quite standard in TCS. The diff proposed here is a more rigid separation between the two modes, which are normally quite fluid. This motivation for this diff is psychological. The efficacy of this approach has yet to be experimentally studied. 
