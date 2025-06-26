-  Virtual hosting (`vhosting`) lets us host multiple websites on a web server.
-  everything that has to do with filtering is written in: [[filtering (file upload)]] 
###### general methodology
-  knowing your environment is crucial. 
-  use `gobuster` for enumerating dirs.
-  use `wappalyser` - technology stack 
-  use `burpsuite`
-  analyze the source code.
-  maybe check [[OWASP (Open Web Application Security Project)]] top 10
-  overwriting existing files on a web server (like images)
-  look for uploading a reverse/bind shell to achieve RCE (remote code execution)

###### jewel task (task 11)
-  `gobuster` helped me find the `/admin`, `/content` and the `/modules` directories.
-  there were 3 filters on the client-side for uploading: one for **the length** of the file, one for **the file extension** and one for **the magic number** of the file. `http://jewel.uploadvulns.thm/assets/js/upload.js`
-  after bypassing the client-side filters, i intercepted the request with burp. I encoded the shell payload (`Node.js` reverse shell) in **base64** and put it in the intercepted request (replaced the old `file` parameter contents). Also "data" is now `application/javascript` (which is basically the mime type for a node js script that will be executed on server side) The new request looks like this: ![[jewel_request_thm.png]]
-  i enumerated the `content` directory (this is where the files are uploaded) using `gobuster` and the <u>wordlist</u> provided by the task. The `jpg` files with a small size are my payloads. (`WSY.jpg` here, is the one working correctly)
![[gobuster_jewel_content_dir.png]]
-  i spawned a listener with **netcat**
-  i executed the payload through the `/admin` page. (`../content/WSY.jpg`)

`sudo sed -i '$d' /etc/hosts` to revert the changes in `/etc/hosts`