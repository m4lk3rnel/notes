
![[Protocols-and-servers2.png]]

###### Sniffing Attack

-  Sniffing attack refers to using a network packet capture tool to collect information about the target. When a protocol communicates in cleartext, the data exchanged can be captured by a third party to analyse.

For sniffing attacks: 
-  **Tcpdump**: free open source command-line interface program, works on many operating systems
	-  `sudo tcpdump -i <interface>`
	-  `sudo tcpdump port 110 -A` (-A: display in ASCII format)
	-  You would require access to the network traffic.
	-  "we can access the traffic exchanged if we launch a successful Man-in-the-Middle (MITM) attack."
-  **Wireshark**: free open source graphical user interface (GUI) program, can be used on several operating systems, including MS Windows, Linux, MacOS
-  **Tshark**: command-line interface alternative to **Wireshark**
	-  `tshark -i <interface>`

-  "In brief, any protocol that uses cleartext communication is susceptible to this kind of attack. The only requirement for this attack to succeed is to have access to a system between the two communicating systems. This attack requires attention; the mitigation lies in adding an encryption layer on top of any network protocol. In particular, Transport Layer Security (TLS) has been added to HTTP, FTP, SMTP, POP3, IMAP and many others. For remote access, Telnet has been replaced by the secure alternative Secure Shell (SSH)."

- to make **Wireshark** display only **IMAP** traffic just use the display filter: "IMAP"

###### Man-in-the-Middle (MITM) Attack

-  "A Man-in-the-Middle (MITM) attack occurs when a victim (A) believes they are communicating with a legitimate destination (B) but is unknowingly communicating with an attacker (E)"
-  Tools for MITM attacks: 
	- [ettercap](https://www.ettercap-project.org/) (ARP spoofing/poisoning for altering the routing on a network)
	- [bettercap](https://www.bettercap.org/)

###### Transport Layer Security (TLS)
![[OSI_Model_servers_and_protocols2.png]]

- "We can use TLS to upgrade HTTP, FTP, SMTP, POP3, and IMAP, to name a few"
![[protocols_and_their_secured_version.png]]

Considering the case of HTTP. Initially, to retrieve a web page over HTTP, the web browser would need at least perform the following two steps:

1. Establish a TCP connection with the remote web server
2. Send HTTP requests to the web server, such as `GET` and `POST` requests.

HTTPS requires an additional step to encrypt the traffic. The new step takes place after establishing a TCP connection and before sending HTTP requests. This extra step can be inferred from the ISO/OSI model in the image presented earlier. Consequently, HTTPS requires at least the following three steps:

1. Establish a TCP connection
2. **Establish SSL/TLS connection**
3. Send HTTP requests to the webserver

To establish an SSL/TLS connection, the client needs to perform the proper handshake with the server. Based on [RFC 6101](https://datatracker.ietf.org/doc/html/rfc6101), the SSL connection establishment will look like the figure below.

![[SSL_handshake.png]]

1. The client sends a ClientHello to the server to indicate its capabilities, such as supported algorithms.
2. The server responds with a ServerHello, indicating the selected connection parameters. The server provides its certificate if server authentication is required. The certificate is a digital file to identify itself; it is usually digitally signed by a third party. Moreover, it might send additional information necessary to generate the master key, in its ServerKeyExchange message, before sending the ServerHelloDone message to indicate that it is done with the negotiation.
3. The client responds with a ClientKeyExchange, which contains additional information required to generate the master key. Furthermore, it switches to use encryption and informs the server using the ChangeCipherSpec message.
4. The server switches to use encryption as well and informs the client in the ChangeCipherSpec message.

-  "A client was able to agree on a secret key with a server that has a public certificate. This secret key was securely generated so that a third party monitoring the channel wouldn’t be able to discover it. Further communication between the client and the server will be encrypted using the generated key."

-  DNS over TLS (DoT) is **one way to send DNS queries over an encrypted connection**.

###### SSH (Secure Shell)
-  Lets you execute commands on a remote machine over a secure channel.
-  To use SSH you need an SSH server and an SSH client. The SSH server listens on port 22 by default.
-  The SSH client can authenticate using: 
	- A username and a password
	- A private and a public key (after the SSH server is configured to recognize the corresponding public key)
-  Syntax: `ssh username@MACHINE_IP`
-  FTPS (FTP secured using SSL/TLS) default port: 990
-  SFTP (FTP secured using SSH) default port: 22, just like SSH

###### Password attacks

Attacks against passwords are usually carried out by:

1. Password Guessing: Guessing a password requires some knowledge of the target, such as their pet’s name and birth year.
2. Dictionary Attack: This approach expands on password guessing and attempts to include all valid words in a dictionary or a wordlist.
3. Brute Force Attack: This attack is the most exhaustive and time-consuming where an attacker can go as far as trying all possible character combinations, which grows fast (exponential growth with the number of characters).

"The choice of the word list should depend on your knowledge of the target. For instance, a French user might use a French word instead of an English one. Consequently, a French word list might be more promising."

Tool for automated password attacks: [THC Hydra](https://github.com/vanhauser-thc/thc-hydra)

-  `hydra -l mark -P /usr/share/wordlists/rockyou.txt 10.10.91.96 ftp`
-  `-vV` for verbosity
-  `-t 16` will create 16 threads used to connect to the target.
-  `-d` for debugging information

![[common_protocols.png]]