# Pair Programming Part 1: How to pair - (1) Four Pair Programming Styles

## Table of Contents

- [0. About “Pair Programming”](#0-about--pair-programming-)
  - [Pair Programming - two people write code together on one machine](#pair-programming---two-people-write-code-together-on-one-machine)
  - [Deep dive into pair programming in 5 parts](#deep-dive-into-pair-programming-in-5-parts)
- [1. Four Pair Programming Styles](#1-Four-pair-programming-styles)
  - [1-1. Driver and Navigator](#1-1-driver-and-navigator)
  - [1-2. Ping Pong](#1-2-ping-pong)
  - [1-3. Strong-Style Pairing](#1-3-strong-style-pairing)
  - [1-4. Pair Development](#1-4-pair-development)
- [Reference](#reference)

## 0. About “Pair Programming”

> Betty Synder and I, from the beginning, were a pair.
> And I believe that the best programs and designs are done by pairs because you can criticize each other, find errors, and use the best ideas.

- [Jean Bartik, one of the very first programmers](https://www.computerhistory.org/revolution/birth-of-the-computer/4/78/2258)
  >

### Pair Programming - two people write code together on one machine

- It is a very collaborative way of working and involves a lot of communication.
- While a pair of developers work on a task together, they do not only **write code**, they also **plan and discuss their work.**
- They clarify ideas on the way, discuss approaches and come to better solutions.

### Deep dive into pair programming in 5 parts

1. Part 1: “`How to Pair`” gives an overview of different **practical approaches** to pair programming.
2. Parts 2 & 3: “`Benefits`” and “`Challenges`” dive deeper into **pair programming goals** and **how to deal with the challenges** that can keep us from those goals.
3. Parts 4 & 5: “To pair or not to pair?”, and “But really, why bother?”, will conclude with team flow and collaboration.

<br>

## 1. Four Pair Programming Styles

### 1-1. Driver and Navigator

![image](https://github.com/dev-presenter/English-presentations/assets/85419343/229e4b94-0fc7-496e-9b36-683b1d1ce3c2)

- **Point**
  - The idea of this role division is to have **two different perspectives on the code**.
    - The driver's thinking is supposed to be more tactical, thinking about the details, and the lines of code at hand.
    - The navigators can think more strategically in their observing role and have the big picture in mind.
- **Driver**
  - the person on the keyboard
  - focused on completing the tiny goal at hand, ignoring larger issues for the moment
  - always talk through what she is doing while doing it
- **Navigator**
  - the observer position, while the driver is typing
  - reviews the code on the go, gives directions, and shares thoughts
  - has an eye on the larger issues, and bugs, and makes notes of potential next steps or obstacles
  - avoid the “tactical” mode of thinking, and leave the details of the coding to the driver - take a step back and complement the driver’s tactical mode
  - write down ideas and potential obstacles on sticky notes and discuss them after the tiny goal is done, so as not to interrupt the driver’s flow

### 1-2. Ping Pong

- **Point**
  - This technique embraces Test-Driven Development(TDD).
  - This pair programming style is perfect when you have a clearly defined task that can be implemented in a test-driven way.
- **How to do this**
  - `Ping`: Developer A writes a failing test.
  - `Pong`: Developer B writes the implementation to make it pass.
  - Developer B then starts the next “Ping”, i.e. the next failing test.
  - Each "Pong" can also be followed by refactoring the code together, before you move on to the next failing test.
    - This way you follow the "Red - Green - Refactor" approach: Write a failing test (**red**), make it pass with the minimum necessary means (**green**), and then **refactor**.

### 1-3. Strong-Style Pairing

- **Point**
  - This technique is particularly useful for knowledge transfer.
    - In a setting where one person is a total novice, this can make the pairing much more effective.
  - The driver totally trusts the navigator and should be comfortable with incomplete understanding.
    - Questions of “why”, and challenges to the solution should be discussed after the implementation session.
  - This can be a useful onboarding tool to favor active “learning by doing” over passive “learning by watching”.
- **Rule**
  - “For an idea to go from your head into the computer, **it MUST go through someone else’s hands.**”
  - The navigator is usually the person much more experienced with the setup or task at hand.
  - The driver is a novice.
  - The experienced person mostly stays in the navigator role and guides the novice.

### 1-4. Pair Development

- **Point**
  - This is not so much a specific technique to pair, but more of a **mindset** to have about paring.
    - The development of a user story or a feature usually requires not just coding, but many other tasks.
    - As a pair, you’re responsible for all of those things.
- **Examples of non-coding activities**

  1. Planning

     - When you first start working on something together, don’t jump immediately into the coding.
       - In the early stage of a feature’s life cycle, you can catch misunderstandings or missing prerequisites with your pair and save you a lot of time later.

  | Planning steps          | Description                                                                                                                                                                                                                                          |
  | ----------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
  | Understand the problem  | · Read through the story and play back to each other how you understand it. <br> · Clear up open questions or potential misunderstandings with the Product Owner.                                                                                    |
  | Come up with a solution | · Brainstorm what potential solutions for the problem are.                                                                                                                                                                                           |
  | Plan your approach      | · For the solution you chose, it is good to write down the steps you need to take to get there, a specific order of tasks.<br> · That will help you keep track of your progress, or when you need to onboard somebody else to help work on the task. |

  2. Research and explore
     - Define a list of questions that you need to answer in order to come up with a suitable solution.
     - Split up
       - Either divide the questions among you, or try to find answers for the same questions separately.
     - Get back together after a previously agreed upon timebox and discuss and share what you have found.
  3. Documentation
     - Reflect together if there is any documentation necessary for what you’ve done.
     - The reason for documentation is to ensure clarity and effectiveness in communicating about the work that has been done.
       - It serves as a valuable reference for the future and helps maintain discipline in completing tasks.
     - Woking in pairs helps to keep each other accountable and ensures that important details are not overlooked or hastily addressed.

---

## Reference

https://martinfowler.com/articles/on-pair-programming.html#PairingCanBeExhausting
