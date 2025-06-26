- the terminal multiplexer lets you run simultaneous tasks incredibly easy.
- ![[tmux_cheatsheet.png]]
- **IppSec**'s video on **tmux**: https://www.youtube.com/watch?v=Lqehvpe_djs
###### Sessions
- `tmux` - start a new session without a custom name
- `CTRL+B d` - detach from current session
- `tmux ls` - list all sessions
- `tmux new -s <session name>` - create a new session with a custom name.
- `tmux kill-session -t <session name>` - kill a session with the specified name.
- `tmux a -t <session name>` - attach to the session with the specified name.
- `CTRL+B )` - next session
- `CTRL+B (` - previous session

- add the following line to your `~/.tmux.conf`:
	- `setw -g mode-keys vi`
- You can now use vim keys to navigate better in copy mode!

###### Windows
- `CTRL+B c` - create a new window
- `CTRL+B &` - kill window
- `CTRL+B n` - move to next window
- `CTRL+B p` - move to the previous window
- `CTRL+B [` - enter copy mode
- `CTRL+B ]` - paste from buffer
- press `q` to quit copy mode
- `CTRL+B w` - based command, easier way to switch between windows.

###### Panes
- `CTRL+B %` - split the window vertically (now you have two panes)
- `CTRL+B "` - split the window horizontally
- `CTRL+B x` - kill current pane
- `CTRL+B o` - go to next pane
- `CTRL+B }` - move pane right
- `CTRL+B {` - move pane left
- `CTRL+B !` - convert pane to window
- `CTRL+B <down arrow>` - move down to pane
- `CTRL+B <right arrow>` - move to pane to the right
- `CTRL+B <left arrow>` - move to pane to the left
- `CTRL+B <up arrow>` - move up to pane
- hold down `CTRL+B` and use the arrow keys to resize the pane.