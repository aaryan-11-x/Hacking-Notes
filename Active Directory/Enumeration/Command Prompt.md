The `net` command is a handy tool to enumerate information about the local system and AD.

```sh
# List all users in the AD domain
C:\>net user /domain

# Enumerate more details about a single user account
C:\>net user [username] /domain

# List all groups
C:\>net group /domain

# Enumerate more details about a group
C:\>net group "[Group name]" /domain

# Enumerate Password Policy of the domain
C:\>net accounts /domain

# Check on guest user
C:\>net user guest /domain

# Add user to a domain group
net group "Management Department" stephanie /add /domain

# Enable/Disable Guest user
C:\>net user guest /active [yes/no]

# Get User SPNs
setspn -L [username]
```


###### Drawbacks
-   The `net` commands must be executed from a domain-joined machine. If the machine is not domain-joined, it will default to the WORKGROUP domain.
-   The `net` commands may not show all information. For example, if a user is a member of more than ten groups, not all of these groups will be shown in the output.