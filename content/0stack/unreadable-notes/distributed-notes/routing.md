Here's a simple cute routing algorithm:
So, the problem is you are in congested clique model. 
Meaning, at each round you can send $\log n$ bits on each edge. 

And, each vertex has $O(n)$ messages that they want to distribute, and each vertex is the recipient of at most $O(n)$ messages.

Then the question is, how to route?

It turns out that it's possible. I'm going to present something slightly weaker to start at least. 

First, let's suppose that everyone only has like $n/(20\log n)$ messages that they want to distribute. 
Then you could solve this as follows. 

Everyone sends message to $5\log n$ random people.
Each node that gets at most $1$ message passes it along. 

ok, now we can handle a slightly higher number of messages like $n/(20\log\log n)$ as follows:

Send message to $5\log \log n$ random people. 
Then in expectation less than $n/\log n$ people get multiple messages.
You can then inform the senders which messages failed to send. And this smaller number of people can try again.  