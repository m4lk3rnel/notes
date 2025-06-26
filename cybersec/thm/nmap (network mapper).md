Tool used for network discovery. It also assists in the exploration of network hosts and services, providing information about open ports, operating systems, and other details.

[[nmap (network mapper)]] result states:
-  **Open**: a service is running on this port and is accessible.
-  **Closed**: no service is running on this port.
-  **Filtered**: [[nmap (network mapper)]] cannot determine if the port is open or closed because the port is not accessible (usually because of a firewall)
-  **Unfiltered:** cannot determine if the port is open or closed because even though the port is accessible.
-  **Open|Filtered:** cannot determine if the port is open or filtered.
-  **Closed|Filtered**: cannot determine if the port is closed or filtered.
###### Spoofing and Decoys
-  `nmap -S SPOOFED_IP TARGET_IP` - the packets for scanning will be sent from `SPOOFED_IP` 
-  `nmap -D 10.10.0.1,10.10.0.2,ME 10.10.92.198` - the scan will appear as coming from the [[IP (internet protocol)]] addresses `10.10.0.1` and `10.10.0.2`.
-  `nmap -D 10.10.0.1,10.10.0.2,RND,RND,ME 10.10.92.198` - the third and fourth [[IP (internet protocol)]] addresses will be assigned randomly. The fifth source will be the attacker's [[IP (internet protocol)]] address. 
-  `--spoof-mac SPOOFED_MAC` - used for spoofing your MAC address.
###### Fragmented Packets
 -  `-f` - used to fragment packets ([[IDS (Intrusion Detection System)]]). The IP data will be divided to 8 bytes or less.
 -  `-f -f` or `-ff` - data will be divided to 16 byte-fragments.
###### Idle/Zombie Scan
-  `nmap -sI ZOMBIE_IP 10.10.92.198` - requires an idle system (zombie) connected to the network you communicate with. [[nmap (network mapper)]] will send the packets from the idle system and checks if it receives any response. The [[IP (internet protocol)]] identification (IP ID) is used.
###### Getting more details
-  `--reason` - can be used for getting [[nmap (network mapper)]]'s reason and conclusion.
-  `-v` or `-vv` - verbose output.
- `-d` or `-dd` - for even more details.
###### Summary
| Port Scan Type                 | Example Command                                         |
| ------------------------------ | ------------------------------------------------------- |
| TCP Null Scan                  | `sudo nmap -sN 10.10.12.227`                            |
| TCP FIN Scan                   | `sudo nmap -sF 10.10.12.227`                            |
| TCP Xmas Scan                  | `sudo nmap -sX 10.10.12.227`                            |
| TCP Maimon Scan                | `sudo nmap -sM 10.10.12.227`                            |
| TCP ACK Scan                   | `sudo nmap -sA 10.10.12.227`                            |
| TCP Window Scan                | `sudo nmap -sW 10.10.12.227`                            |
| Custom TCP Scan                | `sudo nmap --scanflags URGACKPSHRSTSYNFIN 10.10.12.227` |
| Spoofed MAC Address            | `--spoof-mac SPOOFED_MAC`                               |
| Decoy Scan                     | `nmap -D DECOY_IP,ME 10.10.12.227`                      |
| Idle (Zombie) Scan             | `sudo nmap -sI ZOMBIE_IP 10.10.12.227`                  |
| Fragment IP data into 8 bytes  | `-f`                                                    |
| Fragment IP data into 16 bytes | `-ff`                                                   |

| Option     | Purpose                                                    |
| ---------- | ---------------------------------------------------------- |
| `--reason` | explains how [[nmap (network mapper)]] made its conclusion |
| `-v`       | verbose                                                    |
| `-vv`      | very verbose                                               |
| `-d`       | debugging                                                  |
| `-dd`      | more details of debugging                                  |
###### Service detection
 -  `nmap -sV 10.10.12.227` - [[nmap (network mapper)]] has to establish a TCP connection to gather info about running services (has to use the 3-way handshake) meaning that `-sS`(silent scan) and `-sV` cannot be used together.

###### [[OS (operating system)]] detection
-  `sudo nmap -sS -O 10.10.6.101` - [[nmap (network mapper)]] will try to guess the [[OS (operating system)]] the running service is running on. Sometimes cannot be that reliable.

###### Traceroute
- `nmap -sS --traceroute 10.10.6.101` - finds the routers between you and the target.

###### [[nmap (network mapper)]] scripting engine (NSE)

-  A script is a piece of code that does not need to be **compiled**.
-  NSE is a **Lua interpreter** that allows to run scripts written in **Lua**.
-  `--scripts=default` or `-sC` to run the scripts in the `default` category.
![[Script_Categories.png]]
- `--script "SCRIPT-NAME"` to use a specific script. a list of all scripts can be found in `/usr/share/nmap/scripts` . Example: `sudo nmap -sS -n --script "http-date" -p80 10.10.117.235` 

###### Saving the output
-  `-oN FILENAME` - saves the output in normal format.
- `-oG FILENAME` - saves the output in a `grep`able format. grep stands for Global Regular Expression Printer. `grep KEYWORD TEXT_FILE` can be used to display all the lines containing `KEYWORD`.
- `-oX FILENAME` saves the output in XML format.
- `-oA FILENAME` save the output in all three formats specified above.
- `-oS FILENAME` saves the output in 'script kiddie' format. (not recommended)

###### Summary nmap04
![[Summary_nmap04.png]]

- pebbl3: `sudo nmap -sVC -p- -oN nmap -v -T5 [machine_ip]`