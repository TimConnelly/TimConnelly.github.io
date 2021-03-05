---
layout: post
title:  "Linker Scripts: What, Why and How"
date:   2021-02-25 01:27:23 +0000 # FIXME
categories: programming embedded C C++
---
A few weeks ago, I was asked if I write my own linker scripts. The answer "No, the default machinations of the linker have yet to fail me."

But curiosity took hold and I need to answer the questions for myself. What exactly is a linker script? Why would you write a linker script or leaving the defaults? How do you write a linker script? I hope this will be a more succinct answer than reading the `ld` manual, though it will have final say.

## What
In short, a linker script is a set of commands that describes to the linker, `ld` for the Gnu toolchain, how to organize the compiled output of source files, libraries and data into an executable. There is a default script built into `ld` for when you do not provide your own. The `--verbose` option will print out the linker script. 

I find the term "script" is somewhat misleading here. A linker script is really a set of configuration commands given to the linker program. It's configuration-as-code!

- Organizes output of object files into the full executable.
- assemble the compiled object files and libraries
- indicate where it goes in memory


## Why 
Information on what linker scripts are and how to write them is easy to find. The motivating need to write your own linker script is not so easy to answer. After quite a bit of reading, here's my answer: When you cross-compile code, the default linker behavior may not me appropriate for your target system. 

This scenario is most common in embedded systems. Not only will the target execution environment be different from the host, you may have variations of your target system (dev kit, prototype, production). 

That's the basics of linker scripts. There's much more to the topic, which I plan to dig into with a future post. In the meantime, the references below are a good place to start.

## Tools
`ld` - The linker for Gnu tools.

`objdump` - Show information in object files.

`nm` - List symbols in a file (Can also use `objdump -t`).

`readelf` - Reads and outputs information from an ELF format binary.

## Reference
[TI Presentation on linker scripts (pdf)](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=&ved=2ahUKEwiM0J2Fp43vAhXqct8KHe9VDjs4ChAWMAB6BAgCEAM&url=https%3A%2F%2Fe2e.ti.com%2Fcfs-file%2F__key%2Fcommunityserver-discussions-components-files%2F81%2FA-Primer-on-Linker-Scripts-and-Command-Files.pdf&usg=AOvVaw30C7lcZEAf4QHXMii-cSde)

[TI Primer on linker scripts](http://software-dl.ti.com/ccs/esd/documents/sdto_cgt_Linker-Command-File-Primer.html)

[ld man pages](https://man7.org/linux/man-pages/man8/ld.so.8.html)

[RHEL ld Reference](http://web.mit.edu/rhel-doc/3/rhel-ld-en-3/scripts.html)

[ELF Specification (PDF)](refspecs.linuxbase.org/elf/elf.pdf)