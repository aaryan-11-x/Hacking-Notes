**Custom Wordlist Generator**
It crawls the entire webpage and hunts for words!

```sh
# Help Page

-c     Count for each word found
-d     Increase depth (Default 2)
-e     Crawl for emails
-H     Custom Header (name:value)
-m     Minimum word length to search for (Default 3)
-x     Maximum word length allowed
-n     Dont crawl for words
-t     Increase threads
-v     Verbosity

--lowercase: Lowercase words only
--with-numbers: Letters & numbers

--auth_type: Digest or basic.
--auth_user: Authentication username.
--auth_pass: Authentication password.
--ua: User Agent
```

##### Generate a Wordlist
```sh
cewl [url] -w wordlist.txt
```


##### Generate lowercase Wordlist
```sh
cewl [url] --lowercase -w list.txt
```


#### Generate wordlist with letters & numbers
```sh
cewl [url] --with-numbers -w list.txt
```


#### Crawl for Emails only
```sh
cewl [url] -n -e
```


#### Crawl for wordlist with Authentication
```sh
cewl [url] --auth_type: basic --auth_user: [username] --auth_pass: [password]
```