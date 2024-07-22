| What we can do                                                                   | Syntax                                                                                            | Example                                                                                             |
| -------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------- |
| Find files based on filename                                                     | find [directory path] -type f -name [filename]                                                    | find /home/Andy -type f -name sales.txt                                                             |
| Find Directory based on directory name                                           | find [directory path] -type d -name [filename]                                                    | find /home/Andy -type d -name pictures                                                              |
| Find files based on size                                                         | find [directory path] -type f -size [size]                                                        | find /home/Andy -type f -size 10c (c: Bytes, k: Kilobytes, M: Megabytes, G: Gigabytes)              |
| Find files based on username                                                     | find [directory path] -type f -user [username]                                                    | find /etc/server -type f -user john                                                                 |
| Find files based on group name                                                   | find [directory path] -type f -group [group name]                                                 | find /etc/server -type f -group teamstar                                                            |
| Find files modified after a specific date                                        | find [directory path] -type f -newermt '[date and time]'                                          | find / -type f -newermt '6/30/2020 0:00:00'                                                         |
| Find files based on date modified                                                | find [directory path] -type f -newermt [start date range] ! -newermt [end date range]             | find / -type f -newermt 2013-09-12 ! -newermt 2013-09-14 (Only Date: 2013-09-13 will be considered) |
| Find files based on date accessed                                                | find [directory path] -type f -newerat [start date range] ! -newerat [end date range]             | find / -type f -newerat 2017-09-12 ! -newerat 2017-09-14                                            |
| Find files with a specific keyword                                               | grep -iRl [directory path/keyword]                                                                | grep -iRl '/folderA/flag'                                                                           |
| Move Multiple files/folders simultaneously                                       | mv [file1]  [file2]  [file3] -t [directory to move to]                                            | mv logs.txt keys.conf script.py -t /home/savedWork                                                  |
| Move all files from current directory into another directory                     | mv * [directory to move files to]                                                                 | mv * /home/scripts                                                                                  |
| Upload file to a remote machine (Same for Windows, use `-r` to copy a directory) | scp [filename]  [username] @[IP of remote machine]:/[directory to upload to]                      | scp example.txt john@192.168.100.123:/home/john/                                                    |
| Rename File which starts with "-"                                                | mv -- [-filename]  [filename]                                                                     | mv -- -logs logs                                                                                    |
| Find files which contain an IP                                                   | find [directory path] -exec grep -l '[0-9][0-9]*[.][0-9][0-9]*[.][0-9][0-9]*[.][0-9][0-9]*' {} \; | find . -exec grep -l '[0-9][0-9]*[.][0-9][0-9]*[.][0-9][0-9]*[.][0-9][0-9]*' {} \; 2>/dev/null      |
| Unzip **.zip** Files without unzip utility                                       | gunzip -S .zip [file_name.zip]                                                                    | gunzip -S .zip upload_nix.zip                                                                       |
| Find files by ruling out a regex                                                 | find ./ - type f \| grep -v [file_regex]                                                          | find ./ -type f \| grep -v .html                                                                    |

```sh
# Get a File
scp user@$HOST:/path/to/file /local/directory
```

### less Command
- **Open a file**
```shell
less [filename]
```

- **Search for keywords**
	/ ---> [keyword] ---> Enter

- `q` to quit from less
- `g` to move to the Beginning of the file
- `G` to move to the End of the file.

---
# sed command

| Flags | Description                                               |
| ----- | --------------------------------------------------------- |
| -e    | Add a script that needs to be executed within the pattern |
| -f    | Specify the file containing the pattern                   |
| -E    | Use Extended regex                                        |
| -n    | Suppress the automatic printing                           |

| Mode | Description                      |
| ---- | -------------------------------- |
| s    | Substitute mode (Find & replace) |
| y    | Substitute mode but on bytes     |

| Args          | Description                                                                                                   |
| ------------- | ------------------------------------------------------------------------------------------------------------- |
| /g            | Globally(any pattern change will be affected globally, i.e. throughout the text; generally works with s mode) |
| /i            | Case-insensitive                                                                                              |
| /d            | Delete the pattern found (deletes the entire line; takes no parameters)                                       |
| /p            | Print the matching pattern (a duplicate will occur in output if not suppressed with -n flag.)                 |
| /1,/2,/3,../n | To perform an operation on an nth occurrence in a line (works with s mode)                                    |

```sh
sed '1,3 s/john/JOHN/g' file.txt

# 1,3 : Range of lines
# s : Substitute mode
# john : Pattern to be searched
# JOHN : Pattern to be replaced
# g : Global
```
![[Pasted image 20240621095028.png]]

**Viewing a range of Lines**
![[Pasted image 20240621095631.png]]

**Viewing lines except a given range**
![[Pasted image 20240621095708.png]]

**Viewing multiple ranges of lines inside a file**
![[Pasted image 20240621095728.png]]

```sh
# Remove trailing spaces & replace them with ':'
sed 's/  */\:/g' sed1.txt
```
![[Pasted image 20240621101316.png]]

Insert $=>$ in the start & put numbers in [brackets]
```sh
sed 's/\(^\b[[:alpha:] ]*\)\([[:digit:]]*\)/\=\> \1\[\2\]/g' file.txt
```
![[Pasted image 20240621100937.png]]

---
### Find files based on Hash Values
```sh
hash='[hash value]'
find . -type f -exec sh -c '
   sha1sum "$2" | cut -f 1 -d " " | sed "s|^\\\\||" | grep -Eqi "$1"
' find-sh "$hash" {} \; -print
```


## setfacl Command
https://www.geeksforgeeks.org/linux-setfacl-command-with-example/


### Specify Disk Usage
```sh
# List files as well as folder (Default)
du -a

# List file size in human-readable format
du -h

# Print total size of the directory in the end
du -c

# Specify depthness of directory
du -d 2

# Alternative to ls
du --time -d 1 .
```

