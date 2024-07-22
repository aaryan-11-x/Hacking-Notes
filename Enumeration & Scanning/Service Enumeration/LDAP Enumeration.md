```sh
# Get Domain names
ldapsearch -H ldap://192.168.204.122 -x -s base namingcontexts

# If domain is hutch.offsec
ldapsearch -x -H 192.168.162.122 -b "dc=hutch,dc=offsec"   # Search for passwords or any other useful info

# Get account names of the system
ldapsearch -H ldap://192.168.204.122 -x -b DC=hutch,DC=offsec '(objectClass=person)' | grep "sAMAccountName:"
```

https://book.hacktricks.xyz/network-services-pentesting/pentesting-ldap
https://medium.com/@minimalist.ascent/pentesting-ldap-servers-25577bde675b