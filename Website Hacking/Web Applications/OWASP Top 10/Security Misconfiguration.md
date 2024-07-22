Security Misconfiguration occurs when *security could have been configured properly but was not*.

Security misconfigurations include:
-   *Poorly configured permissions on cloud services*, like S3 buckets
-   Having *unnecessary features enabled* like services, pages, accounts or privileges
-   *Default accounts with unchanged passwords*
-   *Hard-coding API keys*, IP addresses, database credentials, and so on in the source code
-   *Error messages* that are *overly detailed* and allow an attacker to find out more about the system
-   Not using [HTTP security headers](https://owasp.org/www-project-secure-headers/), or *revealing too much detail in the HTTP header*
-   Attacker *Discovers Admin Pages* On The Server
-   *Directory Listing* is *Enabled* On The Server

This vulnerability can often lead to more vulnerabilities, such as default credentials giving you access to sensitive data, XXE or command injection on admin pages.


#### Method 1 - Invalid Input
Try a *Special Character* in an *input* area. 
Example  :-
**GET /product?productId=1** ----------> **GET /product?productId=1'**  OR  **GET /product?productId=lmao**

Try something like this to get an *internal server error* message.


#### Method 2 - BCDE
Look For *Comments* Made by the Developer in the *Source Code*!
<mark style="background: #FF5582A6;">BCDE</mark> : <mark style="background: #FF5582A6;">Backup, Config, Debug, Error</mark>

**Debugging Data**
For debugging purposes, many websites generate *custom error messages* and logs that contain large amounts of *information about the application's behavior*. While this information is useful during development, it is also extremely useful to an attacker if it is *leaked in the production environment*.
Debug messages can sometimes contain vital information for developing an attack, including:
	- *Values for key session variables* that can be manipulated via user input
	- *Hostnames and credentials* for back-end components
	- *File and directory names* on the server
	- *Keys* used to *encrypt data* transmitted via the client

Use [[Content Discovery]] Techniques To Discover More Information!



#### Method 3 - TRACE Method
Developers might forget to *disable various debugging options* in the production environment. For example, the HTTP `TRACE` method is *designed for diagnostic purposes*. If enabled, the web server will respond to requests that use the `TRACE` method by echoing in the response the exact request that was received.

We Can Use this `TRACE` method to *reveal additional headers in the request being appended*.
Try **Localhost** in **X-Custom-Ip-Authorization if it's there**!



#### Method 4 - Version control history
Virtually *all websites are developed using some form of version control system*, such as **Git**. By default, a Git project stores all of its version control data in a folder called `.git`. Occasionally, *websites expose this directory in the production environment*. In this case, you might be able to access it by simply browsing to `/.git`.
- *Download* the folder in your Linux machine!
- Use **git-cola** to check for *previous files/messages*

=== multi-column-start: ID_cl7p
```column-settings
Number of Columns: 2
Largest Column: standard
```
##### Git-Cola
Standalone GUI for Git that is used for managing changes to files and collaborating on projects locally.

=== end-column ===
##### Github
online platform for hosting Git repositories and collaborating on code with others.

=== multi-column-end

![[Pasted image 20230415150520.png]]




