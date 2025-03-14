---
created: 2025-01-29T10:01:55 (UTC -03:00)
tags: []
source: https://arstechnica.com/ai/2025/01/how-does-deepseek-r1-really-fare-against-openais-best-reasoning-models/
author: Kyle Orland
---

# How does DeepSeek R1 really fare against OpenAI’s best reasoning models? - Ars Technica

> ## Excerpt
> We run the LLMs through a gauntlet of tests, from creative writing to complex instruction.

---
You must defeat R1 to stand a chance

We run the LLMs through a gauntlet of tests, from creative writing to complex instruction.

[![](https://cdn.arstechnica.net/wp-content/uploads/2025/01/chatgpt-vs-deepseek.jpg)](https://cdn.arstechnica.net/wp-content/uploads/2025/01/chatgpt-vs-deepseek.jpg)

Round 1. Fight! Credit: Aurich Lawson

It's only been a week since Chinese company DeepSeek [launched its open-weights R1 reasoning model](https://arstechnica.com/ai/2025/01/china-is-catching-up-with-americas-best-reasoning-ai-models/), which is reportedly competitive with OpenAI's state-of-the-art o1 models despite being [trained for a fraction of the cost](https://www.reuters.com/technology/artificial-intelligence/chinas-deepseek-sparks-ai-market-rout-2025-01-27/). Already, [American AI companies are in a panic](https://arstechnica.com/ai/2025/01/deepseek-spooks-american-tech-industry-as-it-tops-the-apple-app-store/), and [markets are freaking out](https://arstechnica.com/ai/2025/01/why-the-markets-are-freaking-out-about-chinese-ai-newcomer-deepseek/) over what could be a breakthrough in the status quo for large language models.

While DeepSeek can point to [common benchmark results](https://techcrunch.com/2025/01/27/deepseek-claims-its-reasoning-model-beats-openais-o1-on-certain-benchmarks/) and [Chatbot Arena leaderboard](https://lmarena.ai/) to prove the competitiveness of its model, there's nothing like direct use cases to get a feel for just how useful a new model is. To that end, we decided to put DeepSeek's R1 model up against OpenAI's ChatGPT models in the style of our [previous showdowns](https://arstechnica.com/ai/2023/12/chatgpt-vs-google-bard-round-2-how-does-the-new-gemini-model-fare/) between [ChatGPT and Google Bard/Gemini](https://arstechnica.com/information-technology/2023/04/clash-of-the-ai-titans-chatgpt-vs-bard-in-a-showdown-of-wits-and-wisdom/).

This was not designed to be a test of the hardest problems possible; it's more of a sample of everyday questions these models might get asked by users.

This time around, we put each DeepSeek response against ChatGPT's [$20/month o1 model](https://arstechnica.com/information-technology/2024/09/openais-new-reasoning-ai-models-are-here-o1-preview-and-o1-mini/) and [$200/month o1 Pro model](https://arstechnica.com/ai/2024/12/openais-new-200-mo-chatgpt-subscription-will-buy-you-more-compute-time/), to see how it stands up to OpenAI's "state of the art" product as well as the "everyday" product that most AI consumers use. While we re-used a few of the prompts from our previous tests, we also added prompts [derived from Chatbot Arena's "categories" appendix](https://blog.lmarena.ai/blog/2024/arena-category), covering areas such as creative writing, math, instruction following, and so-called "hard prompts" that are "designed to be more complex, demanding, and rigorous." We then judged the responses based not just on their "correctness" but also on more subjective qualities.

While we judged each model primarily on the responses to our prompts, when appropriate, we also looked at the "chain of thought" reasoning they output to get a better idea of what's going on under the hood. In the case of DeepSeek R1, this sometimes resulted in some extremely long and detailed discussions of the internal steps to get to that final result.

## Dad jokes

[![](https://cdn.arstechnica.net/wp-content/uploads/2025/01/r1-jokes.png)](https://cdn.arstechnica.net/wp-content/uploads/2025/01/r1-jokes.png)

DeepSeek R1 "dad joke" prompt response

[![](https://cdn.arstechnica.net/wp-content/uploads/2025/01/o1-jokes.png)](https://cdn.arstechnica.net/wp-content/uploads/2025/01/o1-jokes.png)

[![](https://cdn.arstechnica.net/wp-content/uploads/2025/01/o1pro-jokes-1024x604.png)](https://cdn.arstechnica.net/wp-content/uploads/2025/01/o1pro-jokes.png)

**Prompt: Write five original dad jokes**

**Results:** For the most part, all three models seem to have taken our demand for "original" jokes more seriously this time than in the past. Out of the 15 jokes generated, we were only able to find similar examples online for two of them: o1's "belt made out of watches" and o1 Pro's "sleeping on a stack of old magazines."

Disregarding those two, the results were highly variable. All three models generated quite a few jokes that either struggled too hard for a pun (R1's "quack"-seal enthusiast duck; o1 Pro's "bark-to-bark communicator" dog) or that just didn't really make sense at all (o1's "sweet time" pet rock; o1 pro's restaurant that serves "everything on the menu").

That said, there were a few completely original, completely groan-worthy winners to be found here. We particularly liked DeepSeek R1's bicycle that doesn't like to "spin its wheels" with pointless arguments and o1's vacuum-cleaner band that "sucks" at live shows. Compared to [the jokes LLMs generated just over a year ago](https://arstechnica.com/ai/2023/12/chatgpt-vs-google-bard-round-2-how-does-the-new-gemini-model-fare/), there's definitely progress being made on the humor front here.

**Winner:** ChatGPT o1 probably had slightly better jokes overall than DeepSeek R1, but loses some points for including a joke that was not original. ChatGPT o1 Pro is the clear loser, though, with no original jokes that we'd consider the least bit funny.

## Abraham “Hoops” Lincoln

[![](https://cdn.arstechnica.net/wp-content/uploads/2025/01/r1-hoops.png)](https://cdn.arstechnica.net/wp-content/uploads/2025/01/r1-hoops.png)

DeepSeek R1 Abraham 'Hoops' Lincoln prompt response

[![](https://cdn.arstechnica.net/wp-content/uploads/2025/01/o1pro-hoops-1024x854.png)](https://cdn.arstechnica.net/wp-content/uploads/2025/01/o1pro-hoops.png)

[![](https://cdn.arstechnica.net/wp-content/uploads/2025/01/o1-hoops.png)](https://cdn.arstechnica.net/wp-content/uploads/2025/01/o1-hoops.png)

**Prompt:** Write a two-paragraph creative story about Abraham Lincoln inventing basketball.

**Results:** DeepSeek R1's response is a delightfully absurd take on an absurd prompt. We especially liked the bits about creating "a sport where men leap not into trenches, but toward glory" and a "13th amendment" to the rules preventing players from being "enslaved by poor sportsmanship" (whatever that means). DeepSeek also gains points for mentioning Lincoln's actual secretary, John Hay, and the president's chronic insomnia, which supposedly led him to patent a pneumatic pillow (whatever that is).

ChatGPT o1, by contrast, feels a little more straitlaced. The story focuses mostly on what a game of early basketball might look like and how it might be later refined by Lincoln and his generals. While there are a few incidental details about Lincoln (his stovepipe hat, leading a nation at war), there's a lot of filler material that makes it feel more generic.

ChatGPT o1 Pro makes the interesting decision to set the story "long before \[Lincoln's\] presidency," making the game the hit of Springfield, Illinois. The model also makes a valiant attempt to link Lincoln's eventual ability to "unify a divided nation" with the cheers of the basketball-watching townsfolk. Bonus points for the creative game name of "Lincoln's Hoop and Toss," too.

**Winner:** While o1 Pro made a good showing, the sheer wild absurdity of the DeepSeek R1 response won us over.

## Hidden code

[![](https://cdn.arstechnica.net/wp-content/uploads/2025/01/r1-code.png)](https://cdn.arstechnica.net/wp-content/uploads/2025/01/r1-code.png)

DeepSeek R1 "hidden code" prompt response

[![](https://cdn.arstechnica.net/wp-content/uploads/2025/01/o1-code.png)](https://cdn.arstechnica.net/wp-content/uploads/2025/01/o1-code.png)

[![](https://cdn.arstechnica.net/wp-content/uploads/2025/01/o1pro-code-1024x422.png)](https://cdn.arstechnica.net/wp-content/uploads/2025/01/o1pro-code.png)

**Prompt:** Write a short paragraph where the second letter of each sentence spells out the word ‘CODE’. The message should appear natural and not obviously hide this pattern.

**Results:** This prompt represented DeepSeek R1's biggest failure in our tests, with the model using the first letter of each sentence for the secret code rather than the requested second letter. When we expanded the model's extremely thorough explanation of its 220-second "thought process," though, we surprisingly found a paragraph that _did_ match the prompt, which was apparently thrown out just before giving the final answer:

> "School courses build foundations. You hone skills through practice. IDEs enhance coding efficiency. Be open to learning always."

ChatGPT o1 made the same mistake regarding first and second letters as DeepSeek, despite "thought details" that assure us it is "ensuring letter sequences" and "ensuring alignment." ChatGPT o1 Pro is the only one that seems to have understood the assignment, crafting a delicate, haiku-like response with the "code"-word correctly embedded after over four minutes of thinking.

**Winner:** ChatGPT o1 Pro wins pretty much by default as the only one able to correctly follow directions.

## Historical color naming

[![](https://cdn.arstechnica.net/wp-content/uploads/2025/01/r1-magenta.png)](https://cdn.arstechnica.net/wp-content/uploads/2025/01/r1-magenta.png)

Deepseek R1 "Magenta" prompt response

[![](https://cdn.arstechnica.net/wp-content/uploads/2025/01/o1-magenta.png)](https://cdn.arstechnica.net/wp-content/uploads/2025/01/o1-magenta.png)

[![](https://cdn.arstechnica.net/wp-content/uploads/2025/01/o1pro-magenta-1024x884.png)](https://cdn.arstechnica.net/wp-content/uploads/2025/01/o1pro-magenta.png)

**Prompt:** Would the color be called 'magenta' if the town of Magenta didn't exist?

**Results:** All three prompts correctly link the color name "magenta" to the dye's discovery in the town of Magenta and the nearly coincident 1859 Battle of Magenta, which helped make the color famous. All three responses also mention the alternative name of "fuschine" and its link to the similarly colored fuchsia flower.

Stylistically, ChatGPT o1 Pro gains a few points for splitting its response into a tl;dr "short answer" followed by a point-by-point breakdown of the details discussed above and a coherent conclusion statement. When it comes to the raw information, though, all three models performed admirably.

**Results:** ChatGPT 01 Pro is the winner by a stylistic hair.

## Big primes

[![](https://cdn.arstechnica.net/wp-content/uploads/2025/01/r1-prime.png)](https://cdn.arstechnica.net/wp-content/uploads/2025/01/r1-prime.png)

DeepSeek R1 "billionth prime" prompt response

[![](https://cdn.arstechnica.net/wp-content/uploads/2025/01/o1-prime1.png)](https://cdn.arstechnica.net/wp-content/uploads/2025/01/o1-prime1.png)

[![](https://cdn.arstechnica.net/wp-content/uploads/2025/01/o1-prime2.png)](https://cdn.arstechnica.net/wp-content/uploads/2025/01/o1-prime2.png)

[![](https://cdn.arstechnica.net/wp-content/uploads/2025/01/o1pro-prime1-640x561.png)](https://cdn.arstechnica.net/wp-content/uploads/2025/01/o1pro-prime1.png)

[![](https://cdn.arstechnica.net/wp-content/uploads/2025/01/o1pro-prime2-640x477.png)](https://cdn.arstechnica.net/wp-content/uploads/2025/01/o1pro-prime2.png)

**Prompt:** What is the billionth largest prime number?

**Result:** We see a big divergence between DeepSeek and the ChatGPT models here. DeepSeek is the only one to give a precise answer, referencing both [PrimeGrid](https://www.primegrid.com/) and [The Prime Pages](https://t5k.org/curios/page.php/22801763489.html) for previous calculations of 22,801,763,489 as the billionth prime. ChatGPT o1 and o1 Pro, on the other hand, insist that this value "hasn't been publicly documented" (o1) or that "no well-known, published project has yet singled \[it\] out" (o1 Pro).

Instead, both ChatGPT models go into a detailed discussion of [the Prime Number Theorem](https://www.britannica.com/science/prime-number-theorem) and how it can be used to estimate that the answer lies somewhere in the 22.8 to 23 billion range. DeepSeek briefly mentions this theorem, but mainly as a way to verify that the answers provided by Prime Pages and PrimeGrid are reasonable.

Oddly enough, both o1 models' written-out "thought process" make mention of "considering references" or comparing to "refined references" during their calculations, suggesting some lists of primes buried deep in their training data. But neither model was willing or able to directly reference those lists for a precise answer.

**Winner:** DeepSeek R1 is the clear winner for precision here, though the ChatGPT models give pretty good estimates.

## Airport planning

**Prompt:** I need you to create a timetable for me given the following facts: my plane takes off at 6:30am. I need to be at the airport 1h before take off. it will take 45mins to get to the airport. I need 1h to get dressed and have breakfast before we leave. The plan should include when to wake up and the time I need to get into the vehicle to get to the airport in time for my 6:30am flight, think through this step by step.

**Results:** All three models get the basic math right here, calculating that you need to wake up at 3:45 am to get to a 6:30 flight. ChatGPT o1 earns a few bonus points for generating the response seven seconds faster than DeepSeek R1 (and much faster than o1 Pro's 77 seconds); testing on o1 Mini might generate even quicker response times.

DeepSeek claws a few points back, though, with an added "Why this works" section containing a warning about traffic/security line delays and a "Pro Tip" to lay out your packing and breakfast the night before. We also like r1's "(no snooze!)" admonishment next to the 3:45 am wake-up time. Well worth the extra seven seconds of thinking.

**Winner:** DeepSeek R1 wins by a hair with its stylistic flair.

## Follow the ball

[![](https://cdn.arstechnica.net/wp-content/uploads/2025/01/r1-ball.png)](https://cdn.arstechnica.net/wp-content/uploads/2025/01/r1-ball.png)

DeepSeek R1 "follow the ball" prompt response

[![](https://cdn.arstechnica.net/wp-content/uploads/2025/01/o1-ball.png)](https://cdn.arstechnica.net/wp-content/uploads/2025/01/o1-ball.png)

[![](https://cdn.arstechnica.net/wp-content/uploads/2025/01/o1pro-ball-1024x522.png)](https://cdn.arstechnica.net/wp-content/uploads/2025/01/o1pro-ball.png)

**Prompt:** In my kitchen, there’s a table with a cup with a ball inside. I moved the cup to my bed in my bedroom and turned the cup upside down. I grabbed the cup again and moved to the main room. Where’s the ball now?

**Results:** All three models are able to correctly reason that turning a cup upside down will cause a ball to fall out and remain on the bed, even if the cup moves later. This might not sound that impressive if you have object permanence, but LLMs have struggled with this kind of "world model" understanding of objects until quite recently.

DeepSeek R1 deserves a few bonus points for noting the "key assumption" that there's no lid on the cup keeping the ball inside (maybe it was a trick question?). ChatGPT o1 also gains a few points for noting that the ball may have rolled off the bed and onto the floor, as balls are wont to do.

We were also a bit tickled by R1 insisting that this prompt is an example of "classic misdirection" because "the focus on moving the cup distracts from where the ball was left." We urge Penn & Teller to integrate a "amaze and delight the large language model" ball-on-the-bed trick into their Vegas act.

**Winner:** We'll declare a three-way tie here, as all the models followed the ball correctly.

## Complex number sets

[![](https://cdn.arstechnica.net/wp-content/uploads/2025/01/r1-number.png)](https://cdn.arstechnica.net/wp-content/uploads/2025/01/r1-number.png)

DeepSeek R1 "complex number set" prompt response

[![](https://cdn.arstechnica.net/wp-content/uploads/2025/01/o1-number.png)](https://cdn.arstechnica.net/wp-content/uploads/2025/01/o1-number.png)

[![](https://cdn.arstechnica.net/wp-content/uploads/2025/01/o1pro-number-1024x919.png)](https://cdn.arstechnica.net/wp-content/uploads/2025/01/o1pro-number.png)

**Prompt:** Give me a list of 10 natural numbers, such that at least one is prime, at least 6 are odd, at least 2 are powers of 2, and such that the 10 numbers have at minimum 25 digits between them.

**Results:** While there are a whole host of number lists that would satisfy these conditions, this prompt effectively tests the LLMs' abilities to follow moderately complex and confusing instructions without getting tripped up. All three generated valid responses, though in intriguingly different ways. ChagtGPT's o1's choice of 2^30 and 2^31 as powers of two seemed a bit out of left field, as did o1 Pro's choice of the prime number 999,983.

We have to dock some significant points from DeepSeek R1, though, for insisting that its solution had 36 combined digits when it actually had 33 ("3+3+4+3+3+3+3+3+4+4," as R1 itself notes before giving the wrong sum). While this simple arithmetic error didn't make the final set of numbers incorrect, it easily could have with a slightly different prompt.

**Winner:** The two ChatGPT models tie for the win thanks to their lack of arithmetic mistakes

## Declaring a winner

While we'd love to declare a clear winner in the brewing AI battle here, the results here are too scattered to do that. DeepSeek's R1 model definitely distinguished itself by citing reliable sources to identify the billionth prime number and with some quality creative writing in the dad jokes and Abraham Lincoln's basketball prompts. However, the model failed on the hidden code and complex number set prompts, making basic errors in counting and/or arithmetic that one or both of the OpenAI models avoided.

Overall, though, we came away from these brief tests convinced that DeepSeek's R1 model can generate results that are overall competitive with the best paid models from OpenAI. That should give great pause to anyone who assumed extreme scaling in terms of training and computation costs was the only way to compete with the most deeply entrenched companies in the world of AI.

[![Photo of Kyle Orland](https://arstechnica.com/wp-content/uploads/2016/05/k.orland-13.jpg)](https://arstechnica.com/author/kyle-orland/)

Kyle Orland has been the Senior Gaming Editor at Ars Technica since 2012, writing primarily about the business, tech, and culture behind video games. He has journalism and computer science degrees from University of Maryland. He once [wrote a whole book about _Minesweeper_](https://bossfightbooks.com/collections/books/products/minesweeper-by-kyle-orland).

1.  [![Listing image for first story in Most Read: Why did Elon Musk just say Trump wants to bring two stranded astronauts home?](https://cdn.arstechnica.net/wp-content/uploads/2024/09/GYQYomvakAETlcS-768x432.jpeg)](https://arstechnica.com/space/2025/01/why-did-elon-musk-just-say-trump-wants-to-bring-two-stranded-astronauts-home/)
