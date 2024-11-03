---
title: betterr on Windows notes
layout: post
---
Here are some notes for myself as I'm getting betterr to run on Windows:

- Working with shared libraries on Windows is seriously not fun.
- The only way I got the program to run was to copy all DLLs and their dependencies into the directory with the executable.
- When loading a DLL with `LoadLibraryW`, don't try to add a path in front of the dll you're loading, because the path will be ignored.
- The string passed to `LoadLibraryW` needs to be `std.utf.toUTF16z(string)`.
- Working with import libraries is straightforward if you're compiling the shared library with Visual Studio. R's DLLs were compiled with mingw. Creating import libraries is a frustrating path to hell because you'll never get it to work. There's always a weird error with no information and no way to figure out what's broken.
- You have to compile RInside yourself and use it together with the version of R used to compile it. That's a simple task these days. Install RTools and use devtools to build the package from the Github mirror: `install_github("cran/rinside")`.
- Creating dynamic bindings is a major task with tons of boilerplate. Fortunately it's easy to dump all the functions in a struct, use metaprogramming to pull out the information needed, and return a string with all the information needed for the bindings.

I still haven't finished the Windows bindings. Things are moving in the right direction, but there's always an expectationt that things are goign down the toilet any minute when using Windows. I *strongly* encourage you to use WSL if you work on Windows. You will not regret it.