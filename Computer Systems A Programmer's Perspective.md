## A Tour of Computer Systems

A *Computer system* consists of hardware and systems software that work together to run application programs.

```c
#include <stdio.h>

int main()
{
    printf("hello, world\n");
    return 0;
}
```

The hello program begins life as a *source program* (or *source file*) that the programmer creates with an editor and saves in a text file called `htllo.c`. The source program is a sequence of bits, each with a value of 0 or 1, organized in 8-bit chunks called *bytes*. Each byte represents some text character in the program.

The source program is stored in a file as a sequence of bytes. Each byte has an integer value that corresponds to some character. Files such as `hello.c` that consist exclusively of ASCII characters are known as *text files*. All other files are known as *binary files*.

```ad-note
All information in a system is represented as a bunch of bits. The only thing that distinguishes different data objects is the context in which we view them.
```

The hello program begins life as a high-level C program because it can be read and understood by human beings in that form. However, in order to run `hello.c` on the system, the individual C statements must be translated by other programs into a dequence of low-level *machine-language* instructions. These instructions are then packaged in a form called an *executable object program* and stored as a binary disk file. Object programs are also referred to as *executable object files*.

```ad-note
On a Unix system, the translation from source file to object file is performed by a compiler driver:

`gcc -o hello hello.c`

![[Pasted image 20230909143555.png]]
```

*Compilation System*:
- *Preprocessing phase*. The preprocessor (cpp) modifies the original C program according to directives that begin with the # character. typically with the `.i` suffix.
- *Compilation phase*. The compiler (cc1) translates the file `hello.i` into the text file `hello.s`, which contains an *assembly-language program*.
- *Assembly phase*. The assembler (as) translates `hello.s` into machine-language instructions, packages them in a form know as a *relocatable object program*, and stores the result in the object file `hello.o`.
- *Linking phase*. For standard library functions there must be some way to merge them into `hello.o` program, the linker (ld) handles this merging. The result is the hello file, which is an executable object file (or *executable*) that is ready to be loaded into memory and executed by the system.

There are some important reasons why programmers need to understand how compilation systems work: