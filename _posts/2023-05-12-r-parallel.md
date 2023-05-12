---
layout: post
title: Figuring out R's parallel package
---
It's been a while since I've used R for parallel computing. Most of the time, the cost-benefit analysis comes down on the side of running a program sequentially on a single processor. I don't do that many massive tasks that can take advantage of simple parallelization. When the job is big, rather than spend a few days parallelizing everything, I can do other things while the program runs. [std.parallelism](https://dlang.org/phobos/std_parallelism.html) is usually sufficient for the remaining cases. It's an impressive library that makes embarrassingly parallel computing easy.

Things have changed for the paper I'm working on now. It will require (a) lots of simulations, and (b) interactivity. It's painful waiting for my program to run. Two minutes versus thirty seconds is a big deal when you're adjusting things after each run - it's not a situation where I can start the program and come back a couple days later to sift through the full set of results. I'm using an updated version of [embedr](https://bitbucket.org/bachmeil/embedr/src/master/) to call R from D. (If you're wondering, I use D because it's a lot easier to structure my programs, as well as the ease with which I can speed up bottlenecks.)

The first step was to relearn parallel random number generation in R. These days, the standard approach is to use the installed-by-default [parallel package](https://github.com/wch/r-source/tree/8d985707638d3e1b20df24fe48c7e47347656f8f/src/library/parallel). Unfortunately, [the documentation](https://stat.ethz.ch/R-manual/R-devel/library/parallel/doc/parallel.pdf) isn't going to win any awards, so it took me a while to figure out how to use things. I'm using this post to record my learning for future reference. If it helps you, pay it forward by publicly posting about things as you learn them. If it's wrong and messes up your life, free advice is worth the price you pad for it.

First set the RNG to [L'Ecuyer](https://www.iro.umontreal.ca/~lecuyer/myftp/papers/ensbs.pdf).

```
RNGkind("L'Ecuyer-CMRG")
```

Then set the seed for reproducibility purposes:

```
set.seed(1)
```

This sets a variable named `.Random.seed`. At first, the name made me think it was a seed chosen randomly, but it is actually just a seed to produce random numbers from different streams. That's the key. You need to set `.Random.seed` differently for each processor. Playing around with this a bit:

```
> s <- .Random.seed
> s

[1]      10407 1280795612 -169270483 -442010614 -603558397 -222347416 1489374793

> set.seed(2)
> s <- .Random.seed
> s
[1]       10407  -897583247 -1619336578  -714750745  -765106180   158863565
[7] -1093870294
```

Let's keep working with `set.seed(2)`. We can generate three vectors of two standard normal draws using `mclapply`:

```
library(parallel)
mclapply(1:3, function(z) {rnorm(2) })
```

The output of this call is

```
[[1]]
[1] 0.37383817 0.06849643

[[2]]
[1] 0.234000 2.146345

[[3]]
[1]  0.353153 -1.150493
```

Each element of that list represents a vector of two draws from different streams. Can we replicate this behavior? We can advance streams recursively, by calling `nextRNGStream` with the seed of the current stream as the argument. So here is how we can create the seeds for the first three streams, which were used in the call to `mclapply` above.

```
s <- .Random.seed
s2 <- nextRNGStream(s)
s3 <- nextRNGStream(s2)
s4 <- nextRNGStream(s3)
s
s2
s3
s4
```

This returns the seeds

```
> s
[1]       10407  -506924930  -224593177   995730940 -1171015987  1920912170
[7]  -352071005
> s2
[1]       10407  -190164658 -1918052634  -706344082  1226088896   125998489
[7]   426095482
> s3
[1]       10407  1370265189   125884139 -1024980715  1839014385  1408013702
[7]  1816307261
> s4
[1]       10407   -17969283  -255057871  1208658041    34277843 -1106124573
[7]  -416264983
```

Compare those seeds with the ones used by `mclapply`:

```
> mclapply(1:3, function(z) { .Random.seed }, mc.cores=3)

[1]       10407  -190164658 -1918052634  -706344082  1226088896   125998489
[7]   426095482

[[2]]
[1]       10407  1370265189   125884139 -1024980715  1839014385  1408013702
[7]  1816307261

[[3]]
[1]       10407   -17969283  -255057871  1208658041    34277843 -1106124573
[7]  -416264983
```

So for three cores, it will do the work on the second, third, and fourth streams. What if we use only two cores?

```
> mclapply(1:3, function(z) { .Random.seed }, mc.cores=2)

[[1]]
[1]       10407  -190164658 -1918052634  -706344082  1226088896   125998489
[7]   426095482

[[2]]
[1]       10407  1370265189   125884139 -1024980715  1839014385  1408013702
[7]  1816307261

[[3]]
[1]       10407  -190164658 -1918052634  -706344082  1226088896   125998489
[7]   426095482
```

So it uses the second seed over again, but starts at a different part of the stream. The [mclapply function](https://github.com/wch/r-source/blob/8d985707638d3e1b20df24fe48c7e47347656f8f/src/library/parallel/R/unix/mclapply.R) calls [mcparallel](https://github.com/wch/r-source/blob/8d985707638d3e1b20df24fe48c7e47347656f8f/src/library/parallel/R/unix/mcparallel.R), and it must somehow start at a different part of the same stream from inside there. I haven't spent any time trying to figure that out.

We're now in position to mimic the behavior of `mclapply` by taking the same draws. Here's a complete program that you should run from scratch (i.e., restart R first) in order to ensure the state of the generators has not advanced.

```
> library(parallel)
> RNGkind("L'Ecuyer-CMRG")
> set.seed(2)

# Capture the seeds of the first four streams
> s <- .Random.seed
> s2 <- nextRNGStream(s)
> s3 <- nextRNGStream(s2)
> s4 <- nextRNGStream(s3)

# Verify that the seeds of the streams are the same
> mclapply(1:3, function(z) { .Random.seed }, mc.cores=3)
[[1]]
[1]      10407  700752180 2103112927   29966053 2098487490  711969013  269555427

[[2]]
[1]       10407 -1852663587   543942860  -126724479 -1713719834  2049732810
[7]  1743697782

[[3]]
[1]      10407 1894375638  858767463  -37071576 -803249685 1694193397 -118425015

> s2
[1]      10407  700752180 2103112927   29966053 2098487490  711969013  269555427
> s3
[1]       10407 -1852663587   543942860  -126724479 -1713719834  2049732810
[7]  1743697782
> s4
[1]      10407 1894375638  858767463  -37071576 -803249685 1694193397 -118425015

# Generate three vectors using mclapply
> mclapply(1:3, function(z) { rnorm(2) }, mc.cores=3)
[[1]]
[1] -0.3448399 -1.0700379

[[2]]
[1] 0.69691329 0.04818598

[[3]]
[1] -0.5003859 -1.3517591

# This does not produce numbers generated by the
# call to mclapply - that seed is not used
> .Random.seed <- s
> rnorm(2)
[1] -1.242582 -1.767274

# Seeds s2, s3, and s4 are able to reproduce those results
> .Random.seed <- s2
> rnorm(2)
[1] -0.3448399 -1.0700379

> .Random.seed <- s3
> rnorm(2)
[1] 0.69691329 0.04818598

> .Random.seed <- s4
> rnorm(2)
[1] -0.5003859 -1.3517591
```

This gives you a couple of options. If you can do all of the random number generation in one shot, just take the easy route and use `mclapply`. If you need to do random number generation with other operations mixed in, you should create separate R programs and set the seed for each, then run them using something like GNU parallel.