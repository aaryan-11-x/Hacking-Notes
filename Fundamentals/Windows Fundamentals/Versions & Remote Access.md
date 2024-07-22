| Operating System Names               | Version Number |     |
| ------------------------------------ | -------------- | --- |
| Windows NT 4                         | 4.0            |     |
| Windows 2000                         | 5.0            |     |
| Windows XP                           | 5.1            |     |
| Windows Server 2003, 2003 R2         | 5.2            |     |
| Windows Vista, Server 2008           | 6.0            |     |
| Windows 7, Server 2008 R2            | 6.1            |     |
| Windows 8, Server 2012               | 6.2            |     |
| Windows 8.1, Server 2012 R2          | 6.3            |     |
| Windows 10, Server 2016, Server 2019 | 10.0           |     |

# Cmdlet
A cmdlet is a *lightweight command* that is used in the *PowerShell environment*. The PowerShell runtime invokes these cmdlets within the context of automation scripts that are provided at the command line. The *PowerShell runtime* also invokes them programmatically through PowerShell APIs.

Cmdlets are in the form of `Verb-Noun`. For example, the command `Get-ChildItem` can be used to list our current directory. Cmdlets also take arguments or flags. 
We can type `Get-ChildItem -` and hit the tab key to iterate through the arguments.
A command such as `Get-ChildItem -Recurse` will show us the contents of our current working directory and all subdirectories.
Another example would be `Get-ChildItem -Path C:\Users\Administrator\Documents` to get the contents of another directory.
Finally, we can combine arguments such as this to list all subdirectories' contents within another directory recursively: `Get-ChildItem -Path C:\Users\Administrator\Downloads -Recurse`.


# Get-Wmi Object
We can use the [Get-WmiObject](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.management/get-wmiobject?view=powershell-5.1) [cmdlet](https://docs.microsoft.com/en-us/powershell/scripting/developer/cmdlet/cmdlet-overview?view=powershell-7) to *find information about the operating system*. This cmdlet can be used to get instances of WMI classes or information about available WMI classes.


We can easily obtain this information using the `win32_OperatingSystem` class, which shows that we are on a Windows 10 host, build number 19041.

```powershell
Get-WmiObject -Class win32_OperatingSystem | select Version,BuildNumber

Version    BuildNumber
-------    -----------
10.0.19041 19041
```

Some other useful classes that can be used with `Get-WmiObject` are `Win32_Process` to get a process listing, `Win32_Service` to get a listing of services, and `Win32_Bios` to get [Basic Input/Output System](https://en.wikipedia.org/wiki/BIOS) (`BIOS`) information.

---
# Remote Access
Most common Remote Access Technologies :-

-   Virtual Private Networks (VPN)
-   Secure Shell (SSH)
-   File Transfer Protocol (FTP)
-   Virtual Network Computing (VNC)
-   Windows Remote Management (or PowerShell Remoting) (WinRM)
-   Remote Desktop Protocol (RDP)


## Remote Desktop Protocol (RDP)
Port: **3389**

- **Windows**
	`win + r ---> mstsc`
	Save connection :-
![[SavingRDPConnections.gif]]
	As pentesters, we can benefit from looking for these saved Remote Desktop Files (`.rdp`) while on an engagement.



- **Linux**
```shell
aaryan11x@htb[/htb]$ xfreerdp /v:<targetIp> /u:htb-student /p:Password
```
