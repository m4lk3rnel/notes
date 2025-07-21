### TTY
- Teletype or teletypewriter, is basically a really old keyboard that was used to interact with a device (e.g. mainframe, servers). No digital screen. It was using paper. 
- ![[tty_(teletype).png]]
- Modern TTY's are **virtualized** (`/dev/tty0`, `/dev/tty1`, etc..)
- "Simulates the **low-level text input/output** between user and system." - chatgpt
- on Linux (`CTRL + SHIFT + F1` to `F6`) -> text-based consoles.
- you can use `aggety` in a text-based console.

### PTY (pseudo-TTY)
- a virtual terminal (software-created) that is used by programs like `tmux`, `ssh`. "fake" terminal.
- `ssh`, `tmux` or any **Terminal emulator** like (GNOME terminal) interact with the **master** side of the **PTY**. The **slave** side is the terminal that run shells like `bash` and send/receive input/output.  
	- `user → ssh / tmux (PTY master) ←→ PTY slave ←→ bash / shell`
- PTY's are character devices. (check [[Devices in Linux]])
- you can use the `tty` command to see which PTY you're using.
	- you can spawn several `tmux` panes and use `tty` to see how each pane has a different PTY allocated.


- `dmesg | grep tty`
- `agetty`

