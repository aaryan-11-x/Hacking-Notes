
=== multi-column-start: ID_5hga
```column-settings
Number of Columns: 2
Largest Column: standard
```
###### SUID
- When a file has the SUID permission set, it allows the process to run with the permissions of the file owner. This means that even if the process is executed by a regular user, it will have temporary root (superuser) privileges for the duration of its execution.
- SUID is a binary flag set on an executable file. It is a single permission that is either enabled or disabled.

=== end-column ===

##### Capabilities
- Capabilities provide a more fine-grained approach to granting privileges. Rather than running the entire process with elevated permissions, specific capabilities can be assigned to the process or executable file, allowing it to perform certain privileged operations without needing root privileges.

- Capabilities provide a more granular approach to granting privileges. There are multiple capabilities available, each representing a specific permission. They can be individually assigned or removed from a process or executable file.

=== multi-column-end


To list **Enabled Capabilities** :-
```sh
getcap -r / 2>/dev/null
/usr/sbin/getcap -r / 2>/dev/null
```

![[Pasted image 20230526205856.png]]
Go to [[GTFOBins]] to search for any available Priv Esc Vectors!


### tcpdump
![[Pasted image 20240521133837.png]]

```sh
# Keep trying on each network interface for atleast 2-5 minutes, then check for any sensitive information
timeout 150 tcpdump -w cap.pcap -i veth1665bcd
tcpdump -r cap.pcap
```