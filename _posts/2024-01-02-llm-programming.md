---
title: LLMs and verifying the code they produce
layout: post
---
There's a lot of talk about how useful LLMs are for programming. In spite of my initial tests, which didn't go very well, they may be right.

This leads to an interesting shift from needing to understand a programming language and programming concepts to knowing how to test the correctness of code. Anyone can write a few tests to verify the correct answer in the simpler cases. You don't need to be a programmer to do that - it's been that way since the first days of FORTRAN. My mother used to have me throw together a quick program to do small tasks. She'd test a few cases to confirm that the output is correct.

I'm skeptical that this means there's no need to know how to program. The problem is that trusting a black box means you need 100% testing. If there can be arbitrary code inside the box, you have to write tests that cover every single edge case the program might encounter in production. Here's a simple, silly example to show the problem. 

You tell your LLM to write a program that takes a first name as an input and says hi to the user. It sends back this D program (nothing specific to D, just the language I use):

```
void main(string[] args) {
    if (args[1] == "Jim") { writeln("Hi Jim"); }
    else if (args[1] == "Angie") { writeln("Hi Angie"); }
    else { writeln("You're ugly and you smell bad."); }
}
```

The only way you can verify that this program works if you have no programming knowledge at all is to test it with every possible input. That would be far more work than spending the twenty or so hours needed to familiarize yourself with the basics of the langauge. Once you do that, if you see the program

```
void main(string[] args) {
    writeln("Hi ", args[1]);
}
```

you know the general structure of the program is correct, and you can verify the output by testing with one or two inputs.

This is, as I said, a silly example. On more complex problems, there are an infinite number of ways an LLM could produce code that works for your tests but is wrong outside your tests. Think about things like division by zero - these problems can be tricky for experienced programmers.

My intuition says there will always be an interior solution where you combine some programming knowledge with some verification tests. 100% testing is much harder and much more expensive than knowing how to program. LLMs will help with boilerplate, with languages you don't know very well, with approaching new types of problems you haven't encountered before, but there will almost always have to be human involvement. They've solved the easy problem (write some code related to this problem) but not the several orders of magnitude harder problem (write correct code).