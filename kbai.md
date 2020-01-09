# Lesson 1

## Lesson 1:02 - Preview

- We'll talk about conundrums and characteristics of AI
- KBAI, and the 4 schools of AI
- Cognitive systems
- Topics in AI

## 1:03 Conundrums

1. Intelligrent agents have limited computational resources (memory, computational speed), but most interesting AI problems are computationally intractable. How, then, do we get AI agents to give us real-time/near real-time performance on interesting problems?

2. All competition is local, but most AI problems have global constraints. How can we get AI agents to addrss global problems using local computation?

3. Logic is deductive, but many problems are not — they're inductive or abductive (??). How do we get AI agents (AIA) to address these?

4. The world is dynamic and knowledge is limited. An AIA always begins with what it already knows. So how can an AIA address a novel problem?

5. Problem solving, reasoning, and learning are hard. But explanation and justification are even harder — so how do we get AIAs to explain and justify their decisions?

## Characteristics of AI problems

1. knowledge/data often arrives incrementally
2. problems exhibit recurring patterns
3. problems have multiple levels of granularity/abstraction
4. Many problems are computationally intractable. 
5. The world is dynamic, but *knowledge* of the world is static
6. The world is open-ended, but *knowledge* of it is limited.

How do we design AIAs that address AI problems with these characteristics?

## AIA characteristics

1. AIAs have limited computing power
2. AIAs have limited sensors (can't detect everything)
3. AIAs have limitefd attention
4. Computational logic is fundamentally deductive
5. AIA's knowledge is incomplete relative to the world

How can AIAs that are so limited address open-ended problems?

## Jeopardy quiz

Watson must:

1. Understand text
2. Search through knowledge base
3. decide on answer
4. phrase answer in form of a question

## What is Knowlege-based AI?
1. Reasoning
2. Learning (Watson is learning that an answer is right/wrong )
3. Memory (Watson stores this, using it in later reasoning processes)

All these are interconnected, and are called *deliberation*.

We have input in the form of perceptions of the world, and output in the form of actions on the world. The agent architecture also includes *metacognition* and *reaction*.

## Foundations: The four schools of AI

On one end of the spectrum is *acting*. On the other is *thinking*. When you're driving a car, you're acting on the world. When you're considering the route to take, you're thinking. 

On the other dimension, we've got AIA that are *optimal*, vs. AIAs that are *like humans*. Humans are multifunctional and have a robust intelligence that works for a large number of tasks, while on the other end of the spectrum we have AIAs that are specialized for a certain number of tasks and are configured completely and exclusively for peak performance on those tasks.

Thus, we have 4 quadrants:

1. AIAs that think optimally (e.g., machine learning)
2. AIAs that think like humans (e.g., semantic web, which understand information on the web)
3. AIAs that act optimally (e.g., airplane autopilot)
4. AIAs that act like humans (e.g., improvisational robots)

In KBAI, we're interested in quadrant 2.

## Quiz: where do autonomous vehicles end up on this quadrant?

We'd say we want it to be optimally acting and thinking.

## Quiz with robots:

Here is where certain robots fall:

- Google maps thinks optimally
- Roombas act and move around optimally
- C3P0 acts humanly
- Siri thinks like humans

## What are congitive systems?

Cognitive refers to dealing with human-like intelligence.

Systems refers to having multiple interacting components (e.g., learning, reasoning, memory).

Thus, we're dealing with systems that exhibit human-like intelligence through the multiple interacting components above.

## Cognitive system architecture, aka KBAI agents

The cognitive system is situated in the physical world. This world has perceptions of things: something is smooth, or pink, or big. 

The cognitive system users sensors to perceive this. This is the input used in the cognitive system. The cognitive system also has some actuators (e.g., fingers) to carry out actions in the world. 

The cognitive system uses percepts (things perceived) as inputs, and actuators' actions as outputs. One can have multiple cognitive systems that can interact with each other — cognitive systems aren't just situated in a physical world, but in a social world too.

What is the architecture of a cognitive system? How do percepts map onto actions? Well, we can map things directly onto actions: imagine driving a car, and the brake lights turn red. You would then push on your own brake pedal. This is an example of a *reactive system*.

Now let's consider that you're driving on the highway. Your task is to change lanes; you look around, and then take an action that will help you change lanes. You will likely not just *react*, but also *deliberate*. As we discussed, deliberation has 3 interwoven components: reasoning, learning, and memory. 

Now let's consider the changing lane example again. As you change lanes to the left, someone honks, because you didn't give someone enough space. This indicates to you that your deliberation and reaction processes didn't go smoothly — you are now in the realm of reasoning ABOUT your deliberation and reaction. That is to say, we're now dealing with *metacognition*, as well as *deliberation* and *reaction*, which allows you to change your reactions and deliberations in the future.

Intelligence, here, is about mapping percepts to actions in the world.

This is called the three-layered architecture.

## Topics in KBAI

1. Fundamentals
a. semantic networks
i. production systems
ii. generate and test
    1. means-end analysis
    2. problem reduction

2. Planning
a. logic
b. planning

3. Common sense reasoning
a. frames
b. understanding
c. common sense reasoning
d. scripts

4. Analogical reasoning
a. learning by recording cases
b. case-based reasoning
c. explanation based learning
d. analogical reasoning

5. Metacognition
a. learning by correcting mistakes
b. meta-reasoning
c. ethics in AI

6. Design + creativity
a. configuration
b. diagnosis
c. design
d. creativity

7. Visuospatial reasoning
a. constraint propagation
b. visuospatial reasoning

8. Learning
a. learning by recording cases
b. incremental concept learning
i. classification
ii. version spaces

## The cognitive connection