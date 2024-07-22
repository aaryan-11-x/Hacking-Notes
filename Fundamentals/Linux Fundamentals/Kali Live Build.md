In this scene of **Mr Robot**, Angela had to boot up a Kali USB drive, and enter several commands in order to get her job done. Can you build a custom Kali ISO that would improve on this? Automate the process so that Angela does not have to touch the keyboard after booting Kali.

Build a live ISO for Angela. Show her how it's done!

```sh
Update your system!
kali@kali:~$ sudo apt-get update
kali@kali:~$
kali@kali:~$ sudo apt-get dist-upgrade
kali@kali:~$
Install needed tools
kali@kali:~$ sudo apt install -y git live-build cdebootstrap curl
kali@kali:~$
Clone Kali live build config files
kali@kali:~$
Overwrite default package lists with minimal needed packages
kali@kali:~$ cd live-build-config/
kali@kali:~/live-build-config$
kali@kali:~/live-build-config$ cat kali-config/variant-default/package-lists/kali.list.chroot
kali@kali:~/live-build-config$
kali@kali:~/live-build-config$ echo cryptsetup > kali-config/variant-default/package-lists/kali.list.chroot
kali@kali:~/live-build-config$
kali@kali:~/live-build-config$ echo openssh-server >> kali-config/variant-default/package-lists/kali.list.chroot
kali@kali:~/live-build-config$
kali@kali:~/live-build-config$ echo nmap >> kali-config/variant-default/package-lists/kali.list.chroot
kali@kali:~/live-build-config$
Add files to live filesystem (Configure custom startup script)
kali@kali:~/live-build-config$ mkdir -p kali-config/common/includes.chroot/lib/systemd/system/
kali@kali:~/live-build-config$
Register a custom "Angela" service to run /usr/bin/startssh
kali@kali:~/live-build-config$ cat << EOF > kali-config/common/includes.chroot/lib/systemd/system/angela.service
[Unit]
Description=Start Custom Script
After=multi-user.target

[Service]
Type=idle
ExecStart=/bin/bash /usr/bin/startssh

[Install]
WantedBy=multi-user.target
EOF

kali@kali:~/live-build-config$
Create /usr/bin (and parents) on live file system
kali@kali:~/live-build-config$ mkdir -p kali-config/common/includes.chroot/usr/bin/
kali@kali:~/live-build-config$
Create a "startssh" script to do our nefarious things
kali@kali:~/live-build-config$ cat << EOF > kali-config/common/includes.chroot/usr/bin/startssh
#!/bin/sh
echo hola > /root/test.txt
EOF

kali@kali:~/live-build-config$
Create live hook to enable our custom service
kali@kali:~/live-build-config$ cat << EOF > kali-config/common/hooks/live/angela.chroot
#!/bin/sh
systemctl enable angela.service || true
EOF
kali@kali:~/live-build-config$
Make it executable
kali@kali:~/live-build-config$ chmod 755 kali-config/common/hooks/live/angela.chroot
kali@kali:~/live-build-config$
Create boot config file, adjust prompt, timeout, autoboot, etc:
kali@kali:~/live-build-config$ cat << EOF > kali-config/common/includes.binary/isolinux/isolinux.cfg
include menu.cfg
default vesamenu.c32
prompt 0
timeout 20
ONTIMEOUT live-amd64
EOF
kali@kali:~/live-build-config$
Build the ISO!
kali@kali:~/live-build-config$ sudo ./build.sh --verbose
kali@kali:~/live-build-config$
```