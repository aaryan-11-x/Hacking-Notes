**Binwalk** is a popular open-source tool in Kali Linux used for *analyzing and extracting firmware images, as well as searching for embedded file systems, executables, and other data types within binary files.* It supports a wide range of file types and can identify and extract various file formats such as firmware, kernel images, and compressed archives, among others.

Binwalk can also be used to *detect and extract code and data that is hidden inside binary files such as malware, shell scripts, and other types of hidden files*. It can perform signature scanning and entropy analysis to identify the type of data present in the file and extract it accordingly.

***Binwalk Can Be Used On Any File!***

###### Command
binwalk -e [filename] --run-as=root

```sh
-qe : Quiet mode extract
```