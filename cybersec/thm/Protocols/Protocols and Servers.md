Some of commonly used protocols: 
-  HTTP
-  [[Protocols and Servers#FTP (File Transfer Protocol)]]
-  POP3
-  SMTP
-  IMAP
###### Telnet
-  Used to connect to a virtual terminal of another computer. User can execute commands, programs, start batch processes, etc. on the remote system. 
-  The data is sent **unencrypted**.
-  Telnet uses port **23**.
-  The secure version of [[#Telnet]] is [[#SSH (Secure Shell)]].
-  Command example of connecting to a remote system: `telnet 10.10.178.220`. The user will be asked to enter login credentials.
-  Command example of connecting to a web server: `telnet 10.10.178.220 80`. `GET /index.html HTTP/1.1` used for retrieving the page `index.html` or `GET / HTTP/1.1` to retrieve the default page. User has to give a value for the `host` header, e.g. `host: telnet`, and then then Enter/Return key has to be pressed **twice**.

###### HTTP (Hypertext Transfer Protocol)
-  Used to transfer web pages.
-  Sends and receives data as cleartext (not encrypted)
-  Interesting: [[#Telnet]] or Netcat can be used as a "web browser" and communicate with a web server.
-  A [[#HTTP (Hypertext Transfer Protocol)]] web server and a [[#HTTP (Hypertext Transfer Protocol)]] client is necessary for using HTTP.
-  A web server will "serve" a specific set of files to the requesting web browser.
-  Three popular [[#HTTP (Hypertext Transfer Protocol)]] servers: 
	-  Apache
	-  Internet Information Services (IIS)
	-  nginx

###### FTP (File Transfer Protocol)
-  a protocol designed to help the efficient transfer of files between different and even non-compatible systems. It supports two modes for file transfer: binary and ASCII (text).
-  Uses listens on port **21**.
-  User has to provide the username (`USER username`) and password (`PASS password`)
-  `STAT` - provides added information
-  `SYST` - provides information about the system type of the target (e.g UNIX)
-  `PASV` - switch the mode to **passive**.
-  [[#FTP (File Transfer Protocol)]] has two modes: 
	-  **Active**: the data is sent over a separate channel originating from the [[#FTP (File Transfer Protocol)]] server's port 20.
	-  **Passive**: the data is sent over a separate channel originating from an [[#FTP (File Transfer Protocol)]] client's port above port number 1023.
-  `TYPE A` - switches the file transfer to ASCII
-  `TYPE B` - switches the file transfer to **binary**.
-  [[#Telnet]] cannot be used to transfer a file because [[#FTP (File Transfer Protocol)]] creates a separate connection for file transfer.
-  `ftp` command can be used to act as an actual [[#FTP (File Transfer Protocol)]] client and execute various [[#FTP (File Transfer Protocol)]] commands.
- Examples of [[#FTP (File Transfer Protocol)]] servers: 
	- vsftpd
	- ProFTPD
	- uFTP
-  FileZilla - [[#FTP (File Transfer Protocol)]] client with GUI (Graphical User Interface).
-  Some [[#FTP (File Transfer Protocol)]] commands: 
	-  `get` for retrieving a file.
	-  `ls` for listing files.
	-  `put` for uploading files.
- get the banner with `nc <ip> 21` (software version)

###### SMTP (Simple Mail Transfer Protocol)
-  Requires the following components: 
	- Mail Submission Agent (MSA)
	- Mail Transfer Agent (MTA)
	- Mail Delivery Agent (MDA)
	- Mail User Agent (MUA)

![[SMTP.png]]

1. A Mail User Agent (MUA), or simply an email client, has an email message to be sent. The MUA connects to a Mail Submission Agent (MSA) to send its message.
2. The MSA receives the message, checks for any errors before transferring it to the Mail Transfer Agent (MTA) server, commonly hosted on the same server.
3. The MTA will send the email message to the MTA of the recipient. The MTA can also function as a Mail Submission Agent (MSA).
4. A typical setup would have the MTA server also functioning as a Mail Delivery Agent (MDA).
5. The recipient will collect its email from the MDA using their email client.  

-  Analogy:
1. You (MUA) want to send postal mail.
2. The post office employee (MSA) checks the postal mail for any issues before your local post office (MTA) accepts it.
3. The local post office checks the mail destination and sends it to the post office (MTA) in the correct country.
4. The post office (MTA) delivers the mail to the recipient mailbox (MDA).
5. The recipient (MUA) regularly checks the mailbox for new mail. They notice the new mail, and they take it.

-  Uses cleartext, [[#Telnet]] can be used to act as an email client (MUA)
-  Listens on port **25** by default.
-  Some [[#SMTP (Simple Mail Transfer Protocol)]] commands:
	-  `helo`
	-  `mail from`
	-  `rcpt to`
	-  `data` 

###### POP3 (Post Office Protocol version 3)
-  Post Office Protocol version 3 (POP3) is a protocol used to download the email messages from a Mail Delivery Agent (MDA) server. After the emails are downloaded locally, they will get deleted automatically from the server.

![[POP3.png]]
-  Uses port **110** by default.
-  Some [[#POP3 (Post Office Protocol version 3)]] commands: 
	-  `USER username`
	-  `PASS password`
	-  `STAT` - response format: `+OK nn mm` where `nn` is the number of email messages in the inbox and `mm` is the size of the inbox in octets (byte)
	-  `LIST` - provides a list of new messages on the server
	-  `RETR 1` - retrieves the first messages on the server
	-  `QUIT`
-  Uses cleartext (data is unencrypted).
-  [[#POP3 (Post Office Protocol version 3)]] is not convenient for multiple users using the same mail account. If we want the mailbox to be synchronized, we need to consider using other protocols like [[#IMAP]].
- - `hydra -l USERNAME -P /path/to/passwords.txt -f <IP> pop3 -V -t 60` - pop3 password brute force (60 threads)


###### IMAP (Internet Message Access protocol)
-  Emails are synchronized across multiple devices.
-  Uses port **143** by default.
-  Each command should be preceded by a random string to be able to track the reply.
-  A list of commands: 
	-  `LOGIN username password`
	-  `LIST "" "*"`
	-  `EXAMINE INBOX` 
-  Sends the credentials in **cleartext**
- `hydra -l USERNAME -P /path/to/passwords.txt -f <IP> imap -V -t 60` - imap password brute force (60 threads)
	

![[Email_Protocols_Summary.png]]

![[Summary_protocolsandservers.png]]

-  remote desktop protocol: `xfreerdp /dynamic-resolution +clipboard /cert:ignore /v:10.10.90.218 /u:Administrator /p:'TryH4ckM3!'`
