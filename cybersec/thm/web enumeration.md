###### `gobuster`
-  `gobuster dir` - enumerate directories
	-  `-x` - specify extensions (e.g. `-x.php,.html,.js,.css,.txt)
	-  `-t` - number of concurrent threads
-  `-u` - flag used for specifying the `url`
-  `-w` - flag used for specifying the `wordlist`
-  `gobuster dns` - brute-force subdomain
	-  `-i` - show `IP` addresses
-  wordlists: `usr/share/wordlists/dirbuster/` or `usr/share/wordlists/dirb/`
-  `gobuster vhost -u http://example.com -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt` - brute-force vhosts

###### `wpscan` 
-  used for `wordpress` sites enumerations.
- `wpscan --url [url] --enumerate p`  - plugins
- `wpscan --url [url] --enumerate t`  - themes
- `wpscan --url [url] --enumerate u`  - users
- `wpscan --url [url] --enumerate vp`  - vulnerable flag used with the plugins flag
- `wpscan --url [url] --passwords rockyou.txt --usernames [username(s)]`  - perform a password brute-force attack on the specified usernames
-  `--plugins-detection aggressive/passive` - aggressive profile

###### `nikto`
-  used for searching web server vulnerabilities (`man nikto`)
-  `nikto -h vulnerable_ip` - headers
-  `nmap -p80 172.16.0.0/24 -oG | nikto -h -`  - use `nikto` on a [[nmap (network mapper)]] scan output 
-  `nikto -h 10.10.10.1 -p 80,8000,8080` - scan multiple ports
-  `nikto --list-plugins`  - plugins for extending the capabilities of `nikto`. Example of using the `apacheusers` plugin: `nikto -h 10.10.10.1 -Plugin apacheusers`
-  `--Display` - flag used for setting the verbosity level. arguments: `1`, `2`, `E` - errors
-  `--Tuning` - flag used to tune your scan. Commonly used options: 
![[nikto_tuning_arguments.png]]
- `-o` (`--Output`) for dumping the output into a file. (`.html` can also be used).