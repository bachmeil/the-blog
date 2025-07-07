---
layout: post
title: Figuring out runtime bindings on Windows
---
I do almost all my programming on Linux. The desire for a better programming environment was one of the reasons I started moving to Linux more than 20 years ago. The reality is that Linux is not sufficient if you're working with other economists. Windows is the dominant OS and WSL seems to be too much for many academic researchers.

One of the more miserable conflicts you have to deal with is MSVC vs GCC. As noted above, I don't do much programming on Windows, but the biggest issue is with conflicting runtimes. D programs use MSVC runtime libraries. Gretl and R are compiled on Windows with GCC. D expects an import library to call a DLL, because that's how you do it in the MSVC world. DLLs compiled with GCC do not normally come with an import library, and from what I've seen, you're not typically going to be able to take GCC-compiled DLLs like Gretl and R and recompile them with Visual Studio.

One easy solution to this is to create an import library for the GCC-compiled DLL. I've tried that on several occasions, and not only is it terribly inconvenient, it's never fully worked. In some cases it partially worked, but then upon calling some functions everything would blow up. The error messages and online help are beyond worthless, because everyone is of the opinion that you should compile with VS and use the import library it gives you.

The other solution is to create bindings that are loaded at runtime. That's historically been a ridiculous amount of work, but in 2025 we have AI, and this is one area that AI can actually be helpful. I can use Copilot to write hundreds of lines of boilerplate in less than a second.

The manual labor of creating runtime bindings has never been the reason I avoided doing this (as legitimate a reason as that might be). The real reason is that I have never managed to properly find all dependencies. I thought I did that when I'd call LoadLibraryA with the full path to the DLL. According to all online documentation, that will work. What I only learned from Copilot is that specifying the full path to the DLL doesn't help in any way with its dependencies. For some reason, even if all the dependencies are in the same directory as the DLL, that doesn't mean anything.

The solution was simple. I use Git Bash for my compilation, so I do a simple PATH export:

```
export PATH="$PATH:/c/Program Files/gretl"
```

In this case, it adds the Gretl DLL's directory to the PATH. To change it permanently for Git Bash, you can open .bashrc and add that line:

```
nano ~/.bashrc
```

You can obviously do that in other Windows terminals, but I stick with Git Bash due to its familiarity for a Linux user. The only thing that is potentially tricky is determining the location of all dependencies. That hasn't been an issue for me, but I suppose it could be if dependencies weren't handled properly when creating the DLL.