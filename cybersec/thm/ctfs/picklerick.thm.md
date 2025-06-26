-  found a username as a comment in the source code of the homepage: `R1ckRul3s`
-  enumerated files with `gobuster` and i found the `login.php` page and the `robots.txt` file. The `robots.txt` file contained the password.
-  after logging in i was redirected to `/portal.php` which was also found with `gobuster`. This page allowed remote code execution.
-  There was a small filter. I couldn't use the `cat` command for display the contents of the file, so i used an alternative: `less`.
-  i found the first ingredient in: `Sup3rS3cretPickl3Ingred.txt`. There was a `clue.txt` that says: "Look around the file system for the other ingredient.". Later on I found the second ingredient in `/home/rick/` in a text file called "second ingredients".
-  the third ingredient was found in /root/3rd.txt. In the `/etc/sudoers` file is specified that: 
```
root    ALL=(ALL:ALL) ALL
www-data                ALL=(ALL) NOPASSWD: ALL
```
 -  to view the 3rd ingredient: `sudo cat /root/3rd.txt` (if a reverse shell is used to bypass the `cat` filter. If not, `sudo less /root/3rd.txt` can be used.)