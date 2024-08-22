Suppose you have an array. MAX allowed size is $N$. 
Current size is $n$. You will do inserts and deletes. 
Then, we can handle this with constant update costs and space $n+O(\sqrt{ N })$ .

How?
Size $\sqrt{ N }$ "slabs". 