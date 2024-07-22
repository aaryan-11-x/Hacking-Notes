# Headers
If there are *missing/additional* headers & you see them in the **Response**, add them in the **Request**.
Try to **change the values of the header** if not working!

## Change Headers
Example in **Content-type** header change the content as we saw in Sanders_Fan_Club CTF ([[RFC 5988]])

## Files with same MD5 Hash
https://github.com/corkami/collisions
https://www.mscs.dal.ca/~selinger/md5collision/

---
# Intermediary
Intermediary refers to a **middle step** or a middle entity that plays a role in redirecting a user from one URL to another. The process typically involves the use of HTTP status codes and additional URLs to guide the user's browser to the final destination.
%% ALWAYS USE BURPSUITE to check for these intermediaries %%

---
# Regex
If you see a search string :-
![[Pasted image 20231112205534.png]]

Use [[Regex]] to display info
```sh
.*
.
*
^name
[\s\S]*
```

---
# SQLi
1) Check the id_name, sometimes SQLi is present there!
https://www.youtube.com/watch?v=vP8wnwM5MFY&list=PL1H1sBF1VAKVmrjF1uWh5wK9a2IzmUjPc&index=17&ab_channel=JohnHammond


# OS Command Injection
1) To bypass display restrictions of the output, use various techniques to output the same
```sh
# Example (Output has 4 columns only)
Owner	Group	Size	Filename

hexdump -C /etc/passwd
```

2) Use SSH Command Injection
```sh
ssh bill@localhost sh -c 'echo 0 1 2 3 4 5 6 7 `whoami`'
```

---
# Infinite Scrolling
https://www.youtube.com/watch?v=omb83B6_SJ8&list=PL1H1sBF1VAKVmrjF1uWh5wK9a2IzmUjPc&index=20&ab_channel=JohnHammond

---
# Bruteforcing
**IMPORTANT**: While Bruteforcing sites with `302 code` for *all requests*, check the `Location` Header, if it changes, then consider the login to be successful!

---
# Bypass robots.txt security
![[Pasted image 20240410205138.png]]

Change the user agent to `Googlebot`
![[Pasted image 20240410205221.png]]