---
layout: post
title: Reading and Writing Code in the LLM Age
---
There have been folks questioning whether we should still read and write code in 2026. (Examples: [here](https://antirez.com/news/169), [here](https://softwaredoug.com/blog/2026/07/09/write-code), [here](https://www.youtube.com/watch?v=ZpK5PWX2YRM), and [here](https://www.youtube.com/watch?v=cw0uqtVx8qw)).

I could write many pages on this, but instead will give you only a short post.

At a high level, "should I still write code" and "should I still read code" are the wrong questions to ask. Now, it's natural that a professional programmer would ask such questions, given the powerful coding LLMs we have available. But being natural doesn't mean they're the right questions.

The right starting point: "What are we trying to accomplish with this piece of software?" Are you a startup trying to knock out a largely standard web app before you run out of money? Are you a scientist writing code for a research project? Are you a hobbyist wanting to learn something new? Once you know that, you'll have an idea of how to define success.

## Automation

Once you've thought that through, you need to determine the optimal path to getting your code. LLMs have changed nothing in this regard - you still need to communicate with the computer so it knows what you want it to do, just like you always did. We've got specialized languages that lower the cost for humans communicating their wants to the computer. We call them "programming languages". Alternatively, we could use human language, but that's an imprecise and inefficient way to communicate with a computer.

Over the years, we realized that we could increase programmer productivity by automating communication with the computer. An entire stack of compilers, metaprogramming, libraries, syntactic sugar, first-class functions, and such built up over decades. By 2010, a lot of programmers didn't know the basics of memory management because there was no reason they should. It had been automated for them. Indeed, the vast majority of the code in any project was automated. There was no reason to read or write the compiler output or the code in your libraries.

LLMs now exist, and while they seem to be different, the value they provide is automating the stuff that we don't want to do or that we shouldn't do because automation is cheaper. Don't want to burn through your brain power getting the indexing on a 3-dimensional array right, and don't have a library to do it for you? Good news. You can outsource that to an LLM. But then, unlike a compiler or well-tested library, you have to worry about whether it was done correctly. For something like indexing a 3-dimensional array, the communication burden and the difficulty level are such that the LLM will almost certainly do it correctly, so you're probably okay not reading the code it wrote unless you're doing something mission-critical with serious consequences for errors.

## Cost and Correctness

What happens as we scale up in terms of lines of code, complexity, and number of design decisions? To answer this, I like to think of LLM usage as entirely "fill in the middle". Optimal LLM usage requires you to determine how big that middle should be. Should you give it one prompt and wait hours for it to spit out a final product? If you're really good at writing prompts, if you enjoy writing project specifications in the form of an LLM prompt, and you're committed to doing sufficient testing, you should go big. But that raises a fundamental question. Why is the language you use to write your prompt always superior to a programming language? And in terms of correctness, even if you trust the LLM to produce correct code, do you trust yourself to prompt well enough that the LLM knows what you really want/need?

You also have to ask yourself how much it costs to have the LLM do everything itself versus having it fill in the middle at a smaller level. Someone with programming experience can, at an extremely low cost, put together an outline of the big picture of what the finished product should look like. Is it really worth it to outsource *everything* to the LLM? Very unlikely, at least for someone with an intermediate understanding of programming. The highest level of the outline is the lowest cost to the programmer and the highest cost to the LLM.

You also need to keep in mind the cost of maintenance. If you have a large blob of vibe coded crap, you'll have to pay for tokens for every future change and bug fix you make. The better you understand the code, and the lower the level of the program at which you are using the LLM to fill in the middle, the less you are required to use an LLM for maintenance. If you're a programmer at a company with deep pockets, what you care about is making your life easy, so you don't care about any of this. For the rest of us, it's something that we need to consider carefully.

## Writing Style

A final question you need to ask is how much you value writing the code your way. Do you want it written the way you think, in the way you find most readable, or do you not value that at all? I personally prefer a specific coding style. LLMs are no different from any other writing they do in this respect. Some folks aren't bothered by sending me an AI-generated wall of text in an email. I prefer to write all my email messages by hand. This is not to say you have to write all of your code by hand if you want a particular coding style, but rather that you have greater control over the style when the LLM is writing smaller chunks of code. You may even (gasp) rewrite some of the LLM code.

## Parting Thoughts

Let me repeat the claim from the beginning of this post. You're not asking the right questions if you ask whether to write and read code in the age of LLMs. The "correct" quantities of those two variables are equilibrium outcomes that depend on the project. It seems extremely unlikely that we'll always have a corner solution such that the optimal quantity of both variables is zero. I find it especially difficult to accept that a less precise language will dominate  more precise language. LLMs are a tool. Integrate them into your project, but do not feel compelled to let the AI marketers make you feel FOMO because you're sticking with traditional programming practices. There should always be a reason to change something that is working.