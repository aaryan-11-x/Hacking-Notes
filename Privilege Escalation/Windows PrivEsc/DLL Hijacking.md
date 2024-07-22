Most Windows applications do not use the *fully qualified path* when loading an *external DLL library*, instead they *search* from the directory they have been *loaded first*.
If attackers can place a *malicious* DLL in that directory, it will be *executed in place of* the *real* DLL.

###### Order Of Checking DLL
1) Directory From Which The Application Is Loaded
2) C:\\Windows\\System32
3) C:\\Windows\\System
4) C:\\Windows
5) Current Working Directory
6) Directories In The System PATH Environment Variable
7) Directories In The User PATH Environment Variable


![[Pasted image 20230119222809.png]]


### Exploitation
https://infosecwriteups.com/ikeext-dll-hijacking-3aefe4dde7f5

![[042 Overview and Escalation via DLL Hijacking--- [ FreeCourseWeb.com ] ---.mp4]]

```sh
# Compile DLL
x86_64-w64-mingw32-gcc myDLL.cpp --shared -o myDLL.dll

# Restart service
sc stop service & sc start service
OR
Restart-Service BetaService (In Powershell)
```

#### Mitigation
- Disallow loading of remote DLLs
- Enable Safe DLL Search Mode to force search for system DLLs in directories with greater restrictions.
- Use auditing tools such as PowerSploit to detect DLL search order hijacking vulnerabilities and correct them.
- Identify and block software executed through search order hijacking, using whitelisting tools like AppLocker.