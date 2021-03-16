---
layout: post
title:  "Linker Scripts: How to write a basic linker script"
date:   2021-03-16 08:57:00 +0000 
categories: programming embedded C C++
---

## How

In my [previous post](https://timconnelly.github.io/programming/embedded/c/c++/2021/03/05/linker-scripts.html) I talked about what a linker script does and why you would write one. Now I will cover the basics of writing a linker script. This was largely a learning experience before, as I have not much a need in the past to dig into linker scripts.

The primary components of linker scripts are commands. I will describe only the SECTIONS and MEMORY commands in this post. 

### SECTIONS
`SECTIONS` is the basic command in a linker script. This is where you will describe to the linker how you want your inputs (object files, data) organized. You may omit the the SECTIONS command, in which case the linker will place all inputs in the output, starting at address 0, in the order it reads them.

`SECTIONS` command will specify output section descriptions for various inputs. Here is the full specification for sections. The parts in square brackets are optional. 
```ld
section-name [address] [(type)] : [AT(lma)]
  {
    output-section-command
    output-section-command
    …
  } [>region] [AT>lma_region] [:phdr :phdr …] [=fillexp]
```

The section names must comply with the output format. The specification for the Executable and Linkable Format (ELF) is listed in the references below. In our examples, `.text` describes code inputs and outputs.

The linker maintains a counter for memory location of sections. So you do not have to be explicit about output section locations. The '.' represents the memory location counter and it can be set explicitly to an address, for example `. = 0x20000000`. However, it is most common to specify a memory region by name- more on this later. You may also specify a memory alignment with the `ALIGN()` attribute. 

The output command describes what should be placed into the section. It can be a file name, a specific section of a file, or make use of patterns to match parts of files. For example, *(.text) describes the the .text part of any input. using the * wildcard. 

```ld
SECTIONS
{
    . = 0x20000000
    .bss : { *(.bss*) }
    . = 0x8000000
    .text : { *(.text*) } 
    .rodata : { *(.rodata*) }
}
```

### MEMORY
For easier reading, it's convention to use the `MEMORY` command to define your memory sections. `MEMORY` tells the linker which sections of memory it may use. These numbers will vary between MCUs and designs. For the STM32F0 sitting on my desk, RAM starts at 0x20000000 and is 16K long. The MEMORY definition command for this is `RAM (xrw) : ORIGIN = 0x20000000, LENGTH = 16K`.

The name RAM is not a keyword. It could be any name, but RAM is a good and descriptive name. ROM and flash are common names for non-volatile memory. The `ORIGIN` and `LENGTH` keywords must be present to define the memory region start and size. These may be abbreviated to `org` or `o` and `len` or `l`. You may provide attributes (rwx) for a memory segment, but these are merely documentation. They do not change the output. Note that if the outputs for a region exceed the length, then the linker will emit an error and stop. 

To place an output section into a memory region, the syntax is `> region name`. So, with RAM defined by the MEMORY command, we can do `.bss : { *(.bss*) } > RAM`. Adapting the example from above to use `MEMORY` will give us:

```ld
MEMORY
{
  RAM    (xrw)    : ORIGIN = 0x20000000,   LENGTH = 16K
  FLASH    (rx)    : ORIGIN = 0x8000000,   LENGTH = 128K
}

SECTIONS
{
    .text : { *(.text*) } > FLASH
    .rodata : { *(.rodata*) } > FLASH
    .bss : { *(.bss*) } > RAM
}
```

That wraps up the complete, basic example of a linker script. They will get much more complicated than this, go look at the output of `ld` with the `-M --verbose` options! To dig deeper, I recommend the RHEL REference [1].

## **Reference**
[1][RHEL Reference](http://web.mit.edu/rhel-doc/3/rhel-ld-en-3/scripts.html)

[2][ld man pages](https://man7.org/linux/man-pages/man8/ld.so.8.html)

[3][ELF Specification (PDF)](refspecs.linuxbase.org/elf/elf.pdf)

[4][Memfault's Zero to Main() series](https://interrupt.memfault.com/blog/how-to-write-linker-scripts-for-firmware)
