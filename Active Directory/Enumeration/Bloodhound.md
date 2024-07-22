**Bloodhound** allowed attackers to visualise the AD environment in a graph format with interconnected nodes. Each connection is a possible path that could be exploited to reach a goal.

---
# Installation, Setup & Startup
```sh
apt install bloodhound -y
```

```sh
neo4j console
```

If logging for the *first time*, change the password from `http://localhost:7474/`
Current username/password: neo4j/toor

```sh
bloodhound --no-sandbox
```

---
# BHCE
https://m4lwhere.medium.com/the-ultimate-guide-for-bloodhound-community-edition-bhce-80b574595acf

```cmd
curl -L https://ghst.ly/getbhce | docker compose -f - up
```

---
# Sharphound
Sharphound is the *enumeration tool* of Bloodhound. It is used to enumerate the AD information that can then be visually displayed in Bloodhound. Bloodhound is the actual GUI used to display the AD attack graphs.

There are three different Sharphound collectors:
-   **Sharphound.ps1** - PowerShell script for running Sharphound. However, the latest release of Sharphound has stopped releasing the Powershell script version. This version is good to use with RATs since the script can be loaded directly into memory, evading on-disk AV scans.  
-   **Sharphound.exe** - A Windows executable version for running Sharphound.
-   **AzureHound.ps1** - PowerShell script for running Sharphound for Azure (Microsoft Cloud Computing Services) instances. Bloodhound can ingest data enumerated from Azure to find attack paths related to the configuration of Azure Identity and Access Management.

When using these collector scripts on an assessment, there is a high likelihood that these files will be detected as *malware* and raise an alert to the <mark style="background: #ADCCFFA6;">blue team</mark>. This is again where our Windows machine that is non-domain-joined can assist. We can use the `runas` command to inject the AD credentials and point Sharphound to a Domain Controller.


1) Copy the Sharphound file to the target machine & run the following command :-
```powershell
# For SharpHound.exe 
SharpHound.exe --CollectionMethods All --Domain za.tryhackme.com --ExcludeDCs

# For SharpHound.ps1
. .\SharpHound.ps1
Invoke-Bloodhound -CollectionMethod All -Domain CONTROLLER.local -ZipFileName loot.zip -OutputDirectory C:\Users\stephanie\Desktop\ -OutputPrefix "lol"
```

-   **CollectionMethods** - Determines what kind of data Sharphound would collect. The most common options are Default or All. Also, since Sharphound caches information, once the first run has been completed, you can only use the Session collection method to retrieve new user sessions to speed up the process.
-   **Domain** - Here, we specify the domain we want to enumerate. In some instances, you may want to enumerate a parent or other domain that has trust with your existing domain. You can tell Sharphound which domain should be enumerated by altering this parameter.
-   **ExcludeDCs** -This will instruct Sharphound not to touch domain controllers, which reduces the likelihood that the Sharphound run will raise an alert.


2) Once completed, you will have a *timestamped ZIP* file in the same folder you executed Sharphound from.
```powershell
PS C:\Users\gordon.stevens\Documents> dir 

	Directory: C:\Users\gordon.stevens\Documents 

Mode LastWriteTime Length Name 
---- ------------- ------ ---- 
-a---- 3/16/2022 7:12 PM 121027 20220316191229_BloodHound.zip 
-a---- 3/16/2022 5:19 PM 906752 SharpHound.exe 
-a---- 3/16/2022 7:12 PM 360355 YzE4MDdkYjAtYjc2MC00OTYyLTk1YTEtYjI0NjhiZmRiOWY1.bin
```


3) Copy the zip file to our machine
```sh
scp <AD Username>@THMJMP1.za.tryhackme.com:C:/Users/<AD Username>/Documents/<Sharphound ZIP> .
```

4) Drag & Drop the zip file to Bloodhound
![[Pasted image 20230718121511.png]]
%% Note that if you import a new run of Sharphound, it would cumulatively increase these counts. %%


##### Attack Paths
There are several attack paths that Bloodhound can show. Pressing the three stripes next to "Search for a node" will show the options. The very first tab shows us the information regarding our current imports.

![[Pasted image 20230718121652.png]]

-   **Overview** - Provides summaries information such as the number of active sessions the account has and if it can reach high-value targets.  
-   **Node Properties** - Shows information regarding the AD account, such as the display name and the title.  
-   **Extra Properties** - Provides more detailed AD information such as the distinguished name and when the account was created.  
-   **Group Membership** - Shows information regarding the groups that the account is a member of.
-   **Local Admin Rights** - Provides information on domain-joined hosts where the account has administrative privileges.
-   **Execution Rights** - Provides information on special privileges such as the ability to RDP into a machine.
-   **Outbound Control Rights** - Shows information regarding AD objects where this account has permissions to modify their attributes
-   **Inbound Control Rights** -  Provides information regarding AD objects that can modify the attributes of this account.

