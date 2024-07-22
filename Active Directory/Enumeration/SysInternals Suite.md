```powershell
# Find logged on users in a computer (Requires Remote Registry service to be turned ON; already turned on in Windows Servers)
.\PsLoggedon.exe \\computername  
## you might also see your username via resource shares, it's because it uses the NetSessionEnum API, which in this case requires a logon in order to work
```