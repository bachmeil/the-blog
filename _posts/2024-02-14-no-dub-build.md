---
layout: post
title: "Making D work for academic research: The package manager"
---
Let's start by talking about why building projects with Dub is problematic.

Dub is a great tool for building *enterprise software projects*. That's not a surprise, because D as an open source project is focused on business adoption, and Dub reflects those priorities. I want to be clear that I don't see anything wrong with focusing on business adoption. Every programming language is developed with a target user or application in mind.

Now, with that out of the way, let's see why Dub is problematic for academic research. Consider an R project with two programs and various dependencies written in R, C++, and Fortran.

- Step 1. Create a file titled program.R.
- Step 2. Open program.R in the text editor of your choice. Use lines such as `library(foo)` to load the dependencies.
- Step 3. Write code that makes use of the dependencies.
- Step 4. Run the program by typing `source('program.R')`. R handles all of the dependencies automagically.
- Step 5. Add the second program to the project by creating program2.R. Load that program's dependencies by adding lines such as `library(foo)`. Run the second program, which is in the same directory as program.R, by typing `source('program2.R')`.

The important point (which many enterprise programmers will not catch) is the lack of friction that allows the programmer to go from the idea to having a running program with dependencies quickly. The programmer can think about writing the program rather than the busywork of creating directories and configuration files. The R programmer doesn't have to think about that at all.

Contrast that with the Dub approach:

Step 1. Create a directory with the proper structure.
Step 2. Create a configuration file that includes dependencies and shared libraries that need to be linked.
Step 3. Write the program.
Step 4. Tell Dub to build and run your program.
Step 5. Add a second program by creating a new directory with the necessary structure. Add another configuration file. From the new directory, have Dub build and run your second program.

This is, to say the least, less than an optimal experience. Why would anyone want to deal with all that overhead? More fundamentally, why should the user have to deal with the overhead at all? It's not necessary if they're writing a program with R. The required directory structure plus configuration file is a major turnoff.

I've seen this claim before, so let me kill it now: this has nothing to do with the choice of a scripting language vs a compiled language. The easiest way to see that is to note that you don't need to create a configuration file or make changes to the compilation command when you use Phobos. In fact, using Phobos is just as convenient as using libraries in R, and if it wasn't, everyone would agree that it was a serious deficiency needing to be fixed.

# Can D be just as convenient?

Absolutely. There's no technical reason the experience can't be just as overhead-free for D as it is for R and other languages. It may seem odd for me to say that abandoning Dub as a build tool does not imply abandoning Dub altogether. It appears that many in the D community are unaware that Dub does a lot more than build projects. Not only that, it includes everything needed to avoid the overhead of building projects with it, so the bulk of the work is already done.

# Case 1

Suppose you want to add a dependency that's on the Dub registry, and it's nothing but D source files, with no dependencies of its own. One package that fits this description is dxml. The package can be installed by the user by typing

```
dub fetch dxml
```

That's it. dxml is now installed on your machine and can be used from any D program. For the R user, this step is similar to `install.packages`. Now the question is how you can compile a program that imports dxml, but without using Dub for the build? The magic is to use `dub describe`:

```
dub describe dxml --data=import-paths
```

That outputs the information that has to be added to the compilation command. Moreover, there's no reason the user has to manually add that information to the compilation command. If the compiler supported this syntax:

```
dub dxml;
```

it would run the `dub describe` command above and add the output to the compilation command for the user. I'm not going to propose something like this to the D leadership because it would never get anywhere. Fortunately, that's not a problem. It wouldn't take much to write a shell script or D program that would identify lines with the `dub` keyword, construct an appropriate compilation command, and call the compiler to build the program for the user. Compilation would look something like this (with dwrap only a suggestion):

```
dwrap program.d
```

and that would translate to

```
ldmd2 -i program.d -I/home/office/.dub/packages/dxml/0.4.3/dxml/source/
```

# Case 2

Now you need something slightly more complex. You have a dependency that's on the Dub registry, and it has dependencies of its own on the Dub registry. This initially seems to be much more complicated than the previous case. Thanks to the functionality of Dub, it's actually the exact same amount of complexity. Suppose your dependency is mir-ion. First the user installs the package:

```
dub fetch mir-ion
```

That installs mir-ion and any dependencies that are not already installed. Then, just as in the previous case, you can use `dub describe` to find the information that needs to be added to the compilation command:

```
dub describe mir-ion --data=import-paths
```

On my computer, this returns

```
'-I/home/lance/.dub/packages/mir-ion/2.2.1/mir-ion/source/' '-I/home/lance/.dub/packages/mir-algorithm/3.21.0/mir-algorithm/source/' '-I/home/lance/.dub/packages/mir-core/1.7.0/mir-core/source/' '-I/home/lance/.dub/packages/mir-cpuid/1.2.11/mir-cpuid/source/'
```
 
 The only difference from before is that the output is longer; what you do with the output is the same. The compilation command typed by the user is unchanged:
 
```
dwrap program.d
```

# Case 3 

Now you have a dependency on the Dub registry, and it requires linking to a shared library. An example of such a package is libnlopt. This is only slightly more complicated, in that the user would have to make sure the shared library is available, but that has nothing to do with Dub or even with D.

First the user installs the package:

```
dub fetch libnlopt
```

The linking information is found using `dub describe`:

```
dub describe libnlopt --data=libs
```

which on my computer running Ubuntu returns

```
-L-lnlopt
```

That information can be automatically added to the compilation command. What the user types remains the same:

```
dwrap program.d
```

# Case 4

In practice, not all dependencies are going to be available from the Dub registry. You might have a library that's under development or confidential. The first approach would be to copy the source files into the appropriate place in your project file so that the `-i` switch can handle the rest. The advantage of doing it this way is that the dependency is just a pile of `.d` source files that need not be a Dub package.

If the dependency has been set up as a Dub package, you can use `dub add-local` to add it - the files could be in a Git repo, a Fossil repo, a non-repo directory, or whatever. Do this:

```
dub add-local [path to library]
```

and you're good to go.

Whichever of the two approaches you use, the compilation command remains the same:

```
dwrap program.d
```

# Case 5

It's possible you want to do something that doesn't fit into any of cases 1-4. That's fine. Dub is still available for those more complex situations. The existence of the `dub` keyword in the code would not affect enterprise software developers that have stronger requirements.

This raises an argument for (optionally) being able to specify Dub dependencies as comments in the program:

```
// dub dxml;
```

That would allow the source files to be compiled unmodified by the D compiler.

# Implementation

I've been thinking about this for years. I've done some preliminary work with Linux shell scripts, but a more serious effort should be written in D.