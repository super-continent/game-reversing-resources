# Game Hacking Resources
heres my attempt at a good list of reading material thats interesting/educational when learning to hack/mod games. ill try to separate it into categories for easy access. generally focuses specifically on fighting games, and has opinionated recommendations on tools/libraries.

## Contents
- [Starting Out](#starting-out)
- [The Tools](#the-tools)
- [Source Code](#source-code)
- [Interesting Blog Posts/Write-ups](#interesting-write-ups)
- [Libraries](#libraries)

## Starting Out
when learning to RE games its very important to understand the actual relation between higher-level languages and the assembly code thats output from them, i think that one of the major problems i had when starting out was misunderstanding how something like an injected DLL worked, and not having an idea of what a calling convention is, i hope to clear these up if you havent heard of them before. all mentioned pages are in the [Interesting Write-ups](#interesting-write-ups) section.

first do the cheat engine tutorial to learn how to find values in memory that seem interesting, then read some of the wiki tutorials on how x86 assembly works, this should give you a good enough idea of whats actually happening when compiled code is getting executed. once you have an understanding of basic assembly, you can learn about how a function hook works.

very important note about function hooking that isnt mentioned enough in most explanations ive seen is that function calls typically use specific calling conventions, when you dont understand these it makes no sense how a function can call another and completely understand where all arguments should be stored when calling it. calling conventions provide specific standards for where arguments should be stored and how values should be returned, as well as specifying whether the caller or callee should clean up the stack. ghidra can also automatically detect the calling convention of functions in certain cases.

these function hooks are the base of basically every PC game mod out there, it enables you to change basically anything about a game if you work hard enough at it, the way that you actually put the hooks into the game is through something like DLL injection, there are plenty of guides on how it works but essentially you are creating a new thread on the program that says "load in this dll" and once the DLL is loaded in it will automatically execute whatever is in a "DllMain" function, you usually want to create a new thread to run your initialization stuff outside of DllMain because of limitations to what you can safely do inside of DllMain.

## Interesting Write-ups
- [CE Auto Assembler Basics](https://wiki.cheatengine.org/index.php?title=Tutorials:Auto_Assembler:Basics) - good intro to x86 asm and CE auto assembler, invaluable knowledge when working on games with any debugger
- [x86 API Hooking Demystified](http://jbremer.org/x86-api-hooking-demystified/) - extremely good explanation of how a function hook actually works
- [x86 Calling Conventions](https://en.wikipedia.org/wiki/X86_calling_conventions) - wiki page explaining how calling conventions work and where they all store/return data
- [Caster - How it's done](https://web.archive.org/web/20180807055016/http://mizuumi.net/2010/11/08/caster-how-its-done/) - the only article ive found on caster developement, it doesnt really go in-depth on how to do much, but it gives a nice general overview that would be very helpful to understand when developing one (please do this for the games i like)
- [Understanding Game Time Revisited](https://walbourn.github.io/understanding-game-time-revisited/) - cool post explaining how a games timing loops work, apparently useful to understand for caster developement
- Various tutorials on [Guided Hacking](https://guidedhacking.com/) - i absolutely hate how the people on these forums communicate sometimes but they do write useful guides that help you understand certain concepts in game hacking so they get on the list.

## The Tools
- [Cheat Engine](https://cheatengine.org/) - this is THE game hacking tool, has a very good set of things to search and modify memory, can do lots of interesting stuff once you learn how to use it well, the tutorial teaches you a lot.
- [Ghidra](https://ghidra-sre.org/) - extremely good static analysis suite, using this in combination with cheat engine is very nice, has a good decompiler so you dont need to deal with perfectly understanding compiler-optimized asm

## Source Code
- [CCCaster](https://github.com/NotMadscientist/CCCaster/) - if youre reading this list you probably already know about this, absolutely amazing caster with netcode that feels like magic, a bit dated but still interesting to read
- [BBCF Improvement Mod GGPO](https://github.com/GrimFlash/BBCF-Improvement-Mod-GGPO) - does NOT currently implement netplay or any form of ggpo, but its an updated version of BBCFIM, useful reference for creating a fake DLL that wraps an original DLL, allowing you to run code within a game without needing to inject a dll or anything, just drop it in the game folder and the game will automatically load it in upon start.
- [gbvs-hook](https://github.com/super-continent/gbvs-hook) - my hook for Granblue Fantasy: Versus that enables extraction and loading of scripts at runtime, not the most usable thing and the code isnt great but i think it works as a decent enough reference for how to make an injected DLL in rust.
- [BBScript](https://github.com/super-continent/bbscript) - my script parser/rebuilder for ArcSys games using bbscript, i attempted to make the code as readable as i could, demonstrates how you might want to create your own ASM-ish language with a PEG when working on unknown script formats.
- [bbtools](https://github.com/dantarion/bbtools) - this is the original modding toolset for ArcSys games like Guilty Gear and Blazblue, however i frankly find the source code very hard to read, useful to see how you could parse those scripts in python though.

## Libraries
### C#
- [EasyHook](https://easyhook.github.io/) - very good function hooking library that allows communication between injected dll and a server through IPC, makes creating UIs for stuff very easy through C# forms shown server-side
- [MemorySharp](https://github.com/ZenLulz/MemorySharp) - very good memory hacking library

### Rust
- [Detour-rs](https://github.com/darfink/detour-rs) - very good function hooking library.
- [Chiter](https://github.com/FabianB1998/chiter-old) - memory hacking library, i recommend recreating stuff you need on your own, probably best to eliminate it as a dependency as it doesnt seem to be updated anymore.

### C++
- [Detours](https://github.com/Microsoft/Detours) - microsoft official function hooking library
- [Blackbone](https://github.com/DarthTon/Blackbone) - memory hacking library, has a lot of features.
- [Polyhook](https://github.com/stevemk14ebr/PolyHook) - very good function hooking library.

