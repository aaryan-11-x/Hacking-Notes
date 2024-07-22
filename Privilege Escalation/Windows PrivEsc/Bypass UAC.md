```powershell
# Confirm if UAC is enabled or not
REG QUERY HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Policies\System\ /v EnableLUA

HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Policies\System
    EnableLUA    REG_DWORD    0x1

# Check UAC Level
REG QUERY HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Policies\System\ /v ConsentPromptBehaviorAdmin

HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Policies\System
    ConsentPromptBehaviorAdmin    REG_DWORD    0x5
0x5 means highest UAC level (Always notify)
```

Then check the **Windows Version** and go to **UACME** to check if an exploit is available or not!

Exploit for **Windows 10 Build 14393** :-
https://academy.hackthebox.com/module/67/section/626