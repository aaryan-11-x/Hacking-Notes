# Writable /etc/shadow file
Generate a new password hash with a password of your choice:
```sh
mkpasswd -m sha-512 [newpasswordhere]
```
Edit the `/etc/shadow file` and replace the original root user's password hash with the one you just generated.

Switch to the root user, using the new password:
```sh
su root
```

---
# Writable /etc/passwd file
Generate a new password hash with a password of your choice (Encryption algorithm used is *crypt*):
```sh
openssl passwd newpasswordhere
```

Edit the `/etc/passwd` file and place the generated password hash between the *first and second colon (:) of the root user's row (replacing the "x").*
Switch to the root user, using the new password:
```sh
su root
```

Alternatively, copy the root user's row and append it to the bottom of the file, changing the first instance of the word "root" to `newroot` and placing the generated password hash between the first and second colon (replacing the "x").
Now switch to the `newroot` user, using the new password:
```sh
su newroot
```