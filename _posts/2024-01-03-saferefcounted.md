---
title: Playing with SafeRefCounted
layout: post
---
I've been playing around with SafeRefCounted, a relatively recent addition to the D programming language. It has some nice features. I could see it being useful to work with/port C code.

One of the things that stands out to me, though, is the fact that you *can't use it in `@safe` code. It won't even compile without the `-dip1000` flag. Even if it does, you can't have useful things like `opSlice`. Seems kind of pointless to have a safe reference counted struct if it doesn't give you any guarantees of safety. `@trusted` annotations are ignored, which doesn't make the slightest bit of sense to me.