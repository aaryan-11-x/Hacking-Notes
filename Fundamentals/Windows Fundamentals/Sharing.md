# Share Permissions
| Permission   | Description                                                                                                                                  |
| ------------ | -------------------------------------------------------------------------------------------------------------------------------------------- |
| **Full Control** | Users are permitted to perform all actions given by Change and Read permissions as well as change permissions for NTFS files and subfolders. | 
| **Change**       | Users are permitted to read, edit, delete and add files and subfolders.                                                                      |
| **Read**         | Users are allowed to view file & subfolder contents.                                                                                         |

---

# Server Message Block (SMB)
The `Server Message Block protocol` (`SMB`) is used in Windows to connect shared resources like files and printers. It is used in large, medium, and small enterprise environments.


#### Creating a Network Share
Keep in mind that in most **large enterprise** environments, **shares are created on a Storage Area Network (SAN), Network Attached Storage device (NAS), or a separate partition on drives** accessed via a server operating system like Windows Server. If we ever come across shares on a desktop operating system, it will either be a small business or it could be a beachhead system used by a penetration tester or malicious attacker to gather and exfiltrate data.

![[Pasted image 20230606183333.png]]

We are going to use the `Advanced Sharing` option to configure our share.

![[Pasted image 20230606183346.png]]

We can see that it is possible to *limit the number of users that can be connected to this share simultaneously*. In a real-world environment it is a good practice for administrators to set this number according to the number of users that regularly need access to the resource being shared.

Similar to NTFS permissions, there is an `access control list` (`ACL`) for *shared resources*. We can consider this the SMB permissions list.  The ACL contains `access control entries` (`ACEs`). Typically these ACEs are made up of `users` & `groups` (also called security principals) as they are a suitable mechanism for managing and tracking access to shared resources.

![[Pasted image 20230606183543.png]]

Keep in mind that **NTFS permissions apply to the system where the folder and files are hosted**. Folders created in NTFS inherit permissions from parent folders by default.

The **Share permissions apply when the folder is being accessed through SMB, typically from a different system over the network**. This means someone logged in locally to the machine or via RDP can access the shared folder and files by simply navigating to the location on the file system and only need to consider NTFS permissions.


---
# Using smbclient to Connect to the Share
```shell
aaryan11x@htb[/htb]$ smbclient -L IPaddressOfTarget -U htb-student
Enter WORKGROUP\htb-student's password: 

	Sharename       Type      Comment
	---------       ----      -------
	ADMIN$          Disk      Remote Admin
	C$              Disk      Default share
	Company Data    Disk      
	IPC$            IPC       Remote IPC
```
What could potentially block us from accessing this share if all our entries are correct and our permissions list has the Everyone group present with at least Read permissions?


---
# Windows Defender Firewall Considerations
It is the *Windows Defender Firewall* that could potentially be blocking access to the SMB share. Since we are connecting from a *Linux-based system the firewall has blocked access from any device that is not joined* to the same `workgroup`.

It is also important to note that *when a Windows system is part of a workgroup*, all `netlogon` requests are *authenticated against that particular Windows system's* `SAM` database.
When a *Windows system is joined to a Windows Domain environment*, all *netlogon requests are authenticated against* `Active Directory`.
The primary difference between a workgroup and a Windows Domain in terms of authentication, is *with a workgroup the local SAM database is used* and *in a Windows Domain a centralized network-based database (Active Directory) is used*.

We must know this information when attempting to logon & authenticate with a Windows system. Consider where the htb-student account is hosted to properly connect to the target.

Windows Defender Firewall Profiles :-
-   `Public`
-   `Private`
-   `Domain`

It is a best practice to enable predefined rules or add custom exceptions rather than deactivating the firewall altogether. Unfortunately, it is very common for firewalls to be left completely deactivated for the sake of convenience or lack of understanding.

Once the proper `inbound` firewall rules are enabled we will successfully connect to the share. Keep in mind that we can only connect to the share *because the user account* we are using (`htb-student`) is in the `Everyone group`.


#### NTFS Permissions ACL (Security Tab)

![[Pasted image 20230606185529.png]]

There's more granular control with NTFS permissions that can be applied to users and groups. Anytime we see a <mark style="background: #CACFD9A6;">grey</mark> checkmark next to a permission, it was *inherited from a parent directory*. By default, *all NTFS permissions are inherited from the parent directory*. In the Windows world, the `C:\ drive` is the parent directory.


#### Mounting to the Share
```shell
aaryan11x@htb[/htb]$ sudo mount -t cifs -o username=htb-student,password=Academy_WinFun! //ipaddoftarget/"Company Data" /home/user/Desktop/
```

If Command Not Working :-
```sh
aaryan11x@htb[/htb]$ sudo apt install cifs-utils
```

The `net share` command allows us to view all the shared folders on the system. Notice the share we created and also the C:\ drive.

```powershell
C:\Users\htb-student> net share

Share name   Resource                        Remark

-------------------------------------------------------------------------------
C$           C:\                             Default share
IPC$                                         Remote IPC
ADMIN$       C:\WINDOWS                      Remote Admin
Company Data C:\Users\htb-student\Desktop\Company Data

The command completed successfully.
```

**Do you remember us sharing the C:\ drive?**
<u>Ans.</u> We didn't manually share C:. The *most important drive with the most critical files on a Windows system is shared via SMB at install*. This means *anyone with the proper access could remotely access the entire C:\* of each Windows system on a network.