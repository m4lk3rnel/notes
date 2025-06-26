-  It is a [[Metasploit]] payload that can be used to interact with the target system and files.
-  [[Meterpreter]] runs in the memory of the target system (RAM - Random Access Memory). It is not installed on the disk, so it can be avoided by antivirus scans.
-  Aims to be avoided by [[IPS (Intrusion Prevention System)]] and [[IDS (Intrusion Detection System)]] by using encrypted communication with the server where [[Metasploit]] runs (typically your attacking machine).
- Can still be recognized by major antivirus software.
- `getpid` in a [[Meterpreter]] session to get the PID (Process ID).
- `ps` - process list 
- "It is also worth noting that [[Meterpreter]] will establish an encrypted (TLS) communication channel with the attacker's system."
- [[Meterpreter]] payloads are divided into two categories: inline (also called single) and staged.
	- The **staged payloads** are sent to the target in two steps. An initial part is installed (the stager) and requests the rest of the payload. This allows for a smaller initial payload size.
	- The **inline payloads** are sent in a single step.
- `msfvenom --list payloads | grep meterpreter`

- Your decision on which version of [[Meterpreter]] to use will be mostly based on three factors;
	- The target operating system (Is the target operating system Linux or Windows? Is it a Mac device? Is it an Android phone? etc.)
	- Components available on the target system (Is Python installed? Is this a [[cybersec/thm/PHP]] website? etc.)
	- Network connection types you can have with the target system (Do they allow raw [[TCP (transmission control protocol)]] connections? Can you only have an HTTPS reverse connection? Are IPv6 addresses not as closely monitored as IPv4 addresses? etc.)

###### Meterpreter commands
- `help`
- `background` - backgrounds the current session
- `exit` - Terminate the [[Meterpreter]] session
- `guid` - Get the session GUID (Globally Unique Identifier)
- `info` - Displays information about a Post module.
- `irb` - opens an interactive Ruby shell on the current session.
- `load` - Loads one or more [[Meterpreter]] extensions
- `migrate` - Allows you to migrate [[Meterpreter]] to another process
- `run` - Executes a [[Meterpreter]] script or Post module
- `sessions` - Quickly switch to another session
-  `sessions -u <session_id>` - stabilizes the session (`shell_to_meterpreter`)
File system commands
- `cd` - change directory
- `ls` - list all files in the current directory (`dir` will also work) 
- `pwd` - prints the current working directory.
- `edit` - will allow you to edit a file
- `cat` - show the contents of a file to the screen
- `rm` - delete the specified file
- `search` - search for files
- `upload` - upload a file or directory
- `download` - download a file or directory.

Networking commands
- `arp` - Displays the host [[ARP (Address Resolution Protocol)]] cache
- `ifconfig` - displays network interfaces available on the target system.
- `netstat` - displays the network connections.
- `portfwd` - forwards a local port to a remote service. (interesting)
	-  `portfwd add -l <local port> -p <remote port> -r <remote address>`
	- ![[port_forwarding_practical_example_chatgpt.png]]
- `route` - allows you to view and modify the routing table.
	 ![[route_command_chatgpt.png]]

System commands
- `clearev` - clears the event logs
- `execute` - executes a command
- `getpid` - shows the current process identifier
- `getuid` - shows the user that [[Meterpreter]] is running as
- `kill` - terminates a process
- `pkill` - terminates processes by name
- `ps` - lists running processes
- `reboot` - reboots the remote computer
- `shell` - drops into a system command shell
- `shutdown` - shuts down the remote computer.
- `sysinfo` - gets information about the remote system, such as [[OS (operating system)]]

Others Commands (these will be listed under different menu categories in the help menu)
- `idletime`: Returns the number of seconds the remote user has been idle
- `keyscan_dump`: Dumps the keystroke buffer
- `keyscan_start`: Starts capturing keystrokes
- `keyscan_stop`: Stops capturing keystrokes
- `screenshare`: Allows you to watch the remote user's desktop in real time
- `screenshot`: Grabs a screenshot of the interactive desktop
- `record_mic`: Records audio from the default microphone for X seconds
- `webcam_chat`: Starts a video chat
- `webcam_list`: Lists webcams
- `webcam_snap`: Takes a snapshot from the specified webcam
- `webcam_stream`: Plays a video stream from the specified webcam
- `getsystem`: Attempts to elevate your privilege to that of local system
- `hashdump`: Dumps the contents of the SAM database

###### Post-Exploitation with Meterpreter
- **Migrate**: you can migrate to another process and you can interact with it using [[Meterpreter]]
	- for example: if you see a word process running on the target (e.g. **word.exe**, **notepad.exe**, etc.), you can migrate to it and start capturing keystrokes sent by the user to this process (`keyscan_start`, `keyscan_stop`, `keyscan_dump`). [[Meterpreter]] will act as a **keylogger**.
	- syntax: `migrate <process-id>
- **Hashdump**: will list the content of the SAM (Security Account Manager) database (users and passwords of the system). These passwords are stored in the **NTLM** (New Technology LAN Manager) format.
- **Search**: locate files with potentially juicy information. Can be used to find configuration files or user-generated files that may contain passwords.
	-  example: `search -f flag2.txt`
- **Shell**: launch a regular command-line shell on the target system. `CTRL+Z` will help you go back to the [[Meterpreter]] shell.

- Post-exploitation phase **goals**:
	- Gathering further information about the target system.
	- Looking for interesting files, user credentials, additional network interfaces, and generally interesting information on the target system.
	- Privilege escalation.
	- Lateral movement.