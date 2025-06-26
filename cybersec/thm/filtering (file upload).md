- instead of `bash`, use `/usr/bin/bash` or `/usr/bin/sh`
- encoding the shell payload into `base64` might help
###### extension validation
- ms windows systems still uses file extensions which are used to identify the contents of a file. Unix based systems rely on other methods. they either **blacklist** extensions (i.e. have a list of extensions which are <u>not</u> allowed) or they **whitelist** extensions (i.e. have a list of extensions which are allowed, and reject everything else).

###### file type filtering
- MIME (Multipurpose Internet Mail Extension) validation. originally when trasfered as attachments over email, but now also when files are being transferred over [[Protocols and Servers#HTTP (Hypertext Transfer Protocol)]] or HTTPS. The MIME type for file upload is attached in the header of the request (e.g. Content-Type: image/jpeg)

###### magic number validation
- The "magic number" of a file is a string of bytes at the very beginning of the file content which identify the content. For example, a PNG file would have these bytes at the very top of the file: `89 50 4E 47 0D 0A 1A 0A`

###### file length filtering

###### file name filtering
-  file names should be sanitised on upload to ensure that they don't contain any "bad characters", which could potentially cause problems on the file system when uploaded (e.g. null bytes or forward slashes on Linux, as well as control characters such as `;` and potentially unicode characters).

###### file content filtering
-  More complicated filtering systems may scan the full contents of an uploaded file to ensure that it's not spoofing its extension, MIME type and Magic Number.

###### bypassing client-side filtering
-  turn off Javascript in your browser, if the site doesn't require in order to provide basic functionality.
-  intercept and modify the incoming page using Burpsuite. (strip out the filter before it has a chance to run)
-  intercept and modify the file upload
-  send the file directly to the upload point with a tool like `curl`, e.g. `curl -X POST -F "submit:<value>" -F "<file-parameter>:@<path-to-file>" <site>`. for this you would first intercept the request with burp too tee the parameters being used in the upload.
- if an app filters the images on client-side by the mime type you could use burpsuite to intercept the upload and change the file extension and the `Content-Type` header, then forward the request.
###### bypassing server-side filtering
 - file extensions:
	- if we have `.php` filtered out we can use `.php3`, `.php4`, `.php5`, `.php7`, `.phps`, `.php-s`, `.pht` and `.phar` . For more info: [wikipedia page](https://en.wikipedia.org/wiki/PHP)
	- if the filter accepts `.jpg` files, for example, we could upload a file called `shell.jpg.shell`
-  magic numbers:
	-  the magic number of a file is a string of hex digits, and is always the very first thing in a file.
	- [list of file signatures on wikipedia](https://en.wikipedia.org/wiki/List_of_file_signatures)
	- `file` to check the file type
	- `hexeditor` - cool command
	- `FF D8 FF DB` - one of the file signatures for `jpeg`
	-  i think burpsuite could help you determine which file types types are accepted.
-  example methodology:
	- 1. The first thing we would do is take a look at the website as a whole. Using browser extensions such as the aforementioned Wappalyzer (or by hand) we would look for indicators of what languages and frameworks the web application might have been built with. Be aware that Wappalyzer is not always 100% accurate. A good start to enumerating this manually would be by making a request to the website and intercepting the response with Burpsuite. Headers such as `server` or `x-powered-by` can be used to gain information about the server. We would also be looking for vectors of attack, like, for example, an upload page.  
    
	1. Having found an upload page, we would then aim to inspect it further. Looking at the source code for client-side scripts to determine if there are any client-side filters to bypass would be a good thing to start with, as this is completely in our control.
	2. We would then attempt a completely innocent file upload. From here we would look to see how our file is accessed. In other words, can we access it directly in an uploads folder? Is it embedded in a page somewhere? What's the naming scheme of the website? This is where tools such as Gobuster might come in if the location is not immediately obvious. This step is extremely important as it not only improves our knowledge of the virtual landscape we're attacking, it also gives us a baseline "accepted" file which we can base further testing on.
	    - An important Gobuster switch here is the `-x` switch, which can be used to look for files with specific extensions. For example, if you added `-x php,txt,html` to your Gobuster command, the tool would append `.php`, `.txt`, and `.html` to each word in the selected wordlist, one at a time. This can be very useful if you've managed to upload a payload and the server is changing the name of uploaded files.
	3. Having ascertained how and where our uploaded files can be accessed, we would then attempt a malicious file upload, bypassing any client-side filters we found in step two. We would expect our upload to be stopped by a server side filter, but the error message that it gives us can be extremely useful in determining our next steps.
	
	Assuming that our malicious file upload has been stopped by the server, here are some ways to ascertain what kind of server-side filter may be in place:
	
	- If you can successfully upload a file with a totally invalid file extension (e.g. `testingimage.invalidfileextension`) then the chances are that the server is using an extension _blacklist_ to filter out executable files. If this upload fails then any extension filter will be operating on a whitelist.
	- Try re-uploading your originally accepted innocent file, but this time change the magic number of the file to be something that you would expect to be filtered. If the upload fails then you know that the server is using a magic number based filter.
	- As with the previous point, try to upload your innocent file, but intercept the request with Burpsuite and change the MIME type of the upload to something that you would expect to be filtered. If the upload fails then you know that the server is filtering based on MIME types.
	- Enumerating file length filters is a case of uploading a small file, then uploading progressively bigger files until you hit the filter. At that point you'll know what the acceptable limit is. If you're very lucky then the error message of original upload may outright tell you what the size limit is. Be aware that a small file length limit may prevent you from uploading the reverse shell we've been using so far.
