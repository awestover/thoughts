This is a place to post random tiny things that I thought were funny, or insightful whatever. Think of it as my twitter account. 

---

- cursor is so good
- I'm a bit frustrated with this RL project --- but I think it's good for me to do --- it will give me better intuition for how ML stuff works. this is going to be extremely valuable.
- memory allocation is a big deal -- like it can make 100x speed impact on your code if you are reallocating a bunch of memory all the time. 
- NEVER spend time trying to optimize code that you haven't profiled
	- You might have some intuition about what parts are bottlenecking 
	- But you could be totally wrong
	- it's as simple as 
```python 
import cProfile
cProfile.run('main()', 'profile')
import pstats
p = pstats.Stats('profile')
p.sort_stats('cumulative').print_stats(10)
```

![[Pasted image 20250303090650.png]]
When your loss is a frowny face :'(

- don't maximize for peak hardware utilization in humans 

- re-frame "i suck at coding" as "oh-no exploding gradients encountered in alek's coding abilities"


