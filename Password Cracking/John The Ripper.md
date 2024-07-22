```sh
# Specify a format
john hash --format=RAW-SHA1

# Add rules
[List.Rules:customRules]
c $1 $3 $7 $!
c $1 $3 $7 $@
c $1 $3 $7 $#

## Save it in a file & append to john.conf
cat custom.rule >> /etc/john/john.conf
john hash --wordlist=ssh.passwords --rules=customRules
```

---
# Encrypted Files
For *Zip File*
```shell
zip2john [filename] > [txt file]
```

For *Rar File*
```shell
rar2john [filename] > [txt file]
```

For *gpg/asc File*
```shell
gpg2john [filename] > [txt file]
```


![[Pasted image 20230101103714.png]]


---
# Linux Users
```
john /etc/shadow
```

![[Pasted image 20230101104401.png]]

##### Unshadow Command
#unshadow
The `unshadow` command is often used in conjunction with the `passwd` and `shadow` files. It takes the `/etc/passwd` and `/etc/shadow` files as input and *combines them to generate a new file that contains both the user account information and the corresponding password hashes*. This combined file is useful in scenarios where you want to crack hashes.

```
unshadow passwd.txt shadow.txt > passwords.txt
john passwords.txt
```


To Use *Wordlists* :-
```sh
john [hash_file] --wordlist=[Wordlist]
```


---
# SSH Private Key
```
ssh2john [ssh_id] > text.hash
john text.hash --wordlist=/usr/share/rockyou.txt
```

![[Pasted image 20230410091443.png]]


---
# Kerberos Ticket Hash
#kerberosticketcrack
```sh
kirbi2john [file] > hash
```