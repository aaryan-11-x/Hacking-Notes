# Windows Services
Services are a major component of the Windows operating system. They *allow for the creation and management of long-running processes*. Windows *services can be started automatically at system boot without user intervention*. These services can *continue to run in the background even after the user logs out* of their account on the system.

1) Windows services are managed via the Service Control Manager (SCM) system, accessible via the `services.msc` MMC add-in.
2) It is also possible to query and manage services via the command line using `sc.exe` using [PowerShell](https://docs.microsoft.com/en-us/powershell/scripting/overview?view=powershell-7) cmdlets such as `Get-Service`.
```powershell
PS C:\htb> Get-Service | ? {$_.Status -eq "Running"} | select -First 2 |fl


Name                : AdobeARMservice
DisplayName         : Adobe Acrobat Update Service
Status              : Running
DependentServices   : {}
ServicesDependedOn  : {}
CanPauseAndContinue : False
CanShutdown         : False
CanStop             : True
ServiceType         : Win32OwnProcess

Name                : Appinfo
DisplayName         : Application Information
Status              : Running
DependentServices   : {}
ServicesDependedOn  : {RpcSs, ProfSvc}
CanPauseAndContinue : False
CanShutdown         : False
CanStop             : True
ServiceType         : Win32OwnProcess, Win32ShareProcess
```

Service statuses can appear as *Running, Stopped, or Paused*, and they can be set to *start manually, automatically, or on a delay at system boot*. Services can also be shown in the *state of Starting or Stopping* if some action has triggered them to either start or stop. Windows has three categories of services: **Local Services, Network Services, and System Services.**

In Windows, we have some [critical system services](https://docs.microsoft.com/en-us/windows/win32/rstmgr/critical-system-services) that ***cannot be stopped and restarted without a system restart***. If we update any file or resource in use by one of these services, we must restart the system.

| Service                   | Description                                                                                                                                                                                                              |
| ------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **smss.exe**                  | Session Manager SubSystem. Responsible for handling sessions on the system.                                                                                                                                              |
| **csrss.exe**                 | Client Server Runtime Process. The user-mode portion of the Windows subsystem.                                                                                                                                           |
| **wininit.exe**               | Starts the Wininit file .ini file that lists all of the changes to be made to Windows when the computer is restarted after installing a program.                                                                         |
| **logonui.exe**               | Used for facilitating user login into a PC.                                                                                                                                                                              |
| **lsass.exe**                 | The Local Security Authentication Server verifies the validity of user logons to a PC or server. It generates the process responsible for authenticating users for the Winlogon service.                                 |
| **services.exe**              | Manages the operation of starting and stopping services.                                                                                                                                                                 |
| **winlogon.exe**              | Responsible for handling the secure attention sequence, loading a user profile on logon, and locking the computer when a screensaver is running.                                                                         |
| **System**                    | A background system process that runs the Windows kernel.                                                                                                                                                                |
| **svchost.exe with RPCSS**    | Manages system services that run from dynamic-link libraries (files with the extension .dll) such as "Automatic Updates," "Windows Firewall," and "Plug and Play." Uses the Remote Procedure Call (RPC) Service (RPCSS). |
| **svchost.exe with Dcom/PnP** | Manages system services that run from dynamic-link libraries (files with the extension .dll) such as "Automatic Updates," "Windows Firewall," and "Plug and Play." Uses the Distributed Component Object Model (DCOM) and Plug and Play (PnP) services.                                                                                                                                                                                                                         |


---
# Processes
Processes *run in the background on Windows* systems. They either *run automatically* as part of the Windows operating system or are *started by other installed applications*. Processes associated with installed applications can often be *terminated without causing a severe impact* on the operating system.

## Local Security Authority Subsystem Service (LSASS)
`lsass.exe` is the process that is **responsible for enforcing the security policy on Windows** systems. When a user attempts to log on to the system, this process verifies their log on attempt and creates access tokens based on the user's permission levels. LSASS is also responsible for *user account password changes*.
LSASS is an extremely high-value target as several tools exist to extract both cleartext and hashed credentials stored in memory by this process.


## Sysinternals Tools
The [SysInternals Tools suite](https://docs.microsoft.com/en-us/sysinternals) is a set of portable Windows applications that can be used to administer Windows systems.

```powershell
C:\htb> \\live.sysinternals.com\tools\procdump.exe -accepteula

ProcDump v9.0 - Sysinternals process dump utility
Copyright (C) 2009-2017 Mark Russinovich and Andrew Richards
Sysinternals - www.sysinternals.com

Monitors a process and writes a dump file when the process exceeds the
specified criteria or has an exception.
```

The suite includes tools such as `Process Explorer`, an enhanced version of `Task Manager`, and `Process Monitor`, which can be used to *monitor file system, registry, and network activity* related to any process running on the system. Some additional tools are `TCPView`, which is used to *monitor internet activity*, and `PSExec`, which can be used to *manage/connect to systems via the SMB protocol remotely*.