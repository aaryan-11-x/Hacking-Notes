Directory Listing a Website which has *authentication*
```sh
dirsearch -u http://vulnet.thm/ -w /usr/share/wordlists/dirbuster/directory-list-2.3-small.txt --auth-type=basic --auth=[username]:[password] -e php,txt,html
```