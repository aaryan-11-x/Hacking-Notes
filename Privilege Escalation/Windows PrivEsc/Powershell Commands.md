```powershell
# Get users in a group
Get-LocalGroupMember Administrators

# List all scheduled tasks
Get-ScheduledTask

# Check Local User descriptions (Might contain password)
Get-LocalUser

# Check Windows Defender Status
Get-MpComputerStatus

# List AppLocker Rules (if AppLocker is present)
Get-AppLockerPolicy -Effective | select -ExpandProperty RuleCollections

# Test AppLocker policy
Get-AppLockerPolicy -Local | Test-AppLockerPolicy -path C:\Windows\System32\cmd.exe -User Everyone

# How well patched is the system?
Get-HotFix | ft -AutoSize

# Get Product Name
Get-WmiObject -Class Win32_Product |  select Name, Version

# Get Installed Applications
## 32-bit
Get-ItemProperty "HKLM:\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall\*" | select displayname

## 64-bit
Get-ItemProperty "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall\*" | select displayname

# List named pipes
gci \\.\pipe\

# Search for file extensions
Get-ChildItem C:\ -Recurse -Include *.rdp, *.config, *.vnc, *.cred -ErrorAction Ignore

# Get running processes
Get-Process
```
