# Encryption
```sh
gpg --cipher-algo [encryption type] [encryption method] [file to encrypt]
```

![[Pasted image 20230606121832.png]]
Then, Put in the password!

---
# Decryption
`gpg -d [encrypted file]`
![[Pasted image 20230606121924.png]]

If there is a **Private PGP** key `.asc, .key`, first load into the memory :-
```sh
gpg --import tryhackme.asc
gpg --list-secret-keys

# Then decrypt it
gpg -d [filename]
```
