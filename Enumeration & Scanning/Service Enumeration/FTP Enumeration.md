Using the Internet's File Transfer Protocol (FTP), *Anonymous FTP is a method for giving users access to files so that they don't need to identify themselves to the server*. Using an FTP program or the FTP command interface, the user enters "*anonymous*" as a user ID. Usually, the password is defaulted or furnished by the FTP server. Anonymous FTP is a common way to get access to a server in order to view or download files that are publicly available.

If someone tells you to use anonymous FTP and gives you the server name, just remember to use the word "*anonymous*" for your user ID. *Usually, you can enter anything as a password.*

```sh
ftp anonymous@[IP] -p [port]

# To Transfer files, set the Local Directory First
ftp> lcd [directory]
ftp> get [file]
```

![[image-324 1.webp]]


#### Useful Stuff
```sh
# If you see passive mode after entering a command (Always use ls -al)
ftp> ls -al
229 Entering Extended Passive Mode (|||22168|)

# Disable it
ftp> passive
Passive mode: off; fallback to active mode: off.

# Set transfer type to binary to prevent "Interrupted system call"
ftp> binary
```

---
# Common FTP Commands
| Command                   | Description                               |
| ------------------------- | ----------------------------------------- |
| ?/help                    | local help information                    |
| append                    | Append to a file                          |
| ascii                     | set ascii transfer type                   |
| binary                    | Set Binary transfer type                  |
| bye/exit/quit             | Terminate ftp session and exit            |
| cd                        | Change remote working directory           |
| chmod                     | Change file permissions of remote file    |
| close                     | Terminate FTP session                     |
| debug                     | toggle/set debugging mode                 |
| delete/mdelete (multiple) | delete remote file                        |
| dir/ls                    | list contents of remote directory         |
| get/recv/mget (multiple)  | receive file                              |
| mkdir                     | make directory on remote machine          |
| passive                   | enter passive transfer mode               |
| put/mput (multiple)       | send one file                             |
| pwd                       | print working directory on remote machine |
| rmdir                     | remove directory on remote machine        |
| size                      | show size of remote file                  |
| type                      | set file transfer type                    |
| verbose                   | toggle verbose mode                       |
