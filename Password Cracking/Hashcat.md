To **Generate a Hash** In Kali:-
 ```shell
echo -n "[String]" | [hashname]sum
```

---
## Basic Syntax
```shell
hashcat -m [Hash Type] -a [Attack Mode] [Hash_File]
```

- **-m** : Hash Type
	- For *MD5* : 0
	- For *SHA1* : 100
	- For *MD4* : 900
	- For *NTLM* : 1000
	- For *Sha2-256* : 1400
	- For *NTLMv2* : 5600
	- For *Kerberos(krb5tgs)* : 13100
	- For *AS-REP(krb5asrep)* : 18200
	- For `bcrypt $2*$, Blowfish (Unix)` : 3200

- **-a** : Attack Mode
	- For *Wordlist* : 0
	- For *Combinator* : 1
	- For *Bruteforce* : 3


- To Show **Potfile** :-
![[Pasted image 20221231211129.png]]

- To Know How Fast Your PC Can **Crack Hashes** :-
	**hashcat** **--benchmark**![[Pasted image 20221225190025.png]]

- To Select The *CPU/GPU* To Use :-
	Use **-D** [Processor Number]

- To Crack With a *Wordlist* :-
	**hashcat** **-a** 0 **-m** [Hash_Type]  [Wordlist]![[Pasted image 20221231130128.png]]![[Pasted image 20221231130139.png]]

- To Save Results In A *File*
	In Command Type **--outfile** [File_Name]


### Crack Keepass2
```sh
# Remove the name of the database file in the beginning after you convert it with John
hashcat -m 13400 hash /usr/share/wordlists/rockyou.txt -r /usr/share/hashcat/rules/rockyou-30000.rule --force
```


---
# Rules
Rules File Is Available In ***/usr/share/hashcat/rules***
- **:** Try The *Password As It Is Written*
- **$1** *Append 1* At The End Of The Password(s)
- **r** *Reverse* The Password
- **c** *Capitalize* the password

**To Use These Rules** :-
```sh
# 30000 rules for rockyou.txt
hashcat -a 0 -m 0 hash /usr/share/wordlists/rockyou.txt -r /usr/share/hashcat/rules/rockyou-30000.rule

# Best 64 rules
hashcat -m 1000 hash /usr/share/wordlists/rockyou.txt -r /usr/share/hashcat/rules/best64.rule --force
```
![[Pasted image 20221231220746.png]]


#### Combinator
Combines Two *Different Wordlists* Trying Out Different Combinations
![[Pasted image 20221231221922.png]]

---

# Brute Force
**?l** - abcde.......
**?u** - ABCDE........
**?d** - 0123456789
**?s** - #!$......
**?a** - ?l?u?d?s
**?b** - 0x00 to 0xff (Hex Digits)

**Note :** If You Don't Know The Password Length, Then use **--increment --increment-min [min_value] --increment-max [max_value]**
![[Pasted image 20221231222742.png]]![[Pasted image 20221231222809.png]]