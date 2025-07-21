- `0046999F` - address function
- `0049C000` - money related function
- memory addresses: https://gtamods.com/wiki/Memory_Addresses_(SA)
- function memory addresses: https://gtamods.com/wiki/Function_Memory_Addresses_(SA)#Vehicles

### Hooking IDirect3DDevice9::EndScene

- you can use the [**vtable**](https://en.wikipedia.org/wiki/Virtual_method_table) (**virtual table**) which is an **array** of **function pointers** of an [COM object](https://en.wikipedia.org/wiki/Component_Object_Model).
- hooking techniques:
	- **vtable hook**: you replace the **function pointer of the original function** with the **function pointer** of **your** function. In your function then you call the original function. I think this only works for [COM objects](https://en.wikipedia.org/wiki/Component_Object_Model) 
	- **trampoline hook**: you replace the **code inside the original function** (initial bytes) and create a jump (**JMP**) to your function and then return to the original function.
	- you can also use libraries like [MinHook](https://github.com/TsudaKageyu/minhook) or [Microsoft Detours](https://en.wikipedia.org/wiki/Microsoft_Detours)
- [find the function address of EndScene using a scanning function](https://stackoverflow.com/questions/51875901/hooking-detouring-d3d9-present-endscene-seems-to-call-my-function-then-crash)
- if the **EndScene** was **exported** in *d3d9.dll* you could've get the address of the function using: 
```cpp
FARPROC func = GetProcAddress(LoadLibrary("d3d9.dll"), "EndScene");
```
- but it is not exported, so to get the function address the **vtable** is used.
- https://www.codereversing.com/archives/282 explains how you can get the the address of the function using the [windows debugging APIs](https://learn.microsoft.com/en-us/windows/win32/debug/debugging-functions), but i'm not sure if it works, because, again, the EndScene function is not exported (or exposed). 
- the index of EndScene in the interface's vtable is **42**.
- for the **vtable** and **trampoline** hook techniques you need a [dummy device](https://learn.microsoft.com/en-us/windows/win32/api/d3d9/nf-d3d9-idirect3d9-createdevice). 
- get a pointer the game's device -> hook the EndScene method using the vtable. 
- found pointer to `IDirect3DDevice9`: `0xC97C28`, source: https://gtamods.com/wiki/Memory_Addresses_(SA)
### ShowCursor
- `0x7481CD` -> `6A 00` -> two bytes
- `0x7481CF` -> `FF 15 EC828500` -> 6 bytes

### SetCursorPos
- `0x74542B` -> `50` -> one byte
- `0x74542C` -> `51` -> one byte
- `0x74542D` -> `FF 15 00838500` -> 6 bytes

### ~~Problem~~
- ~~I want to hook `GetDeviceState` or use, so I can intercept the mouse input and block it whenever I want to interact with the menu.~~
- ~~I could also use `DirectInput8Device::Unacquire()` or `DirectInput8Device::Acquire()`~~
### My options
- ~~Find some way to hook `GetDeviceState` without relying on `DirectInput8Create`.~~
	- ~~You must search for the pointer to the game's `DirectInput8Device` to use `Unacquire()` or `Acquire()`~~
	- ~~when you press Escape, the game "frees" the mouse. Maybe you can find the mechanism that does this by reverse engineering the game.~~
- ~~If I want to hook `DirectInput8Create`, I need to:~~
	- ~~Create a proxy DLL that forwards everything to the real `dinput8.dll`~~
	- ~~Use a launcher that starts your game with your DLL injected.~~
	- ~~You can maybe use [ASI Loader](https://github.com/ThirteenAG/Ultimate-ASI-Loader)~~

- found addresses for "freeing" the mouse from the center at: https://www.blast.hk/threads/10970/page-5
- i couldn't create a "launcher", because it was being flagged as malware by the system, lol.

- Really interesting, SA:MP **might** use the `IDirect8Device::Unaquire()` function to free the mouse.  

- try the dll proxy technique again. Maybe [MinimalDInput8Hook](https://github.com/pampersrocker/DInput8HookingExample/blob/master/MinimalDInput8Hook/MinimalDInput8Hook.cpp) can help.
- dinput8.dll proxy: https://github.com/Jessyy/gtasa-dll-windowmode-mousefix/blob/master/DINPUT8.cpp
- 
- look more into https://github.com/ThirteenAG/Ultimate-ASI-Loader/blob/master/source/dllmain.cpp

---
- ASI loader launches the game with the DLL (`.asi`) injected which is really cool.
- Hook `DirectInput8Create` -> `IDirectInput8 Interface` -> `IDirectInput8::CreateDevice` -> `IDirectInputDevice8 Interface` -> `IDirectInputDevice8::Acquire` . 
---

- a **function pointer** is a pointer that stores the address of a function.
- to read: https://www.codeproject.com/Articles/14040/Hooking-a-DirectX-COM-Interface
- trampoline hook (inline function hooking): https://www.lrqa.com/en/insights/articles/windows-inline-function-hooking/