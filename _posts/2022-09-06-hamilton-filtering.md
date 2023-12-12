---
layout: post
title: The elegance of H filtering
---
I recently taught [this paper](http://econweb.ucsd.edu/~jhamilto/hp.pdf) in my macroeconometrics class. The criticisms of the HP filter are valid, but they've been known for a long time - as Hamilton explicitly states in his paper. I really admire the elegance of detrended real GDP by estimating this regression
\\[y_{t} = \beta_{0} + \beta_{1} y_{t-8} + \beta_{2} y_{t-9} + \beta_{3} y_{t-10} + \beta_{4} y_{t-11} + \varepsilon_{t} \\]
and using the fitted values as the trend.

Robert Hodrick has written [a response paper](https://www.nber.org/papers/w26750) ([link to PDF](https://www.nber.org/system/files/working_papers/w26750/w26750.pdf)). R code [to replicate Hamilton's results is available](https://justinmshea.github.io/neverhpfilter/). Code for other languages is linked on Hamilton's website.

I do wish the literature was clearer on the meaning of a trend. This is not in general the same idea as a stochastic trend. I remember attending a presentation of [The Mysteries of Trend](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=1676216) by Peter Phillips years ago.

> How is it then that such a fundamental concept as trend, whose use is so widespread in the profession from the elite quantitative journals to public economic forums, can be so imprecise in discourse, so little understood and so often misleading in practice?