%% Press `LeftCtrl` to change the label display settings %%



![[Pasted image 20230718122000.png]]

The icons are called nodes, and the lines are called edges. There is an AD user account with the username of **T0_TINUS.GREEN**, that is a member of the group **Tier 0 ADMINS**. But, this group is a nested group into the **DOMAIN ADMINS** group, meaning all users that are part of the **Tier 0 ADMINS** group are effectively Domain Admins!

Furthermore, there is an additional AD account with the username of **ADMINISTRATOR** that is part of the **DOMAIN ADMINS** group. Hence, there are two accounts in our attack surface that we can probably attempt to compromise if we want to gain DA rights. Since the **ADMINISTRATOR** account is a built-in account, we would likely focus on the user account instead.

##### Finding Attack Vectors
Our Start Node would be our AD username, and our End Node will be the **Tier 1 ADMINS** group since this group has administrative privileges over servers.
![[Pasted image 20230718122517.png]]
It shows that one of the **T1 ADMINS, ACCOUNT,**  broke the tiering model by using their credentials to authenticate to **THMJMP1**, which is a workstation. It also shows that any user that is part of the **DOMAIN USERS** group, including our AD account, has the ability to `RDP` into this host.

We could do something like the following to exploit this path:
1.  Use our AD credentials to RDP into **THMJMP1**.
2.  Look for a privilege escalation vector on the host that would provide us with Administrative access.
3.  Using Administrative access, we can use credential harvesting techniques and tools such as [[Mimikatz]].
4.  Since the T1 Admin has an active session on **THMJMP1**, our credential harvesting would provide us with the NTLM hash of the associated account.


If there is no available attack path using the selected edge filters, Bloodhound will display "No Results Found". %% Note, this may also be due to a Bloodhound/Sharphound mismatch, meaning the results were not properly ingested. %%


#### Session Data Only
The one thing that does change constantly is *active sessions* and *LogOn events*. Since Sharphound creates a point-in-time snapshot of the AD structure, active session data is not always accurate since *some users may have already logged off their sessions or new users may have established new sessions*. This is an essential thing to note and is why we would want to execute Sharphound at regular intervals.

A good approach is to execute Sharphound with the "All" collection method at the start of your assessment and then execute Sharphound at least twice a day using the "Session" collection method. This will provide you with new session data and ensure that these runs are faster since they do not enumerate the entire AD structure again. The best time to execute these session runs is at around 10:00, when users have their first coffee and start to work and again around 14:00, when they get back from their lunch breaks but before they go home.


**Benefits**
-   Provides a GUI for AD enumeration.
-   Has the ability to show attack paths for the enumerated AD information.
-   Provides more profound insights into AD objects that usually require several manual queries to recover.

**Drawbacks**
-   Requires the execution of Sharphound, which is noisy and can often be detected by AV or EDR solutions.

---
# Bloodhound.py
Run Bloodhound without logging into the user
```sh
bloodhound-python -d test.local -u fmcsorley -p CrabSharkJellyfish192 -v --zip -c All -ns 10.10.10.1
```

---
# Cypher Query Language
```cql
// Match all PCs
MATCH (m:Computer) RETURN m

// Match all users
MATCH (m:User) RETURN m

// Match all groups
MATCH (m:Group) RETURN m

// Show logged on users in each computer (If an SID is returned as a user, it means that administrator is logged in the computer)
MATCH p = (c:Computer)-[:HasSession]->(m:User) RETURN p

// Find all Unconstrained Delegation from non-DCs
MATCH (c1:Computer)-[:MemberOf*1..]->(g:Group) WHERE g.objectid ENDS WITH '-516' WITH COLLECT(c1.name) AS domainControllers MATCH (c2 {unconstraineddelegation:true}) WHERE NOT c2.name IN domainControllers RETURN c2

// Find users with "pass" in their description
MATCH p = (d:Domain)-[r:Contains*1..]->(u:User) WHERE u.description =~ '(?i).*pass.*' RETURN p

// List all owned objects
MATCH (n) WHERE "owned" in n.system_tags RETURN n

// Find all paths from owned to tier 0 objects (Warning, can be heavy)
MATCH p = allShortestPaths((o)-[*1..]->(h)) WHERE 'owned' in o.system_tags AND 'admin_tier_0' in h.system_tags RETURN p
```