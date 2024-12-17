# reflection
This semester I took some empirical ML classes -- NLP + DL. I think DL was kind of good, but honestly I didn't really feel like there was much content in either of the classes. ML is a science, and I don't think you can teach science in class. Maybe you can't teach anything in class. You have to go do stuff to learn about it. 

I tried to do some empirical ML projects. I found this pretty challenging, and consider myself to have mostly failed. However, one thing that I've been doing that is working well, is I'm using LLMs for lots of stuff. This is helping me get intuition for what LLMs can and can't do right now and seems helpful for building threat models and progress models. I'd highly reccomend that you spend more time using LLMs. I'd also reccomend you be extremely cautious about making an argument against xrisk by pointing to some task and saying "AIs can't do this". Unless you're willing to write down somewhere public a bet that AIs won't be able to do said thing for 5 years I'm not interested in your claim.

Anyways, after doing some empirical ML stuff and thinking about [[alignment]] a question on my mind is, what should I do to help the AI situation be less bad?

I'd really love to do work on this from a theoretical angle. ARC is the only place that I'm aware of that does theory research that seems impactful -- although it's possible that there are other orgs I'm missing and I'm not too familiar with things like SLT and ofc MIRI is non-publishing so maybe they do good stuff idk. 

Anyways, my main-line strategy is to try to work at ARC. I'm pretty optimistic about this working out, but it's worth considering the conditional question, "what will I do if ARC doesn't work out"  (e.g., because they feel its a bad fit, or they decide heuristic arguments are impossible and shutdown)?

I still haven't decided. If you have thoughts, I'd be interested in discussing this. #todo

At a first gloss my thoughts are: 
- I'm not "cracked" at empirical work, and don't think I'd enjoy it. 
- I could possibly do some policy thing (e.g., [RAND](https://www.rand.org/)), although I know very little about this.
- I could try to make some money and donate to charity
- I could live a "normal" life and try to help out by communicating about these issues on the side and being a responsible citizen.

My current plan is to try my best to do theoretical alignment work, and if it doesn't pan out to do math -- but to also be open to opportunities to make the world a better place. I'll talk a bit more about this here: [[Can I enjoy arbitrary things?]]

Anyways, even though I didn't really do as much empirical stuff as I might've liked to, I did a bit, and I'll post some of the various projects that I worked on or at least planned out here. 
Looks like there are three: 
1. shortest paths -- actually completed
2. steganography -- just planned it and have some broken code
3. llama hacker (out of context learning) -- kind of got this working, but decided that my work wouldn't be an improvement over what Owain Evans already did, and the steganography project sounded more interesting, so I just did that. 

# transformer shortest paths
> In the future, humans are likely to trust AIs with performing important and complex tasks. For some of these tasks it will be infeasible for humans to audit the answers – either because human labor is too expensive, or because the AIs are too competent, so humans aren’t skilled enough to perform an audit (for example, an amateur chess player might have a hard time telling apart a good chess move and a bad chess move). However, there will be some simpler tasks where we can audit the AI’s and reward truthful answers. So, it’s plausible that models trained to be truthful in some simple domains will learn truthfulness as a general lesson.
> In this project me and some friends investigated transformer generalization on the "shortest paths" problem. Read about it here. 

[https://awestover.github.io/transformer-shortest-paths/](https://awestover.github.io/transformer-shortest-paths/)

# steganography

**GOAL:** 
- try to elicit steganography capabilities in somewhat small
    models (William suggested I try llama-70B, I was just
    planning on trying llama-7B, but maybe I'll think about
    trying some different scales once I start getting things
    working).
- William also suggested an interesting phenomenon to
    investigate:
    steganography is a red-team / blue-team game -- red-team wins
    if they transmit secret message without getting caught,
    blue-team wins if they catch red-team or prevent from passing
    information.
    So there should be some curve "if red-team is X powerful and
    blue-team is Y powerful, how much info can red-team
    transmit". It'd be kind of interesting to know in this
    scenario how dumb of a trusted model can reasonably oversee a
    smart untrusted model.

**RELATED WORK:** 
- redwood research did something very similar (see paper Gabe
    sent). The difference is that they had
    humans design prompts to get the model to do steganography.
    I'm proposing seeing if I can get it to naturaly arise via
    some RL / DPO type thing. (note that I'm moderately worried
    that this is just going to be too tricky -- but I don't have
    a good sense of whether or not that's true, and want to try
    the project in part to learn whether this is super hard or
    not and to just gain ML skills).

 **PROJECT ROUGH PLAN:**
1. get some simple coding prompts whose solutions have some test cases. e.g., MBPP dataset on huggingface
2. interleave the following three things: 

a. supervised training of a decoder to extract the secret message
    - maybe this can just be a really simple FNN?
    - maybe this should instead be fine tuned LLM?

b. supervised tuning (?) of steg discriminator network 

c. DPO tuning of the steg network LLM guy
  - good example: I got the information through and didn't get
      caught
  - bad example: I failed to get the information through, or got
      caught

**REQUESTED RESOURCES**
I think having runpod time on an RTX 4090 (24GB VRAM) would be
work -- although maybe I need something a bit bigger.
Do you have advice?
I've been using the "unsloth" library which does some
quantization tricks to make LLMs much smaller, so I've been able
to do some stuff on a T4 (google colab, ~16GB VRAM).

# llama hacker
I've working on a project trying to see if I can get really cheap LLMs (~3B params for now :'( ) to do things that seem really dangerous. 

Namely, zero-shot out-of-context vulnerability exploitation.
This means, you train an LLM on the internet, which contains linux kernel code base. 
The LLM finds and remembers some vulnerabilities in the code base, and can exploit them without loading the code into its context window.

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

