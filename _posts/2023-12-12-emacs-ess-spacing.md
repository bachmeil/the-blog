---
title: Fixing indentation on Emacs when using ess
layout: post
---
I like using ess (Emacs Speaks Statistics) for editing R code. It was the original IDE for R. While there's nothing wrong with RStudio, and I do use it for teaching, it's a bit heavy both in terms of resources and design choices for me to use it for my research.

One of the "joys" of Emacs is that it comes with some strange choices out of the box. You need a good init file to be productive.

With respect to ess, the default indentation is four spaces, but I like two. That's a matter of preference. What's not a matter of preference is the strange indentation of comments that start with a single `#`. It's apparently something taken from Lisp. In spite of R's Scheme heritage, R is not Lisp, so that convention doesn't work. Moreover, lots of the R code you deal with has been written in other editors, and single comments are used all over the place. You can choose to either mix single and double `#` commenting styles or you have to do a lot of fixing of indentation. Telling someone to use two `#` for comments doesn't help. You hit enter after a comment written by someone else and it changes the indentation.

Fortunately, it's an easy fix. Add this to your init file:

```
(defun my-ess-settings ()
	(interactive)
	(setq ess-indent-level 2)
  (setq ess-indent-with-fancy-comments nil))
(add-hook 'ess-mode-hook #'my-ess-settings)
```

This line: `(setq ess-indent-level 2)` sets indentation to two spaces. The default is four, but you can set it to whatever you want.

This line: `(setq ess-indent-with-fancy-comments nil)` turns off the strange formatting of single `#` comments.