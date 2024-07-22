WSL can be installed by running the PowerShell command `Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux` as an Administrator. Once this *feature is enabled, we can either download a Linux distro from the Microsoft Store and install it or manually download the Linux distro of our choice* and unpack and install it from the command line.

WSL installs an application called `Bash.exe`, which can be run by merely typing `bash` into a Windows console to *spawn a Bash shell*. We have the full look and feel of a Linux host from this shell, including the standard Linux directory structure.

```powershell
PS C:\htb> ls /

bin dev home lib lLib64 media opt root sbin srv tmp var
boot etc init 1lib32 Libx32 mnt proc run Snap sys usr
```

```powershell
PS C:\htb> uname -a

Linux WS01 4.4.0-18362-Microsoft #476-Microsoft Frit Nov 01 16:53:00
PST 2019 x86_64 x86 _64 x86_64 GNU/Linux
```

We can **access the C$ volume and other volumes on the host operating** system via the `mnt` directory, making the transition from the WSL host and the Windows host OS seamless.