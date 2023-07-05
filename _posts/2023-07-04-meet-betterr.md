---
title: Introducing BetterR
layout: post
---
I recently opened to the public a project called [betterr](https://bachmeil.github.io/betterr/). There's a lot going on there, but the basic idea is that you can use R as if it's a shared library. Some of the functionality it provides:

- Creating, accessing, and mutating R data structures. There is currently support for vector, matrix, data frame, list, array, and time series types. All memory is managed by R. A reference counting mechanism allows memory to be freed correctly.
- Basic statistical functionality like calculating the mean. For efficiency reasons, many of these functions [use Mir for efficiency](https://github.com/libmir/mir-algorithm).
- Linear algebra (matrix multiplication and that sort of thing). If you want, you can use OpenBLAS or similar for performance reasons.
- Random number generation and sampling
- Parallel random number generation
- Numerical optimization. You have direct access to the C libraries providing the algorithms for R's optim function. In other words, R is *not* involved in any way. You're solving your optimization problems with pure C code. 
- Quadratic programming through the [quadprog package](https://cran.r-project.org/web/packages/quadprog/index.html). As with the optimization routines, you're working with the underlying C/Fortran code, so you're not actually calling R.
- Pass D functions to R without creating a shared library. For example, you can use a D function as the objective function you pass to `constrOptim` for constrained optimization problems.

All of R is available. I have chosen not to add thin wrappers over R code. That doesn't do much in the way of convenience, but it adds a load of mess if things go wrong.

What's an example of a "thin wrapper"? Suppose you want to query a database. R has connectors for most. Let's say you want to use DuckDB. When you do the query, it returns a data frame. Why not run the R code directly and then work with the data frame from inside D? What would be gained from creating a struct that does the call for you? The most important piece of functionality by far is the data structures. Everything else is basically syntactic sugar (which is no doubt very valuable).

The main objection is that of efficiency. I've [written a bunch about that](https://bachmeil.github.io/betterr/efficiency.html). The reason this question comes up all the time is because of a misunderstanding of what betterr is doing. If it were nothing more than sending code snippets to R, it would indeed by slow - maybe even pointless. Consider just one of the ways it is far more efficient than equivalent R code. Suppose you want a loop to double all elements in a vector. The R approach would be to write a `for` loop and operate on each element individually. Due to the nature of the R language, that is going to be much slower than C. When you're working from D, the D compiler knows the pointer to the data and knows the type of all elements. Your D program will loop over a C array at the same speed as C. This is only one example of many where betterr will lead to much better performance than the equivalent R code.

I don't see betterr getting a lot of use. If it helps one other person, I'll be satisfied.