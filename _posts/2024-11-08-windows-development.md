---
title: My workflow for developing on Windows
layout: post
---
I don't often work on Windows, but there are a couple of times that I need to do so:

- If I want to share a program I've written with a Windows user. I obviously have to get it to compile on Windows.
- If I only have access to a Windows machine. I have a Windows laptop provided by my employer and I use Windows for various administrative tasks. I could use Linux, but Microsoft has set it up so that it's less convenient to do so. Since it's for work, I just go ahead and use Windows for those tasks, and I use Linux for my own stuff. Once my administrative position comes to an end, there will be less reason to use Windows.

Windows used to have a terrible experience for software development. Unless you were using a particular set of languages with Visual Studio, which was never my case, it really sucked. It has improved considerably over the years. The final piece that made things workable was finding a useful file manager application. The default Windows file manager is basically just a scam to get users to upload their documents to OneDrive.

I should also mention WSL. It's a wonderful option in 2024. Still not the Linux development experience, but not so far behind. WSL is easy to install. If you're willing to work with Visual Studio and use its console, which is not necessarily everyone's first choice, it's a usable experience. The biggest limitation of WSL is that the Windows users you work with are probably not using WSL.

Here are the tools I have installed on Windows 11 to make the development experience tolerable. I'll update the list as more things come to mind.

- Files App. You can download it from Github or from the Windows store.
- Windows Terminal. Available in the Windows store.
- Git Bash. Bring the Linux terminal experience (with limitations) to Windows. You can set Bash aliases as on Linux. That's pretty important because you don't need to mess around with setting PATH on Windows.
- Geany. My editor of choice for programming.
- RTools. For compiling R packages on Windows. Unlike the old days, it's very easy to set up, and when combined with the devtools package, compiling packages with source code on Windows is straightforward.