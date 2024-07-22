This occurs when *user input* is passed to *system commands*. As a result, an attacker is *able to execute arbitrary system commands on application servers*.
It occurs when *server-side code*in a web application makes a *system call on the hosting machine*.
Attacker Can Inject a Shell on your server!

![[Pasted image 20230417095155.png]]
![[Pasted image 20230417095319.png]]


---

###### Java

![[Pasted image 20230417102729.png]]


##### PHP

![[Pasted image 20230417102806.png]]
1. Checking if the parameter "commandString" is set

2. If it is, then the variable `$command_string` gets what was passed into the input field

3. The program then goes into a try block to execute the function `passthru($command_string)`. It is *executing what gets entered into the input* then *passing the output directly back to the browser*.


---

# Types of Command Injection

=== multi-column-start: ID_jr9n
```column-settings
Number of Columns: 2
Largest Column: standard
```
##### In-Band Command Injection
Attacker *Receives the Response* of the Command In The Application

=== end-column ===
##### Blind Command Injection
Attacker *Doesn't Receive the Response* of the Command In The HTTP Response

=== multi-column-end

---

**Pro Tip : Use # At the End of These Injections!**


![[Pasted image 20230417113522.png]]


##### Example

![[Pasted image 20230424121840.png]]


---


![[Pasted image 20230424162329.png]]

##### Example 1

![[Pasted image 20230424124608.png]]

##### Example 2

Use <mark style="background: #FF5582A6;">Burp Collaborator</mark> To Open an *Out-of-Band* Channel back to a server you control.
![[Pasted image 20230424161532.png]]

---
## Filter Bypasses
https://book.hacktricks.xyz/linux-hardening/bypass-bash-restrictions

---

Cheatsheet : https://hackersonlineclub.com/command-injection-cheatsheet/
