# General Active Directory Hardening Measures
#### LAPS
Accounts can be set up to have their *password rotated on a fixed interval* (i.e., 12 hours, 24 hours, etc.). This free tool can be beneficial in reducing the impact of an individual compromised host in an AD environment.


#### Audit Policy Settings (Logging and Monitoring)
Every organization needs to have *logging and monitoring setup to detect and react to unexpected changes or activities* that may indicate an attack. Effective logging and monitoring can be used to detect an attacker or unauthorized employee adding a user or computer, modifying an object in AD, changing an account password, accessing a system in an unauthorized or non-standard manner, performing an attack such as password spraying, or more advanced attacks such as modern Kerberos attacks.


#### Group Policy Security Settings
Group Policy Objects (GPOs) are virtual collections of policy settings that can be applied to specific users, groups, and computers at the OU level. These can be used to apply a wide variety of security policies to help harden Active Directory. The following is a non-exhaustive list of the types of security policies that can be applied:

-   **Account Policies** - Manage how user accounts interact with the domain. These include the password policy, account lockout policy, and Kerberos-related settings such as the lifetime of Kerberos tickets.

-   **Local Policies** - These apply to a specific computer and include the security event audit policy, user rights assignments (user privileges on a host), and specific security settings such as the ability to install drivers, whether the administrator and guest accounts are enabled, renaming the guest and administrator accounts, preventing users from installing printers or using removable media, and a variety of network access and network security controls.

    - The LM hash is relatively weaker than the NT and is prone to a fast brute-force attack. The best recommendation is to *prevent Windows from storing the password's LM hash*. You can access it through the following:
    `Group Policy Management Editor > Computer Configuration > Policies > Windows Settings > Security Settings > Local Policies > Security Options > double click Network security - Do not store LM (Lan Manager) hash value on next password change policy > select "Define policy setting" `

    - Configuring SMB signing through group policy is crucial to *detect Man in the Middle (MiTM) attacks* that may result in modification of SMB traffic in transit. SMB signing ensures the integrity of data for both client and server. All supported Windows versions have an SMB packet signing option.
	`Group Policy Management Editor > Computer Configuration > Policies > Windows Settings > Security Settings > Local Policies > Security Options > double click Microsoft network server: Digitally sign communication (always) > select Enable Digitally Sign Communications`

	- LDAP signing is a Simple Authentication and Security Layer (SASL) property that only accepts signed LDAP requests and ignores other requests (plain-text or non-SSL).
	`Group Policy Management Editor > Computer Configuration > Policies > Windows Settings > Security Settings > Local Policies > Security Options > Domain controller: LDAP server signing requirements > select Require signing from the dropdown`

-   **Software Restriction Policies** - Settings to control what software can be run on a host.
   
-   **Application Control Policies** - Settings to control which applications can be run by certain users/groups. This may include blocking certain users from running all executables, Windows Installer files, scripts, etc. Administrators use AppLocker to restrict access to certain types of applications and files.

-   **Advanced Audit Policy Configuration** - A variety of settings that can be adjusted to audit activities such as file access or modification, account logon/logoff, policy changes, privilege usage, and more.

![[Pasted image 20230614192107.png]]



### Microsoft Security Compliance Toolkit (MSCT)
Microsoft Security Compliance Toolkit (MSCT) is an official toolkit provided by Microsoft to implement and manage local and domain-level policies. You don't have to worry about complex policy syntaxes and scripts, as Microsoft will provide pre-developed security baselines per the end user environment. You can download MSCT from the official Microsoft website [link](https://www.microsoft.com/en-us/download/details.aspx?id=55319).

`Open Microsoft Security Compliance Website > click Download > click Windows Servers Security Baseline.zip > Download`
![[Pasted image 20240430120517.png]]

`Open extracted folder > Scripts > & select desired baseline & execute with PowerShell`
![[Pasted image 20240430120658.png]]


![[Pasted image 20240430122331.png]]