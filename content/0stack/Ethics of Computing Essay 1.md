Alek Westover
Ethics of Computing Essay 1
Topic: Is Totalism true?
~1400 words

Full Prompt: 
>"Totalism" implies that an AI catastrophe would be far worse than a run-of-the-mill disaster,
like a hurricane leveling most of Florida, or a nuclear power plant melting down. What is
Totalism and why does it imply this? Do you think Totalism is true? If not, explain and defend
an objection to it. If so, explain and respond to an objection.

### Introduction
Totalism is the thesis that the **value** of a -- possibly counterfactual -- universe is a well defined quantity, and that the value of a universe can be determined by summing numbers representing how "good" the lives of the people in that universe are. Many people find totalism to be an attractive "theory of the good" because of its simplicity and elegance. However, some people, such as the philosopher Parfit, claim that totalism gives unintuitive conclusions when comparing the goodness of certain universes. In this note, I will argue that totalism is true. I will do so by first motivating totalism, and then arguing that totalism actually gives the correct answer in supposed counter-examples like Parfit's "Repugnant Conclusion".

### Defining and Motivating Totalism
Totalism asserts that (1) the "value of a universe" is a well defined quantity, and (2) there is a simple "formula" for computing the value of a universe. I will now formally define and motivate these two assertions.

By "a universe", I specifically mean a "possible" trajectory for the evolution of the physical world across time. Here, "possible" should be interpreted as "logically possible" -- that is, it will often be informative to consider hypothetical universes rather than only the specific way that the actual universe has played out. The idea that the universe has a numerical "value" may initially seem strange, as we do not posses a "thermometer" to measure this value. However, the fact that the universe has a value is a simple consequence of our ability to compare possible universes. Suppose that you think our current universe is better than an alternate universe which is the same in all regards, except that it lacks pizza. Then, our universe has higher value than the alternate universe. If one thinks that two universes are equally good, then this corresponds to the universes having identical value. If we accept that the value of any two universes can be compared (and that comparisons are transitive), then there exist real numbers that we can assign to universes that give the same ordering on universes as our preference function. 
For the remainder of the paper I will take as a premise that the value of a universe is a well-defined quantity, and focus on the question of whether totalism is the correct method for determining this value.

Totalism asserts that the value of a universe is the sum of the welfare levels of all people that exist in this universe. The welfare level of a person is a measure of the total quality of their life. For instance, if person X lived in extreme poverty while person Y lived comfortably, then we would say that (ceteris paribus) person Y had a higher welfare level than person X. 
There are various formalizations of welfare (e.g., in healthcare[^1]); for the purposes of this paper, an intuitive notion of welfare will suffice.
I will actually argue for a broader version of totalism, which is inclusive of all conscious experience rather than just human conscious experience. I hope the reader finds it intuitive that if there were "aliens", which were very similar to humans but looked different, then the love, curiosity and excitement (and so on) of these aliens would make the universe a better place. More precisely, I claim that the value of a universe can be expressed as a sum over conscious experiences of a measure of the quality of the conscious experience. In this paper, the human-centric definition of totalism mostly coincides with the more inclusive totalism proposed above; whenever the distinction is important I will explicitly point this out. It is worth noting that, as defined above, totalism only assigns values to "complete" universe trajectories, and thus can only be computed by the time the universe ends. A natural method for extending totalism to assign values to partial universe trajectories is to define this as the expected value of the full universe trajectory.

To further explain totalism, it is productive to consider an example of using totalism to compare two universes.

> World 1: A hurricane levels most of Florida.
> World 2: An AI catastrophe occurs, leaving all humans dead.

Totalism does a good job of explaining why world 2 is much worse than world 1: world 2 is worse because in world 2 all the current living humans, and all the potential future humans end, after which they contribute no welfare to the sum determining the value of the universe.
A potential objection to this simple analysis is "don't the AI contribute value to the universe, potentially much more value than humans?". But this concern can be dismissed by noticing that by default there is no reason to expect that AI would have valuable conscious experiences (such as love, wonder, curiosity).

At this point I have developed a reasonably formal definition of what I mean by totalism, and motivated the idea that the value of a universe is a quantity that exists. How should one go about deciding whether totalism is true? Occam's razor tells us that "the simplest explanation is usually the best". Totalism is arguably the simplest possible way of aggregating welfare into value -- just summing welfares without any special cases. In the rest of the paper I will show that totalism correctly chooses the better universe in some tricky examples. The empirical success and aesthetic cleanness of totalism make a compelling case for totalism as a correct theory of value.

### Evidence for Totalism
Parfit argues that totalism incorrectly compares the value of the following two worlds:

| World 1                           | World 2                              |
| --------------------------------- | ------------------------------------ |
| 100 people, with 100 welfare each | 1 million people with 1 welfare each |
Parfit contends that world 1 with a few very well-off people is better than world 2 with a million people living "barely positive value lives". I think this conclusion is wrong. Below, I give several analogies to make it more intuitive why world 2 is actually much better than world 1.

**Analogy 1: Welfare as time**
All else equal, living longer increases your welfare --- no one would dispute that it is tragic when a young child dies, cutting off their potential for an amazing life.
With this lens, we can rephrase Parfit's scenario as follows:

| World 1                              | World 2                                 |
| ------------------------------------ | --------------------------------------- |
| 100 people, living for 100 days each | 1 million people, living for 1 day each |
Note that when by "living for 1 day each" I refer to one day of conscious experience of an adult from our world. 
Put another way, the world 1 and world 2 listed above should be seen as *completions* of the current world. So the question is, if you had to choose between a future where humanity ends in 1 day, and 1 million people get to experience this final day, or a future where humanity ends in 100 days, but only 100 people get to live in these days, which would you pick?
I think with this framing it becomes clear that world 1 is much worse than world 2.

**Analogy 2: Scarcity**
Suppose that an environment is such that it can either support 1 person at welfare level 100 (world 1) or 2 people at welfare 90 (world 2) -- e.g., imagine the environment has exactly enough food for 1 healthy person, or for 2 half-starved people.
I think it's again intuitive that world 2 is better: if not, then an agent that murdered one of these two people would be "making the world a better place" which seems ludicrous.

If the "Repugnant Conclusion" is false, why do so many people agree with Parfit?
I think this boils down to scope insensitivity -- the emotions of humans are not
well calibrated in situations with big and small numbers.

To conclude, I will pose a comparison that I find more concerning that Parfit's, but then argue that totalism still gives the right answer.
The situation is summarized in the following table:

| World A                   | World B                       |
| ------------------------- | ----------------------------- |
| 1 person with 100 welfare | 2 people with 40 welfare each |
Totalism asserts that world A is better. However, a naive appeal to "fairness" seems to indicate that world B is better -- for instance, is the value of a happy person that actively engages with life larger than that of a depressed person that doesn't engage actively in life?
I think the reason why human intuition leads us astray in comparing world A and world B is that we don't accurately imagine what a 2.5x welfare difference entails. I think a 2.5x welfare gap could be the difference between a comfortable life and a life full of frequent torture. One easy way of making it more intuitive that world A is better is visualizing welfare as life lengths, as discussed earlier.

In this paper I have argued that the simplicity and empirical success of totalism are compelling reasons to believe totalism as a theory of value. To summarize, value is quite simple: quality conscious experiences are what give the universe value.

[^1]: Wouters, Olivier J., Huseyin Naci, and Nilesh J. Samani. "Quality-Adjusted-Life-Years in cost-effectiveness analysis: an overview for cardiologists." Heart 101.23 (2015): 1868-1873.
