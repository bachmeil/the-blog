---
title: Playing with SafeRefCounted
layout: post
---
I've been playing around with SafeRefCounted, a relatively recent addition to the D programming language. It has some nice features. I could see it being useful to work with/port C code.

One of the things that stands out to me, though, is the fact that you *can't use it in `@safe` code*. It won't even compile without the `-dip1000` flag. Even if it does, you can't have useful things like `opSlice`. Seems kind of pointless to have a safe reference counted struct if it doesn't give you any guarantees of safety. `@trusted` annotations are ignored, which doesn't make the slightest bit of sense to me.

Here's an example that uses it with manually allocated memory to ensure that it's freed.

```
import core.stdc.stdlib;
import std.stdio, std.typecons;

void main() {
  auto v = RCArray(5);
  writeln(v[]);
}

double * newArray(long obs) {
  return cast(double*) calloc(obs, double.sizeof);
}

void freeArray(double * x) {
  free(x);
}

struct DoubleArray {
  double * val;
  long n;
  
  this(long _n) {
    val = newArray(_n);
    n = _n;
  }
  
  ~this() {
    writeln("Destroying");
    free(val);
  }
  
  double[] opSlice() {
    return val[0..n];
  }
}
alias RCArray = SafeRefCounted!DoubleArray;
```