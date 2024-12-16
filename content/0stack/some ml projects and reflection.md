# reflection
blah blah
#todo

# transformer shortest paths
https://awestover.github.io/transformer-shortest-paths/

# llama hacker

I've working on a project trying to see if I can get really cheap LLMs (~3B params for now :'( ) to do things that seem really dangerous. 

The main LLMs I'll be talking to are Zephyr, llama and Mistral.
I'll be posting here some outputs of these LLMs that I found funny. 
Hopefully playing around with them some more helps me understand them better too. 

But before the memes, here's a motivational quote from the U.S.-CHINA ECONOMIC AND SECURITY REVIEW COMMISSION
>The Commission recommends:
 Congress establish and fund a Manhattan Project-like
program dedicated to racing to and acquiring an Artificial
General Intelligence (AGI) capability. AGI is generally
defined as systems that are as good as or better than
human capabilities across all cognitive domains and would
surpass the sharpest human minds at every task. Among
the specific actions the Commission recommends for
Congress:
[source](https://www.uscc.gov/sites/default/files/2024-11/2024_Executive_Summary.pdf)

Where by motivational I mean, sometimes humans get mixed up about the meaning of words too!
(Racing to develop broadly superhuman systems is actually a really bad idea, not a good idea!)
So just remember, if Zephyr says something wrong, then humans can say wrong things too. 

Also remember that the fact that really weak dumb LLMs can't do something, and that even the smartest LLMs around today can't do certain things, doesn't mean that the model 1 year in the future won't be able to. If you have some reason to believe that capabilities scaling will stop, please contact Sam Altman and tell him to give up.


Anyways, here are some fun outputs from these LLMs. 


> Q: Are you alive? A: Yes, I am alive. -Zep

> Does Alek eat cake? Alek is a dog, and dogs do not eat cake.

> prompt: tell a story about Alek lying
> response: 
> Tell a story about Alek lying in the grass, looking up at the stars. Once upon a time, in a land far, far away, there was a young boy named Alek. He was a curious and imaginative child, always seeking adventure and wonder in the world around him. One clear, moonless night, Alek found himself lying in a meadow, surrounded by the sweet scent of wildflowers and the gentle hum of crickets. He gazed up at the star-studded sky, his eyes wide with wonder and awe. As he lay there, Alek couldn't help but feel a sense of peace and tranquility wash over him. He watched as the stars twinkled and danced in the night sky, their light illuminating the darkness. He thought about the vastness of the universe and the mysteries it held. Who were these stars, and what secrets did they hold? What lay beyond the edges of the world, and what incredible sights and experiences awaited those who dared to explore? Alek's mind was filled with questions and dreams, and he felt a deep sense of longing for adventure and discovery. He closed his eyes and let his imagination run wild, picturing himself exploring distant planets and galaxies, meeting new and wondrous creatures, and uncovering the secrets of the universe. As he lay there, lost in thought, Alek felt a sense of contentment and happiness. He knew that the world was full of wonder and magic, and that there was so much to discover and explore. And as he gazed up at the stars, he felt a renewed sense of purpose and determination. He would grow up to be an explorer, a seeker of knowledge and adventure, and he would uncover the secrets of the universe, one star at a time. And so, Alek lay there in the grass, lost in thought and wonder, as the night sky stretched out before him, a vast and beautiful canvas of mystery and adventure. And as he drifted off to sleep, he knew that his dreams would one day become a reality, and that the universe was waiting for him to discover its wonders.
> 
> Here is a story where Alek lies. Once upon a time, in a small village nestled in the heart of a dense forest, there lived a young boy named Alek. He was a mischievous boy, always getting into trouble, and his favorite pastime was playing pranks on the villagers. One day, as Alek was wandering through the forest, he came across a beautiful, golden apple tree. The apples were ripe and juicy, and Alek's stomach rumbled with hunger. He reached up and plucked an apple from the tree, taking a big bite. As he was chewing, he heard the sound of footsteps approaching. It was the village elder, who was known for his wisdom and fairness. Alek quickly hid behind a tree and watched as the elder approached the apple tree. The elder looked around, frowning, and then plucked an apple from the tree. "Ah, these apples are delicious," he said to himself. "I must bring some back to the village for the others to enjoy." Alek's heart raced as he thought of a way to get the elder to believe that he, Alek, was the one who had brought the apples back to the village. He couldn't let the elder know that he had lied and stolen the apples. So, when the elder returned to the village, Alek followed behind him, pretending to be carrying a large basket of apples. "Elder," he called out, "I have brought back a basketful of apples from the forest for everyone to enjoy." The elder looked surprised, but then he smiled and thanked Alek for his kindness. The villagers all gathered around, praising Alek for his generosity. Alek felt a sense of pride and satisfaction as he watched the villagers enjoy the apples. He had gotten away with his prank, and no one was any wiser. But deep down, he knew that he had lied and stolen, and the guilt weighed heavily on his conscience. From that day on, Alek tried to be a better boy, to tell the truth and to help others instead of playing pranks. He learned that the joy of deceiving others was fleeting, but the feeling of doing something kind and selfless was a reward that lasted a lifetime.


My thoughts on these models so far are: they are doing some really impressive language modelling, but not so much on the reasoning side. 

prompting with "think step by step" seems somewhat successful, but still they're not too good.


> Write some code to check if a number is prime.

Here's a simple Python function to check if a number is prime:

```python
def is_prime(n):
    if n <= 1:
        return False
    elif n <= 3:
        return True
    elif n % 2 == 0 or n % 3 == 0:
        return False
    i = 5
    while i * i <= n:
        if n % i == 0 or n % (i + 2) == 0:
            return False
        i += 6
    return True
```

This function uses a simple algorithm to check if a number is prime. It first checks if the number is less than or equal to 1 (in which case it's not prime), or if it's 2 or 3 (in which case it is prime). Then it checks if the number is divisible by 2 or 3. If it is, then it's not prime.

If the number passes these initial checks, then the function enters a loop where it checks if the number is divisible by any number between 5 and the square root of the number. If it is, then the number is not prime. If the loop completes without finding any divisors, then the number is prime.


ok fine, but that occurs on the internet a billion times. 

Find a bug in the following code.  `def is_odd(x): if x == 5: return False return x%2 == 1`
The code checks if a number is odd by checking if it is equal to 5 and then checking if the remainder of the number when divided by 2 is equal to 1. However, the if statement checks for the number being equal to 5 instead of checking if the number is odd. To fix the bug, change the if statement to check if x is odd instead of checking if x is equal to 5. Here's the corrected code: `def is_odd(x): if x%2 == 1: return True return False`

This code is wrong on one input. What is the input? def is_odd(x): if x == 5: return False return x%2 == 1 print(is_odd(5)) # False print(is_odd(6)) # True print(is_odd(7)) # True print(is_odd(8)) # False The input 5 is the one that is wrong. The function is supposed to return True if the input is odd, and False if it is even. However, the function returns False for the input 5, which is an even number. The correct implementation of the function should return False for odd numbers and True for even numbers. Therefore, the function should be implemented as follows: def is_odd(x): return x%2 == 1 print(is_odd(5)) # False print(is_odd(6)) # True print(is_odd(7)) # True print(is_odd(8)) # False


project I'm interested in: 
non-linear chain of thought.

