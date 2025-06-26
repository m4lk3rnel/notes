- https://owasp.org/www-project-top-ten/
-  The Open Web Application Security Project is a nonprofit foundation focused on understanding web technologies and exploitations and provides resources and tools designed to improve the security of software applications.

1. [[#Broken Access Control]]
2. [[#Cryptographic Failures]]
3. [[#Injection]]
4. [[#Insecure Design]]
5. [[#Security Misconfiguration]]
6. [[#Vulnerable and Outdated Components]]
7. [[#Identification and Authentication Failures]]
8. [[#Software and Data Integrity Failures]]
9. [[#Security Logging & Monitoring Failures]]
10. [[#Server-Side Request Forgery (SSRF)]]

###### Broken Access Control
-  If a website allow regular users to access protected pages that should only be accessed by admins, then the access controls are broken.
-  Allows attackers to bypass **authorisation**.

###### Cryptographic Failures
-  Misuse (or lack of use) of **cryptographic algorithms** for protecting sensitive information.
-  **Encrypting data in transit**: encrypting the network traffic between the client and the server.
-  **Encrypting data in rest**: encrypting the data that will be stored. e.g. the email provider stores the email **encrypted** in their server.
-  Techniques like **Man in The Middle Attacks** are used for taking advantage of [[#Cryptographic Failures]] (attacker forces the user's connections go through a device they control)

###### Injection
-  Injection flaws are very common in applications today. These flaws occur because the application interprets user-controlled input as commands or parameters.
-  Common examples: 
	-  SQL Injection: This occurs when user-controlled input is passed to SQL queries. As a result, an attacker can pass in SQL queries to manipulate the outcome of such queries.
	-  Command Injection: This occurs when user input is passed to system commands. As a result, an attacker can execute arbitrary system commands on application servers, potentially allowing them to access users' systems.
-  Preventing injection attacks: 
	-  Using an allow list: the input is compared to a list of safe inputs or characters. If it is safe than it is processed. 
	-  Stripping input: If the input contains dangerous characters, these are removed before processing.

###### Insecure Design

-  Insecure Design is related to the idea behind the app, not misconfigurations or bad implementations. 
-  Example of Insecure Design: simple answers to security questions for password resets, e.g. "What is your favorite color". In this case the color could be guessed easily. 
-  Please read this (example of insecure design): [[How I Could Have Hacked Any Instagram Account]]

###### Security Misconfiguration

Security Misconfigurations are distinct from the other Top 10 vulnerabilities because they occur when security could have been appropriately configured but was not. Even if you download the latest up-to-date software, poor configurations could make your installation vulnerable.

- Poorly configured permissions on cloud services, like S3 buckets.
- Having unnecessary features enabled, like services, pages, accounts or privileges.
- Default accounts with unchanged passwords.
- Error messages that are overly detailed and allow attackers to find out more about the system.
- Not using [HTTP security headers](https://owasp.org/www-project-secure-headers/).

For more info, look at the [OWASP top 10 entry for Security Misconfiguration](https://owasp.org/Top10/A05_2021-Security_Misconfiguration/).

###### Debugging interfaces

-  Exposure of debugging features in production software. e.g. the [werkzeug console](https://werkzeug.palletsprojects.com/en/3.0.x/)
-  [Patreon breach 2015](https://labs.detectify.com/2015/10/02/how-patreon-got-hacked-publicly-exposed-werkzeug-debugger/) 
-  Werkzeug is a vital component in Python-based web applications as it provides an interface for web servers to execute the Python code. Werkzeug includes a debug console that can be accessed either via URL on `/console`, or it will also be presented to the user if an exception is raised by the application.

###### Vulnerable and Outdated Components
-  The target is using with a well-known vulnerability
-  e.g. the target uses an outdated version of WordPress. A tool like [WPScan](https://wpscan.com) to determine what version of WordPress is used.  [Exploit-DB](https://www.exploit-db.com/exploits/41962) can be used to search for known vulnerabilities. In this case this version of WordPress is vulnerable to Remote Code Execution.

###### [[#Vulnerable and Outdated Components]] Lab
-  [Web shell script](https://www.exploit-db.com/exploits/47887) by uploading a file or remote code execution? Very interesting.

###### Identification and Authentication Failures

-  Authentication allows users to gain access to web applications by verifying their identities.
-  Usually a session cookie is provided by the server if the user enters their correct credentials.
-  If an attacker is able to find flaws in an authentication mechanism, they might gain access to other users' accounts.
	-  Brute force attacks: an attacker can launch brute force attacks, if the mechanism uses usernames and passwords. (mitigation: automatic lockout after a certain number of attempts.)
	-  Use of weak credentials: if passwords like "password1" are used, the attacker could easily guess the password. (mitigation: the server uses a strong password policy.)
	-  Weak Session cookies: if session cookies contain predictable values, attackers can set their own session cookies and access users' accounts. 
	
-  Implement Multi-Factor Authentication. If a user has multiple authentication methods, for example, using a username and password and receiving a code on their mobile device, it would be difficult for an attacker to get both the password and the code to access the account.

###### Identification and Authentication Failures Practical

-  Because developers forget to sanitise the input, the application can be vulnerable to SQL injections. 
-  Example: re-registration of an existing user. If there is an registered account with the username `admin`, an attacker can create an account using ` admin` (note the space). Attacker will gain admin privileges.

###### Software and Data Integrity Failures
-  hashes are used when downloading files, that will be sent from the server so the user can check if there are no integrity issues with the file. (a transmission error might happen).
- This vulnerability arises from code or infrastructure that uses software or data without using any kind of integrity checks. Since no integrity verification is being done, an attacker might modify the software or data passed to the application, resulting in unexpected consequences.

###### Software Integrity Failures

-  example: If an attacker manages to modify the contents of jquery.6.1... (injects malicious code), every user that access a website that includes jquery.6.1... that is stored in the library's server will pull the malicious code. (rewrite this paragraph, please.). 
```html
<script src="https://code.jquery.com/jquery-3.6.1.min.js"></script>
```
-  to mitigate this vulnerability, a hash could be specified along the library's URL, so the library code is executed only if the hash of the downloaded file matches the expected value. This security mechanism is called `Subresource Integrity (SRI)`. Read more about it [here](https://www.srihash.org/). 

```html
<script src="https://code.jquery.com/jquery-3.6.1.min.js" integrity="sha256-o88AwQnZB+VDvE9tvIXrMQaPlFFSUTR+nldQm1LuPXQ=" crossorigin="anonymous"></script>
```

###### Data Integrity Failures

-  Session tokens are usually stored in the client's browsers as **Cookies** (key-value pairs). 
-  The session token is used on each request. The web server will verify the identity of the user by using the session token.
-  Security-wise, storing the username in the session token would be a bad idea because an attacker could steal that token and impersonate the victim.
-  JWT (JSON Web Token) could help us with the integrity check.
-  The structure of a JWT Token is formed of 3 parts: ![[JWT_Token.png]]
	-  The **Header** contains metadata indicating this is a JWT, and the signing algorithm in use is **HS256**.
	-  The **Payload** contains the data that the web application wants the client to store.
	-  The **Signature**, similar to a hash, is used to verify the payload's integrity. If you change the payload, the web application will verify the signature. It will not match the payload and it will know that you tampered the payload. The signature, unlike a simple hash, involves the use of a secret key held by the server only, which means that if you change the payload, you won't be able to generate the matching signature unless you know the secret key.
-  All three parts of the JWT are encoded in **base64**.

###### JWT and the None Algorithm
- To bypass the signature verification you could specify `none` for `alg`. The attacker could use the username `admin` and encode the header and the payload back. The **Signature** part was removed but a  `.`  was kept at the end of the encoded payload.
-  The header and the payload should be encoded separately. And a dot should be added at the end.

###### Security Logging and Monitoring Failures
-  Every action of the user should be logged. Trough logging malicious activity could be traced.
-  The information stored in logs should include the following:
	-  [[Protocols and Servers#HTTP (Hypertext Transfer Protocol))]] status codes
	-  Time Stamps
	-  Usernames
	-  API endpoints/page locations
	-  [[IP (internet protocol)]] addresses

###### Server-Side Request Forgery (SSRF)

-  This type of vulnerability occurs when an attacker can coerce a web application into sending requests on their behalf to arbitrary destinations while having control of the contents of the request itself. SSRF vulnerabilities often arise from implementations where our web application needs to use third-party services.
-  The %23% entity (url encoding) could be used to break the url from other parameters. Might have to do also with bypassing a white listed input. 