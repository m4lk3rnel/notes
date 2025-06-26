-  [[Metasploit]] is an exploitation framework. 
-  Versions:
	-  [[Metasploit]] **Pro**: This version has a **GUI** (**Graphical User Interface**)
	-  [[Metasploit]] **Framework**: Open-Source, has a **CLI** (**Command-Line Interface**).
- Components: 
	-  **msfconsole**: The main command-line interface.
	-  **Modules**: supporting modules such as exploits, scanners, payloads, etc.
	-  **Tools**: tools that will help vulnerability research, vulnerability assessment, or penetration testing. Some of these tools are **msfvenom**, **pattern_create** and **pattern_offset**.

- **Exploit:** A piece of code that uses a vulnerability present on the target system.
- **Vulnerability:** A design, coding, or logic flaw affecting the target system. The exploitation of a vulnerability can result in disclosing confidential information or allowing the attacker to execute code on the target system.
- **Payload:** An exploit will take advantage of a vulnerability. However, if we want the exploit to have the result we want (gaining access to the target system, read confidential information, etc.), we need to use a payload. Payloads are the code that will run on the target system.

- `sudo apt upgrade`, `sudo apt install metasploit-framework` - `msfupdate` is no longer supported anymore.
- `/usr/share/metasploit-framework/modules`

###### Encoders 
- `/usr/share/metasploit-framework/modules/encoders`
- encode the payload to avoid getting detected by **signature-based AV** (cool)
- "Signature-based antivirus and security solutions have a database of known threats. They detect threats by comparing suspicious files to this database and raise an alert if there is a match. Thus encoders can have a limited success rate as antivirus solutions can perform additional checks."

##### Evasion
- `/usr/share/metasploit-framework/modules/evasion`
- "While encoders will encode the payload, they should not be considered a direct attempt to evade antivirus software. On the other hand, “evasion” modules will try that, with more or less success."

###### Exploits
- `/usr/share/metasploit-framework/modules/exploits`

###### NOPs (No OPeration)
- `/usr/share/metasploit-framework/modules/nops`
- "NOPs (No OPeration) do nothing, literally. They are represented in the Intel x86 CPU family with 0x90, following which the CPU will do nothing for one cycle. They are often used as a buffer to achieve consistent payload sizes."

###### Payloads
-  `/usr/share/metasploit-framework/modules/payloads`
-  "Exploits will leverage a vulnerability on the target system, but to achieve the desired result, we will need a payload. Examples could be; getting a shell, loading a malware or backdoor to the target system, running a command, or launching calc.exe as a proof of concept to add to the penetration test report. Starting the calculator on the target system remotely by launching the calc.exe application is a benign way to show that we can run commands on the target system."
- Metasploit has a subtle way to help you identify single (also called “inline”) payloads and staged payloads.
	- generic/shell_reverse_tcp
	- windows/x64/shell/reverse_tcp

	- Both are reverse Windows shells. The former is an inline (or single) payload, as indicated by the “_” between “shell” and “reverse”. While the latter is a staged payload, as indicated by the “/” between “shell” and “reverse”.

###### Post
-  `/usr/share/metasploit-framework/modules/post`
-  post-exploitation

###### msfconsole
- `msfconsole`
-  The [[Metasploit]] console can be used just like a regular command-line shell.
-  It supports most Linux commands, including `clear`
-  The `help` command can be used on its own or for a specific command.
-  You can use `history` to see commands you have typed earlier.
-  It has the tab completion feature.
-  `use /exploit/windows/smb/ms17_010_eternalblue`
-  you can still run Linux commands even though we set the "context" in which we will work.
-  whenever the "context" is set you can use `show options`.
-  `show exploits`, `show payloads`, `show <module>`
-  leave the context with `back`
-  when in context: `info`
	- `info exploit/windows/smb/ms17_010_eternalblue`
-  `search <keyword>`, example: `search reverse` (search the [[Metasploit]] database for modules, you can use **CVE** numbers, exploit names like `eternalblue`, or `heartbleed`, etc.., or target system)
	-  after searching, you can use `use <number_of_the_result_line>` or `info <number_of_the_result_line>`
	-  "Another essential piece of information returned is in the “rank” column. Exploits are rated based on their reliability. The table below provides their respective descriptions."
	![[metasploit_exploit_ranks.png]]
	- `search type:auxiliary telnet`
- [[Meterpreter]] prompt: `meterpreter >`
- `set <parameter> <value>` - set a parameter's value, example `set RPORT 8080` 
-  you can have a range of [[IP (internet protocol)]] addresses and set them as a value for the `RHOSTS` parameter, after saved in a file (`set rhosts file:/path/to/file.txt`)
- `unset <parameter>` or `unset all`
- `setg` - you can use this command to set a value for a parameter that will be used in all modules.
- `unsetg`
- you can use `run` which is an alias for the `exploit` command.
- `exploit -z` - run the exploit and background the session as soon as it opens.
- some modules support the command `check` used to check if the target system is vulnerable without exploiting it.

###### Sesssions
- use `background` to background the session prompt and go back to the `msfconsole` prompt. You can also use `CTRL+Z`.
- `sessions` - display all the existing sessions.
- To interact with any session: `sessions -i <session_number>`