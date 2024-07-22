Firefox < 32 (key3.db, signons.sqlite)
Firefox >=32 (key3.db, logins.json)
Firefox >=58.0.2 (key4.db, logins.json)
Firefox >=75.0 (sha1 pbkdf2 sha256 aes256 cbc used by key4.db, logins.json)
at least Thunderbird 68.7.0, likely other versions
# firefox_decrypt

```sh
python firefox_decrypt.py [directory]

# Ex: python firefox_decrypt.py /root/CTF/Rooms/ProvingGrounds/insanityhosting/esmhp32w.default-default/
## In /Downloads/
```

###### Non fatal password decryption
By default, encountering a corrupted username or password will *abort decryption*. Since version 1.1.0 there is now `--non-fatal-decryption` that tolerates individual failures.
```sh
$ python firefox_decrypt.py --non-fatal-decryption
(...)
Website:   https://github.com
Username: '*** decryption failed ***'
Password: '*** decryption failed ***'
```

---
# firepwd

```sh
python firepwd.py -d /root/CTF/Rooms/ProvingGrounds/insanityhosting/esmhp32w.default-default/

# To give Master Password
python firepwd.py -p 'MISC*' -d mozilla_db/
```

---
# [[msfconsole]]

https://www.hackingarticles.in/exploit-save-password-in-mozilla-firefox-in-remote-windows-linux-or-mac-pc/