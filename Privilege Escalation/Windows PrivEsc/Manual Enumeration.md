https://sushant747.gitbooks.io/total-oscp-guide/content/privilege_escalation_windows.html

```powershell
# Current user Privileges/Groups
whoami /priv
whoami /groups

# Show logged-in users
query user

# List users
net users

# List details of a user
net user [username]

# Other users logged in simultaneously
qwinsta

# User groups defined
net localgroup

# List members of a specific group
net localgroup [group]

# Collect sysinfo
systeminfo | findstr /B /C:"Host Name" /C:"OS Name" /C:"OS Version" /C:"System Type" /C:"Hotfix(s)"

# Display Environment Variables
set

# How well patched is the System?
wmic qfe get Caption,Description,HotFixID,InstalledOn

# Show all disk drives in the System
wmic logicaldisk get caption,description,providername

# List all drivers in the system
driverquery

# See scheduled tasks (https://juggernaut-sec.com/scheduled-tasks/)
schtasks /query /fo LIST /v

# Show running processes
tasklist /svc

# See Application Logs
wevtutil qe Application

# Listing named pipes
pipelist.exe /accepteula

# Check permissions on a named pipe
accesschk.exe -accepteula -w \pipe\WindscribeService -v
```


## Detect Firewall/AV
```powershell
# Check if Windows Defender is running
sc query windefend

# Check for other Antiviruses
sc queryex type-service

# Check if firewall is in Operational mode or not
netsh advfirewall firewall dump
netsh firewall show state

# If firewall is operating, check configurations
netsh firewall show config
```

---
## Password Hunting

#### Unattend.xml
Unattended installation files may define auto-logon settings or additional accounts to be created as part of the installation. Passwords in the `unattend.xml` are stored in plaintext or base64 encoded.
Location: `%SYSTEMDRIVE%\Windows\Panther`
```xml
<?xml version="1.0" encoding="utf-8"?>
<unattend xmlns="urn:schemas-microsoft-com:unattend">
    <settings pass="specialize">
        <component name="Microsoft-Windows-Shell-Setup" processorArchitecture="amd64" publicKeyToken="31bf3856ad364e35" language="neutral" versionScope="nonSxS" xmlns:wcm="http://schemas.microsoft.com/WMIConfig/2002/State" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
            <AutoLogon>
                <Password>
                    <Value>local_4dmin_p@ss</Value>
                    <PlainText>true</PlainText>
                </Password>
                <Enabled>true</Enabled>
                <LogonCount>2</LogonCount>
                <Username>Administrator</Username>
            </AutoLogon>
            <ComputerName>*</ComputerName>
        </component>
    </settings>
```


#### PowerShell History file
```powershell
(Get-PSReadLineOption).HistorySavePath
gc (Get-PSReadLineOption).HistorySavePath

# One liner
foreach($user in ((ls C:\users).fullname)){cat "$user\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt" -ErrorAction SilentlyContinue}
```

### XAMPP Password
`C:\xampp\mysql\bin\my.ini`
`C:\xampp\passwords.txt`

#### Decrypt PowerShell Credentials
```powershell
# If we have gained command execution in the context of this user or can abuse DPAPI, then we can recover the cleartext credentials from encrypted.xml
$credential = Import-Clixml -Path 'C:\scripts\pass.xml'
$credential.GetNetworkCredential().username
$credential.GetNetworkCredential().password
```


#### Sticky Notes Passwords
This file is located at `C:\Users\<user>\AppData\Local\Packages\Microsoft.MicrosoftStickyNotes_8wekyb3d8bbwe\LocalState\plum.sqlite`

We can copy the three `plum.sqlite*` files down to our system and open them with a tool such as DB Browser for SQLite and view the Text column in the Note table with the query `select Text from Note;`

```powershell
# Using Powershell
PS C:\htb> Set-ExecutionPolicy Bypass -Scope Process

Execution Policy Change
The execution policy helps protect you from scripts that you do not trust. Changing the execution policy might expose
you to the security risks described in the about_Execution_Policies help topic at
https:/go.microsoft.com/fwlink/?LinkID=135170. Do you want to change the execution policy?
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "N"): A

PS C:\htb> cd .\PSSQLite\
PS C:\htb> Import-Module .\PSSQLite.psd1
PS C:\htb> $db = 'C:\Users\htb-student\AppData\Local\Packages\Microsoft.MicrosoftStickyNotes_8wekyb3d8bbwe\LocalState\plum.sqlite'
PS C:\htb> Invoke-SqliteQuery -Database $db -Query "SELECT Text FROM Note" | ft -wrap
 
Text
----
\id=de368df0-6939-4579-8d38-0fda521c9bc4 vCenter
\id=e4adae4c-a40b-48b4-93a5-900247852f96
\id=1a44a631-6fff-4961-a4df-27898e9e1e65 root:Vc3nt3R_adm1n!
\id=c450fc5f-dc51-4412-b4ac-321fd41c522a Thycotic demo tomorrow at 10am
```

OR use `strings` or `type`

---
## Identify Outdated Software's
```powershell
# This command doesn't return apps for 32-bit apps built on 64-bit arch
wmic product get name,version,vendor

# This command returns all
wmic service list brief | findstr "Running"

# Get detailed info about a service
sc qc [service_name]
```

### Bonus Tips
1) Check for *recently closed tabs* of that user in the web browser!

---
# Application Exploits
#### Splunk Hijacking
https://airman604.medium.com/splunk-universal-forwarder-hijacking-5899c3e0e6b2


### Erlang (Port 25672) Exploit
Erlang is a programming language designed around distributed computing and will have a network port that allows other Erlang nodes to join the cluster. The secret to join this cluster is called a cookie. Many applications that utilize Erlang will either use a weak cookie (**RabbitMQ** uses `rabbit` by default) or place the cookie in a configuration file that is not well protected. Some example Erlang applications are **SolarWinds, RabbitMQ, and CouchDB**. For more information check out the [Erlang-arce blogpost from Mubix](https://malicious.link/post/2018/erlang-arce/)

---
# Mount VHDX/VMDK
Three specific file types of interest are `.vhd, .vhdx, and .vmdk` files. These are **Virtual Hard Disk, Virtual Hard Disk v2 (both used by Hyper-V), and Virtual Machine Disk (used by VMware)**.
If we encounter any of these three files, we have options to mount them on either our local Linux or Windows attack boxes. If we can mount a share from our Linux attack box or copy over one of these files, we can mount them and explore the various operating system files and folders as if we were logged into them using the following commands.

```sh
# Mount VMDK on Linux
guestmount -a SQL01-disk1.vmdk -i --ro /mnt/vmdk

# Mount VHD/VHDX on Linux
guestmount --add WEBSRV10.vhdx  --ro /mnt/vhdx/ -m /dev/sda1
```