[Paul stated some interesting graph theory questions here](https://sideways-view.com/2019/08/25/some-open-problems-in-p2p-routing/) that he suspected were open.

Basic setup:
- Randomly  choose $X\subseteq\{0,1\}^{n}$ of size $1.1^{n}$.
- Connect each person $x$ to $n^{2}$ random people.

Easy mode:
two functions
- LISTEN(ID) -- pop the last message sent from person ID from the queue.
- SEND(ID, msg) -- send a message to someone 

Hard mode 1:
- You can only use these functions within the network topology

Hard mode 2:
- Some people are evil :O
- maybe need to be a bit careful to define this...

Hard mode 3:
- hard mode 1 + 2

Problems:
1. Choose two people $(s,t)$ randomly. $s$ must send a message to $t$.
2. Choose $n$ pairs of people $(s_{i},t_{i})$, all the $s_i$'s must send message to $t_i$'s.

---

Some thoughts:

## Easy mode

**Problem 1**-- it seems like the following thing just works: 
- just broadcast-anate the message throughout the entire graph :)

**Problem 2** -- Paul described a protocol basically.
- todo write it down and analyze it


## Hard mode 1

**Problem 1**,  broadcasting message to whole graph still works

**Problem 2**

Let $N = |X|$.
We had some method for getting $\sqrt{ N } \cdot \mathsf{polylog}(N)$.

We basically 
- chose a set of $\sqrt{ N }$ special guys. 
- assign everyone canonically to a special guy
- we give each of the special guys a chance to talk to the whole graph -- this just lets us learn paths to them and between them.

- to actually send the messages we just send them our designated special guy, who sends it to recipient's designated special guy, who sends it to recipient

todo flesh out details.

we then tried to improve on this because this is rlly slow.

had some idea of having "supernodes"
didnt flesh out yet.

I think with a little supernoding we can get down to $N^{3/8}$

**edit:** I think the above claim was probably fake
