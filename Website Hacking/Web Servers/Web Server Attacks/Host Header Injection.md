In Old Times, **One Server ---> One Website Hosted**
![[Pasted image 20230403091803.png]]


But Due to Advancement In Technologies like :-
- Shared Hosting
- Cloud Computing
- Virtual Computing

And Limitations Like :-
- Limited IPv4 Address

**One Server ---> Multiple Websites Hosted**
![[Pasted image 20230403092105.png]]

----


If There is no *Sanitization* In the Request Field, You Can Redirect it To Your **Desired Webpage**
![[Pasted image 20230403093012.png]]

If *Sanitization* is present, you can't redirect it!
![[Pasted image 20230403093053.png]]

----


=== multi-column-start: ID_49ob
```column-settings
Number of Columns: 3
Largest Column: standard
```
#### First Way (Change The Host Name)

**GET  / HTTP/1.1**
Host: evil.com
...
**HTTP/1.1 302 Found**
Location: http://evil.com/login.php

=== end-column ===
#### Second Way (Use X-Forwarded-Host)

**GET /HTTP/1.1**
Host: a.com
**X-Forwarded-Host**: www.evil.com
...

=== end-column ===
#### Third Way (Use Another Host)

**GET  / HTTP/1.1**
Host: a.com
**Host: evil.com**

=== multi-column-end



![[Pasted image 20230403101304.png]]

