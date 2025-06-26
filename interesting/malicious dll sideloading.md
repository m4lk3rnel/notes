https://www.twitch.tv/videos/2275175397
###### from this stream:
- search for an unsigned dll, preferably one that loads whenever the main program starts. You can analyze what dll loaded on runtime for a program using **Process Monitor**
- dnSpy was used for analyzing the **Greenshot** executable (which is a .NET executable)
- turn 'AutoloadStdApi' to false for meterpreter, is helps with evasion
- `WaitForSingleObject` is used to keep the program running. In this case, you wouldn't need it.
- use ports like `443` not `6969` for the shell (LPORT)