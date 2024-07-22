Depending on the host that was breached, various configuration files may be of value for enumeration: 

-   Web application config files
-   Service configuration files
-   Registry keys
-   Centrally deployed applications


# Breaching Credentials from McAfee config file
**McAfee Enterprise Endpoint Security**, which organisations can use as the endpoint detection and response tool for security.
McAfee embeds the credentials used during installation to connect back to the orchestrator in a file called `ma.db`. This database file can be retrieved and read with local access to the host to recover the associated AD service account.

```sh
thm@thm:# sqlitebrowser ma.db
```
Using sqlitebrowser, we will select the Browse Data option and focus on the **AGENT_REPOSITORIES** table:
![[Pasted image 20230706111055.png]]

```sh
thm@thm:~$ python2 mcafee_sitelist_pwd_decrypt.py <AUTH PASSWD VALUE>
Crypted password : <AUTH PASSWD VALUE>
Decrypted password : <Decrypted Pasword>
```
