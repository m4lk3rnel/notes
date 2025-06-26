
- https://www.youtube.com/watch?v=9DSSyffnMn0
- back in the day a COM to RJ45 cable was used to connect to the Switch Console. Laptops nowadays do **not** have a COM port. You would need a USB to COM adapter. You can then use PuTTY to connect to the Switch console.
- If the router doesn't have a RJ45 port, you can use a Micro USB to USB cable.

### Switch configuration
- you can press tab for autocompletion.
- `Switch>` - the `>` indicates that we are in **User mode**. In User mode you have limited privileges and you can't configure the Switch.
- `enable` - command used to switch to **Enable mode**. You can configure the Switch in **enable mode**. Packet tracer doesn't prompt you for a password but a real switch will. You can also use `en` instead of writing the whole command.
- `disable` - go back to **User mode**.
- the `?` command can be used to see a list of commands. You can use `Space` to see a **page** of commands or `Enter` to go one line at a time.
- if a command doesn't work check the mode you're in.
- "Learn to be lazy" - use autocompletion.
- `conf(igure) t(erminal)`