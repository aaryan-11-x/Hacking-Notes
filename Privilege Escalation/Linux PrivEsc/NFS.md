
```sh
cat /etc/exports
```

![[Pasted image 20230527130720.png]]

The critical element for this privilege escalation vector is the “**no_root_squash**” option you can see above. By default, NFS will change the root user to *nfsnobody* and *strip any file from operating with root privileges*. If the “**no_root_squash**” option is present on a writable share, we can create an executable with SUID bit set and run it on the target system.

Do [[NFS Enumeration]]

![[Pasted image 20230527131337.png]]

```C
int main(){
setgid(0);
setuid(0);
system("/bin/bash");
return 0;
}
```

Once we compile the code we will set the SUID bit.
![[Pasted image 20230527133059.png]]

![[Pasted image 20230527133139.png]]

