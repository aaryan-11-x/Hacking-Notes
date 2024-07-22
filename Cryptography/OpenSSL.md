# RSA
1) To Generate a **Private Key** :-
`openssl genrsa -out private-key.pem 2048`
- **genrsa** : Generate an RSA Private Key
- **-out** : Output File
- **2048** : Key Size (Bits)

2) To Generate the **Public Key** :-
`openssl rsa -in private-key.pem -pubout -out public-key.pem`
- **rsa** : Algorithm used is RSA
- **-pubout** : Generate the Public Key from the Private Key
- **-in** : Select the Private Key File

3) To Print the **Private Key** :-
`openssl rsa -in private-key.pem -text -noout`
The values of _p_, _q_, _N_, _e_, and _d_ are `prime1`, `prime2`, `modulus`, `publicExponent`, and `privateExponent` respectively.


- [ ] **If we already have the recipient’s public key, we can ENCRYPT it with the command** :-
`openssl pkeyutl -encrypt -in plaintext.txt -out ciphertext -inkey public-key.pem -pubin`
- **-in** : Plain Text to Encrypt
- **-out** : Cipher Text File
- **-inkey** : Public Key of the Recipient


- [ ] **For it's DECRYPTION** :-
`openssl pkeyutl -decrypt -in ciphertext -inkey private-key.pem -out decrypted.txt`

---

# Diffie Hellman
1) Generate Diffie-Hellman Parameters (P & G)
```sh
openssl dhparam -out dhparams.pem 2048
```
- **dhparam** : Diffie-Hellman Parameters
- **2048** : Size of Parameters in bits

2) View the Parameters
```sh
openssl dhparam -in dhparams.pem -text -noout
```

3) Generate **Private Keys**
```sh
# Generate Alice's Private Key
alice@tryhackme$ openssl genpkey -paramfile dhparams.pem -out alice_private.pem

# Generate Bob's Private Key
bob@tryhackme$ openssl genpkey -paramfile dhparams.pem -out bob_private.pem
```

4) Generate **Public Keys**
```sh
# Generate Alice's Public Key
alice@tryhackme$ openssl pkey -in alice_private.pem -pubout -out alice_public.pem

# Generate Bob's Public Key
bob@tryhackme$ openssl pkey -in bob_private.pem -pubout -out bob_public.pem
```

5) Derive **Shared Secret Key**
```sh
# Derive Secret Key
alice@tryhackme$ openssl pkeyutl -derive -inkey alice_private.pem -peerkey bob_public.pem -out shared_secret.bin
```


```sh
# Encryption
bob@tryhackme$ openssl enc -aes-256-cbc -pass file:shared_secret.bin -in recipe.txt -out encrypted_recipe.enc

# Decryption
alice@tryhackme$ openssl aes-256-cbc -d -in encrypted_recipe.enc -pass file:shared_secret.bin -out recipe.txt
```

To summarize, this command uses the **AES-256-CBC** encryption algorithm and a shared secret key file `shared_secret.bin` to encrypt the contents of `recipe.txt`. The encrypted result is saved in `encrypted_recipe.enc`.

---

# Hash Generation with Salt
#opensslpasswd
```sh
openssl passwd -1 -salt THM password1
```
-   `openssl passwd`: This command invokes the *OpenSSL utility for generating password hashes*.
-   `-1`: This option specifies the *MD5-based Apache variant* of the crypt algorithm. It ensures that the generated hash is compatible with systems that use this specific algorithm for password hashing.
-   `-salt THM`: The `-salt` option allows you to specify a salt value to be used in the password hashing process. In this case, `THM` is the salt value provided.
-   `password1`: This is the password you want to hash. Replace `password1` with the actual password you want to generate a hash for.


---
# Encrypt File with AES-256
We can also override the default iterations counts with the option `-iter` 100000 and add the option `-pbkdf2` to *use the Password-Based Key Derivation Function 2* algorithm. When we hit enter, we'll need to provide a password.

```sh
aaryan11x@htb[/htb]$ openssl enc -aes256 -iter 100000 -pbkdf2 -in [file_to_encrypt] -out [output_file]

enter aes-256-cbc encryption password:                                                         
Verifying - enter aes-256-cbc encryption password: 
```
```sh
# Decryption
aaryan11x@htb[/htb]$ openssl enc -d -aes256 -iter 100000 -pbkdf2 -in passwd.enc -out passwd                    

enter aes-256-cbc decryption password:
```

---
# Communicate with a Server over SSL/TLS
```sh
echo "[password]" | openssl s_client -connect localhost:1234


# -ign_eof: Inhibit shutting down the connection when end of file is reached in the input.
```

---
# Bruteforce OpenSSL Data with Salted Password

![[Pasted image 20231206112310.png]]

```sh
bruteforce-salted-openssl -t 50 -f /usr/share/wordlists/rockyou.txt -d sha256 drupal.txt.enc -1

# Specify the cipher (-c) & digest (-d)
# Default cipher is AES-256-CBC & digest is MD5 so no need to mention it
# -1: Stop at first valid password
```
![[Pasted image 20231206112646.png]]

```sh
# Decrypt the File
openssl aes-256-cbc -d -in drupal_decoded.txt.enc -out drupal.txt -k friends
```
