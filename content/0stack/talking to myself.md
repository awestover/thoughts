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



# some interesting quotes

- "school taught many of us that we’d be punished for getting things wrong" -- oh gosh that sounds awful

> I imagine them going: “Whoa. Basically all of history, the whole thing, all of everything, almost didn’t happen.” I imagine them thinking about everything they see around them, and everything they know to have happened, across billions of years and galaxies — things somewhat akin, perhaps, to discoveries, adventures, love affairs, friendships, communities, dances, bonfires, ecstasies, epiphanies, beginnings, renewals. They think about the weight of things akin, perhaps, to history books, memorials, funerals, songs. They think of everything they love, and know; everything they and their ancestors have felt and seen and been a part of; everything they hope for from the rest of the future, until the stars burn out, until the story truly ends.
> 
> All of it started there, on earth. All of it was at stake in the mess and immaturity and pain and myopia of the 21st century. That tiny set of some ten billion humans held the whole thing in their hands. And they barely noticed.

“Somebody has to, and no one else will.”


- Coding in vim is so much better. 
- my eyes are so much happier if I use dark mode for everything :) 

- against MYOPIA --- more important to do something than to feel like you're doing things

- fine tuning seems insanely overpowered
