# runas.exe
The `runas.exe` command is a Windows utility that allows a user to *run a program with different credentials*, typically with ***elevated privileges***. It enables users to execute a program or command under a different user account, provided they have the necessary permissions.

When running `runas.exe`, the user is prompted to enter the password for the specified user account. Once authenticated, the specified program or command is executed with the privileges of the target user account.

```powershell
runas.exe /netonly /user:<domain>\<username> cmd.exe
```

-   **/netonly** - Since we are not domain-joined, we want to load the credentials for network authentication but not authenticate against a domain controller. So commands executed locally on the computer will run in the context of your standard Windows account, but any network connections will occur using the account specified here.

In simple terms, when you run a program or command with `/netonly`, it means that any network-related actions performed by that program or command will use the specified user's credentials. However, actions performed locally on the system, such as accessing files or interacting with the user interface, will continue to use the credentials of the currently logged-in user.

For example, let's say you are logged into a computer as **User A**, and you want to run a program that needs to access network resources, such as a shared network drive or a remote server. However, you also have access to those network resources with a different user account, **User B**. By using `runas.exe /netonly`, you can run the program using User B's credentials just for network-related activities. The rest of the actions on your local machine will still be performed using your own User A credentials.


-   **/user** - Here, we provide the details of the domain and the username. It is always a safe bet to use the **Fully Qualified Domain Name** (FQDN) instead of just the NetBIOS name of the domain since this will help with resolution.
-   **cmd.exe** - This is the program we want to execute once the credentials are injected. This can be changed to anything, but the safest bet is cmd.exe since you can then use that to launch whatever you want, with the credentials injected.

Once you run this command, you will be prompted to supply a password. Note that since we added the /netonly parameter, the credentials will not be verified directly by a domain controller so that it will **accept any password**. We still need to confirm that the network credentials are loaded successfully and correctly.

> Important note
>  If you use your own Windows machine, you should make sure that you run your first Command Prompt as Administrator. This will inject an Administrator token into CMD. If you run tools that require local Administrative privileges from your Runas spawned CMD, the token will already be available. This does not give you administrative privileges on the network, but will ensure that any local commands you execute, will execute with administrative privileges.

After providing the password, a new command prompt window will open. Now we still need to verify that our credentials are working. The most surefire way to do this is to list SYSVOL. Any AD account, no matter how low-privileged, can read the contents of the `SYSVOL` directory.

`SYSVOL` is a folder that exists on all domain controllers. It is a shared folder storing the *Group Policy Objects (GPOs)* and information along with any other domain related scripts. It is an essential component for Active Directory since it delivers these GPOs to all computers on the domain. Domain-joined computers can then read these GPOs and apply the applicable ones, making domain-wide configuration changes from a central location.

---
**Note:** These next steps you only need to perform if you use your own ***Windows machine***.

Before we can list SYSVOL, we need to *configure our DNS*. Sometimes you are lucky, and internal DNS will be configured for you automatically through DHCP or the VPN connection, but not always. It is good to understand how to do it manually.
```powershell
$dnsip = "<DC IP>"
$index = Get-NetAdapter -Name 'Ethernet' | Select-Object -ExpandProperty 'ifIndex'
Set-DnsClientServerAddress -InterfaceIndex $index -ServerAddresses $dnsip
```

---

```powershell
# First check whether DNS is working or not
C:\> nslookup za.tryhackme.com

# If DNS is working(It resolves the IP), then force a network-based listing of the SYSVOL directory
C:\Tools>dir \\za.tryhackme.com\SYSVOL\
```


**Question:** _Is there a difference between_ _`dir \\za.tryhackme.com\SYSVOL` and `dir \\<DC IP>\SYSVOL`_ _and why the big fuss about DNS?_

It boils down to the **authentication method** being used. When we provide the hostname, network authentication will attempt first to perform `Kerberos authentication`. Since Kerberos authentication uses hostnames embedded in the tickets, if we provide the IP instead, we can force the authentication type to be `NTLM`. While on the surface, this does not matter to us right now, it is good to understand these slight differences since they can allow you to remain more stealthy during a Red team assessment. In some instances, organisations will be monitoring for OverPass- and Pass-The-Hash Attacks. Forcing NTLM authentication is a good trick to have in the book to avoid detection in these cases.