- https://www.unknowncheats.me/forum/arma-2-a/175227-driver-disable-process-thread-object-callbacks.html

- BattlEye is a **kernel-level anti-cheat** system.

### Thread Object Callbacks
- mechanism used to monitor the **behavior** (creation, modification) of **threads** in a **game process**.
- whenever a **thread** is accessed, created, duplicated, terminated, the **callback** gets called.

### OBJECT_TYPE
- internal windows structure.
- it holds metadata about **processes** (**PsProcessType**), **threads** (**PsThreadType**), **files**, etc..
- this structure has a field called `CallbackList`
	- `ObRegisterCallbacks` - can be used to register callbacks that get added to `CallbackList`
	- **linked list** of **callbacks** that are called whenever an operation is being done on the **OBJECT_TYPE**.
	- each entry in the list:
		- has an `altitude string` that tells windows the order of the callbacks
		- has **pointers** to `PreOperation` and `PostOperation`. The code provided in the post **overwrites** the **function pointers** to point to **dummy** functions.