# Windows Management Instrumentation
WMI is a subsystem of PowerShell that provides system administrators with powerful tools for system monitoring. The goal of WMI is to consolidate device and application management across corporate networks. WMI is a core part of the Windows operating system and has come pre-installed since Windows 2000. It is made up of the following components :-

| Component Name     | Description                                                                                                                                                                        |
| ------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| WMI Service        | The Windows Management Instrumentation process, which runs automatically at boot and acts as an intermediary between WMI providers, the WMI repository, and managing applications. |
| Managed Objects    | Any logical or physical components that can be managed by WMI.                                                                                                                     |
| WMI Providers      | Objects that monitor events/data related to a specific object.                                                                                                                     |
| Classes            | These are used by the WMI providers to pass data to the WMI service.                                                                                                               |
| Methods            | These are attached to classes and allow actions to be performed. For example, **methods can be used to start/stop processes on remote machines**.                                  |
| WMI Repository     | A database that stores all static data related to WMI.                                                                                                                             |
| CMI Object Manager | The system that requests data from WMI providers and returns it to the application requesting it.                                                                                  |
| WMI API            | Enables applications to access the WMI infrastructure.                                                                                                                             |
| WMI Consumer       | Sends queries to objects via the CMI Object Manager.                                                                                                                                                                                   |

Some of the *uses for WMI* are:
-   Status information for local/remote systems
-   Configuring security settings on remote machines/applications
-   Setting and changing user and group permissions
-   Setting/modifying system properties
-   Code execution
-   Scheduling processes
-   Setting up logging

These tasks can all be performed using a combination of PowerShell and the WMI Command-Line Interface (WMIC). WMI can be run via the Windows command prompt by typing `wmic`.


## wmic Commands

1. **Get CPU Information**
```sh
wmic cpu list brief
OR
wmic cpu list full
```

2. **Get OS Information**
```powershell
C:\htb> wmic os list brief

BuildNumber  Organization  RegisteredUser  SerialNumber             SystemDirectory      Version
19041                      Owner           00123-00123-00123-AAOEM  C:\Windows\system32
```
The above command example uses `LIST` to show data and the adverb `BRIEF` to provide just the core set of properties.


3. **Get BIOS Information**
```powershell
C:\htb> wmic bios list brief
```

4. **Get Memory Information**
```sh
C:\htb> wmic memorychip list full
```

5. **Get Disk Information**
```sh
C:\htb> wmic diskdrive list full
```

6. **Get Motherboard Information**
```sh
C:\htb> wmic baseboard list full
```

7. **Get GPU Information**
```sh
C:\htb> wmic path win32_videocontroller
```

8. **Get SID of Users/Groups**
```shell
wmic useraccount get name,sid
wmic group get name,sid
```



WMI can be used with PowerShell by using the `Get-WmiObject` [module](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.management/get-wmiobject?view=powershell-5.1). This module is used to *get instances of WMI classes or information about available classes*. This module can be used against local or remote machines.

```powershell
PS C:\htb> Get-WmiObject -Class Win32_OperatingSystem | select SystemDirectory,BuildNumber,SerialNumber,Version | ft

SystemDirectory     BuildNumber SerialNumber            Version
---------------     ----------- ------------            -------
C:\Windows\system32 19041       00123-00123-00123-AAOEM 10.0.19041
```

We can also use the `Invoke-WmiMethod` [module](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.management/invoke-wmimethod?view=powershell-5.1), which is used to *call the methods of WMI objects*. A simple example is *renaming a file*. We can see that the command completed properly because the `ReturnValue` is set to 0.

```powershell
PS C:\htb> Invoke-WmiMethod -Path "CIM_DataFile.Name='C:\users\public\spns.csv'" -Name Rename -ArgumentList "C:\Users\Public\kerberoasted_users.csv"

__GENUS          : 2
__CLASS          : __PARAMETERS
__SUPERCLASS     :
__DYNASTY        : __PARAMETERS
__RELPATH        :
__PROPERTY_COUNT : 1
__DERIVATION     : {}
__SERVER         :
__NAMESPACE      :
__PATH           :
ReturnValue      : 0
PSComputerName   :
```
