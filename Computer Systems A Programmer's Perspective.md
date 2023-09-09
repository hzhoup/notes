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
- Optimizing program performance
- Understanding link-time errors
- Avoiding security holes

At this point, our `hello.c` source program has been translated by the compilation system into an executable object file called hello that is stored on disk. To run the executable file on the Unix system:

```shell
./hello
hello, world
```

![[Pasted image 20230909160214.png]]

Hardware organization of a system:
- *Buses*. Running throughout the system is a collection of electrical conduits called buses that carry bytes of information back and forth between the components.
- *I/O Devices*. Input/output devices are the system's connection to the external world. Each I/O device is connected to the I/O bus by either a *controller* or an *adapter*.
- *Main Memory*. The main memory is a temporary storage device that holds both a program and the data it manipulates while the processor is executing the program. Physically, main memory consists of a collection of *dynamic random access memory* chips. Logically, memory is organized as a linear array of bytes, each with its own unique address starting at zero.
- *Processor*. The *central processing unit* (CPU), or simply processor, is the engine that interprets (or executes) instructions stored in main memory. At its core is a word-size storage device (or register) called the *program counter* (PC). At any point in time, the PC points at some machine-language instruction in main memory.

Running the hello program:
1. ![[Pasted image 20230909161453.png]]
2. ![[Pasted image 20230909161518.png]]
3. ![[Pasted image 20230909161532.png]]

Caches Matter: ![[Pasted image 20230909162310.png]]

Storage Devices Form a Hierarchy: ![[Pasted image 20230909162414.png]]

The operating system has two primary purposes: (1) to protect the hardware from misuse by runaway applications and (2) to provide applications with simple and uniform mechanisms for manipulating complicated and often wildly different low-level hardware devices.

Layered view of a computer system: ![[Pasted image 20230909162937.png]]

Abstractions provided by an operating system: ![[Pasted image 20230909163456.png]]

A *process* is the operating system's abstraction for a running program. Multiple processes can run concurrently on the same system, and each process appears to have exclusive use of the hardware. By *concurrently*, we mean that the instructions of one process are are interleaved with the instructions of another process. A single CPU can appear to execute multiple processes concurrently by having the processor switch among them. The operating system performs this interleaving with a mechanism known as *context switching*.

Process context switching: ![[Pasted image 20230909164327.png]]

In modern systems a process can actually consist of multiple execution units, called *threads*, each running in the context of the process and sharing the same code and global data.

*Virtual memory* is an abstraction that provides each process with the illusion that it has exclusive use of the main memory. Each process has the same uniform view of memory, which is known as its *virtual address space*.

Process virtual address space: ![[Pasted image 20230909164732.png]]

Starting with the lowest addresses and working our way up:
- *Program code and data*
- *Heap*
- *Shared libraries*
- *Stack*
- *Kernel virtual memory*

A network is another I/O device: ![[Pasted image 20230909165239.png]]

```ad-note

The *Amdahl's law* main idea is that when we speed up one part of a system, the effect on the overall system performance depends on both how significant this part was and how much it sped up.

Consider a system in which executing some application requires time $T_{old}$. Suppose some part of the system requires a fraction $\alpha$ of this time, and that we improve its performance by a factor of $k$. That is, the component originally required time $\alpha T_{old}$, and it now requires time $\frac{\alpha T_{old}}{k}$. The overall execution time would thus be

$$$
T_{new} = (1 - \alpha)T_{old} + \frac{\alpha T_{old}}{k}
$$$

```

