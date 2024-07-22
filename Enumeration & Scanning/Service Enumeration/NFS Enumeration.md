Port 2049 (Default)
# Showmount
- If You See An *nfs Open Port*, Run This Command To Get The *Shared File Location*.
	showmount -e [IP_Address]
![[Pasted image 20220811102128.png]]

- Next, *Create A Mount Folder Inside /mnt* & **Mount The Files Of The Server On That Folder**
![[Pasted image 20220811102354.png]]

#### Unmount a device
```sh
umount -f -l /mnt/nfs
# -f – Force unmount (in case of an unreachable NFS system). (Requires kernel 2.1.116 or later.)
# -l – Lazy unmount. Detach the filesystem from the filesystem hierarchy now, and cleanup all references to the filesystem as soon as it is not busy anymore. (Requires kernel 2.4.11 or later.)
```

---

# Nmap
```sh
nmap -p 111 --script=nfs-ls,nfs-statfs,nfs-showmount [MACHINE_IP]
```
