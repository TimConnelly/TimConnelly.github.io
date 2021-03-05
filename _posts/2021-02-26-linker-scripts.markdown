---
layout: post
title:  "Linker Scripts: What, Why and How"
date:   2021-02-25 01:27:23 +0000 # FIXME
categories: programming embedded C C++
---
I recently was asked if I write my own linker scripts. The answer "No, the default linker machinations of `ld` have yet to fail me."

But curiosity took hold and I need to answer the questions for myself. What exactly is a linker script? Why would you write a linker script or leaving the defaults? How do you write a linker script? I hope this will be a more succint answer than reading the `ld` manual, though it will have final say.

## What
In short, a linker script is file that describes to the linker, `ld` for the Gnu toolchain, how to organize the compiled output of individual source files into an executable. There is a default built into `ld` for when you do not provide your own. The `--verbose` option will print out the linker script. 

The term "script" is somewhat misleading here. A linker script is really a configuration file that passed to the linker that tells it how to put together your final executable. It's "configuration as code!"

- Organizes output of object files into the full executable.
- assemble the compiled object files and libraries
- indicate where it goes in memory


## Why 
> • You already have a linker script or command file<br>
> • It came with the development system you started on<br>
> • You need to change it to match your final system<br>

- Cross-compiling for embedded systems is a common use case where you will need to customize the executable for the target system.

- Production environment may be different than dev environment (kit, prototype, etc). 

[Source: TI Presentation on linker scripts](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=&ved=2ahUKEwiM0J2Fp43vAhXqct8KHe9VDjs4ChAWMAB6BAgCEAM&url=https%3A%2F%2Fe2e.ti.com%2Fcfs-file%2F__key%2Fcommunityserver-discussions-components-files%2F81%2FA-Primer-on-Linker-Scripts-and-Command-Files.pdf&usg=AOvVaw30C7lcZEAf4QHXMii-cSde)

[Companion TI article](http://software-dl.ti.com/ccs/esd/documents/sdto_cgt_Linker-Command-File-Primer.html)

So in essence, the linker (`ld`) has a default behavoir. The default may not be appropriate for your target environment, in which case you will need to write your own linker script. 

## **Tools**
1. `ld` - The linker for Gnu tools.
1. `objdump` - Show information in object files
1. `nm` - List symbols in a file (Can also use `objdump -t`)
1. `readelf` - Does what it the name says.

## **Reference**
[ld man pages](https://man7.org/linux/man-pages/man8/ld.so.8.html)

[RHEL Reference](http://web.mit.edu/rhel-doc/3/rhel-ld-en-3/scripts.html)

[ELF Specification (PDF)](refspecs.linuxbase.org/elf/elf.pdf)