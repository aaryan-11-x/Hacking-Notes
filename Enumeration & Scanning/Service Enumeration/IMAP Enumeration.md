https://book.hacktricks.xyz/network-services-pentesting/pentesting-imap

```sh
Login
    A1 LOGIN username password
Values can be quoted to enclose spaces and special characters. A " must then be escape with a \
    A1 LOGIN "username" "password"

List Folders/Mailboxes
    A1 LIST "" *
    A1 LIST INBOX *
    A1 LIST "Archive" *
    A2 LIST "" "*"
	a3 EXAMINE INBOX
	a4 FETCH 1 BODY[]
	a5 FETCH 2 BODY[]


List Subscribed Mailboxes
    A1 LSUB "" *

Status of Mailbox (There are more flags than the ones listed)
    A1 STATUS INBOX (MESSAGES UNSEEN RECENT)

Select a mailbox
    A1 SELECT INBOX

List messages
    A1 FETCH 1:* (FLAGS)
    A1 UID FETCH 1:* (FLAGS)

Retrieve Message Content
    A1 FETCH 2 body[text]
    A1 FETCH 2 all
    A1 UID FETCH 102 (UID RFC822.SIZE BODY.PEEK[])

Close Mailbox
    A1 CLOSE

Logout
    A1 LOGOUT
```

---
## Curl
```sh
# Listing mailboxes (imap command LIST "" "*")
curl -k 'imaps://1.2.3.4/' --user user:pass

# Listing messages in a mailbox (imap command SELECT INBOX and then SEARCH ALL)
curl -k 'imaps://1.2.3.4/INBOX?ALL' --user user:pass

# Use unique ID to access messages
curl -k 'imaps://1.2.3.4/INBOX;UID=1' --user user:pass
```