- a **thread** is the smallest unit of execution in a program. A thread is a single flow of instructions inside that program. via ChatGPT

## in game cheat development

- cheat starts in its own **process**
- it opens a handle of the game process (`OpenProcess`)
- it allocates memory inside the game's process (`VirtualAllocEx`)
- it writes shellcode or a DLL path into the game's memory (`WriteProcessMemory`)
- it creates a new **thread** inside the game (`CreateRemoteThread`) that starts executing that code.


- It can bypass some protection. The game runs the injected code as if it were its own.
- Lets you call game functions directly: from inside the process, you can call internal functions, read game memory.
- Works for DLL injection: often, cheats inject a DLL and call `LoadLibraryW` via a remote thread to load it into the target game process.