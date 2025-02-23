# Memory layout of C programs

memory is split to the following sections:
1. text segement aka Instructions aka code
2. Initialized data segment
3. Uninitialized data segment (bss)
4. Heap
5. Stack
![[Pasted image 20241121133314.png]]as you can see the following image gives us a good representation of the memory but what does each part do we will get to it in a bit.
this is a typical memory of a running program.
## Text segment
also known as a code segment or simply as text, is one of the sections of a program's memory which contains executable instructions. as a memory region, a text segment may be placed below the heap or stack in order to prevent heaps and stack overflows from overwriting it.

usually the text segment is shareable so that only a single copy needs to be in memory for frequently executed programs, such as text editors, the C compiler, the shells and so on. Also, the text segment is often read-only, to prevent a program from accidentally modifying its instructions.

## Initialized Data Segment
Initialized data segment, usually called simply the Data segment. A data segment is a portion of the virtual address space of a program, which contains the global variables and static variables that are initialized by the programmer. ![[Pasted image 20241121151511.png]]
Ex: static int i = 10 will be stored in the data segment and global int i = 10 will also be stored in data segment
## Uninitialized Data Segment `bss`
often called "bss" segment, named after an ancient assembler operator that stood for "block started by symbol" Data in this segment is initialized by the compiler to arithmetic 0 before the program starts, so basically if you declare a variable it will be here or if you give it a value of zero until you change that value or set another value and so on
## Stack
so this is kinda of where when you call a recursive function it is pushed.

also the stack area traditionally adjoined the heap area and grew in the opposite direction; when the stack pointer met the heap pointer, free memory was exhausted *with modern large address spaces and virtual memory techniques they may be placed almost anywhere, but they still typically grow in opposite directions* 

the stack area contains the program stack, a LIFO structure *Last in First out* typically located in the higher parts of memory on the standard pc x86 computer architecture, it grows towards address zero, on some other architectures it grows on opposite direction, a stack pointer register tracks the top of the stack; it is adjust each time a value is pushed onto the stack the set of values pushed for one function call is termed "stack frame" a stack frame consists at minimum of a return address
it is where automatic variables are stored along with information that is saved each time a function is called
## heap
heap is the segment where dynamic memory allocation usually takes place. the heap area begins at the end of the BSS segment and grows to larger addresses from there the heap area is managed by malloc, realloc and free.
the heap area is shared by all shared libraries and dynamically loaded modules in a process.

## understanding more
so basically we have a basic program
```
int main()
{
	return 0;
}
```
if we `cc test.c` and the run `size a.out` we will get the following output
![[Pasted image 20241121151620.png]]
but if we add a static int variable in our code and set the value to 100 we can see that dec goes up![[Pasted image 20241121151854.png]]
but if we give no value to i it will go to bss 
```c
#include <stdio.h>

int global; /* Uninitialized variable stored in bss*/

int main(void)
{
    return 0;
}
```

```c
#include <stdio.h>

int global; /* Uninitialized variable stored in bss*/

int main(void)
{
    static int i; /* Uninitialized static variable stored in bss */
    return 0;
}
```

```c
#include <stdio.h>

int global; /* Uninitialized variable stored in bss*/

int main(void)
{
    static int i = 100; /* Initialized static variable stored in DS*/
    return 0;
}
```

```c
#include <stdio.h>

int global = 10; /* initialized global variable stored in DS*/

int main(void)
{
    static int i = 100; /* Initialized static variable stored in DS*/
    return 0;
}
```