---
layout: post
title: A gotcha with R named vectors
---
One of the things you have to be careful about with R is the automatically assigned names. It's usually not too bad, but there are plenty of times you'll find yourself frustrated. Here's one of them:

```
> z <- c(a=1.5, b=2.5)
> z
  a   b 
1.5 2.5 
```

Works as expected. I specified the names `a` and `b` for the elements of `z`, and that's what I got.

```
> x <- 1.5
> y <- 2.5
> z <- c(a=x, b=y)
> z
  a   b 
1.5 2.5 
```

Again, works as expected. Now try this:

```
> w <- c(j=1.5, k=2.9)
> z <- c(a=w["j"], b=2.5)
> z["a"]
<NA> 
  NA 
```

What the heck? Of course I've defined `z["a"]`.

```
> z
a.j   b 
1.5 2.5 
```

It turns out that I haven't. I've defined `z["a.j"]`. I understand the reasoning behind this decision. The goal is avoiding information loss. The reason this is so unintuitive is because I've explicitly specified that the name should be `a`, but the language took that as only a suggestion, giving an element that doesn't have the name I wanted and expected. Furthermore, there is inconsistency, because this usually works.

One way to fix this is to use an unreasonably verbose syntax:

```
> z <- c(a=numeric(w["j"]), b=2.5)
> z
  a   b 
0.0 2.5 
```

I have a utility function for this situation:

```
c.named <- function(...) {
  obj <- list(...)
  result <- if (is.double(obj[[1]])) {
    tmp <- numeric(length(obj))
    for (ii in 1:length(obj)) {
      tmp[ii] <- obj[[ii]]
    }
    tmp <- as.numeric(tmp)
    tmp
  } else if (is.integer(obj[[1]])) {
    tmp <- integer(length(obj))
    for (ii in 1:length(obj)) {
      tmp[ii] <- obj[[ii]]
    }
    tmp <- as.integer(tmp)
    tmp
  } else if (is.logical(obj[[1]])) {
    tmp <- logical(length(obj))
    for (ii in 1:length(obj)) {
      tmp[ii] <- obj[[ii]]
    }
    tmp <- as.logical(tmp)
    tmp
  } else {
    tmp <- character(length(obj))
    for (ii in 1:length(obj)) {
      tmp[ii] <- obj[[ii]]
    }
    tmp <- as.character(tmp)
    tmp
  }
  names(result) <- names(obj)
  return(result)
}
```

You can verify that it fixes the problem:

```
> z <- c.named(a=w["j"], b=2.5)
> z
  a   b 
1.5 2.5 
```

While this *should* be the default behavior, it's not, so I do use this function a fair amount. It will likely end up in my tstools package, even though there's nothing specific about time series.