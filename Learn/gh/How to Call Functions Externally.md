-  https://www.youtube.com/watch?v=A4UVtu-LDpE
-  each **process** has its own memory allocated and isolated by the **operating system**.
-  **0x00000000** -> **0xFFFFFFFF**

### Process virtual memory layout
-  it starts with the code segment (**.text**). It is marked with "**read-only**".
	-  You can change it to **read and write** with [VirtualProtect](https://learn.microsoft.com/en-us/windows/win32/api/memoryapi/nf-memoryapi-virtualprotect)
	-  contains all the executable **instructions** of the program.
- after .text -> the **.data** and **.bss** store variables and globals.
	- **.bss** holds **uninitalized** variables. (like **int i;**)
	- **.data** holds **initialized** variables. (like **int i = 10;**)
	- the size of these two segments is calculated on compilation.
- after .data and .bss -> **Heap** memory
	- the **heap** memory can grow dynamically (in runtime)
	- [malloc](https://en.cppreference.com/w/c/memory/malloc) uses the **heap** memory.
	- "**every process does have access to heap memory**—it's part of the process's virtual memory layout." via ChatGPT
	- ![[heap_memory_process_virtual_memory_layout.png]]
	- `int* a = new Int;` - C++

- after Heap -> **Stack** memory
	- each thread has its own stack.
	- function arguments
	- local variables
	- return address (where to go back after function ends)
	- interesting (via ChatGPT):
		-  **stack overflow**: happens when the stack runs out of space, usually due to deep or infinite recursion.
		-  **buffer overflow**: happens when a local buffer: `char buf[10]` is written beyond its limits, possibly overwriting return addresses. Often exploited in security attacks.
	- the **stack** memory is located to the top of the virtual memory and grows downwards.
	- register for the stack memory (on the x86 architecture): 
		- **ESP** -> stack pointer, always points to the top of the stack and moves downwards as the data is pushed into it