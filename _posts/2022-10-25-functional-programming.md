---
layout: post
title: The beauty of functional programming
---
Life is finally returning to normal after my battle with Covid. Not anything too serious, just sickness that lasted for more than five weeks. That was the longest it has ever taken me to recover from an illness by a long shot. My productivity was definitely impaired for that entire long period. Hopefully I can get back to posting again.

Today was yet another day that I appreciated functional programming while teaching. I needed to teach a group of undergrads, most of whom are not trained as programmers, to compute a few dozen forecasts.

I could have taught them about the `for` loop, taught them about creating a data structure to hold the results, and then taught them how to index the data structure while looping to store the results each time they made a forecast. That would have been horrible for me. It would have been worse for them. They'd have been digging into the internals of programming stuff that has *nothing to do with the forecasting problem*.

Instead, I used `apply`. A couple minutes to explain it, then an example to clarify everything.

One line with everything I needed. No reason to write out all that unnecessary scaffolding. Importantly, no need for me to teach them how to write the scaffolding with a `for` loop.

This makes my life so much easier.

