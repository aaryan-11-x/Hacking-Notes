**Sensitive Data Exposure**, also known as *Information Leakage*, is when a website *unintentionally reveals sensitive information*!
Websites may leak all kinds of information, including :-
- Data about other users, such as *usernames or financial information*
- *Sensitive commerical or business* data
- Usernames and Passwords


---
# Flat-File Database
In a production environment it is common to see databases set up on dedicated servers, running a database service such as MySQL or MariaDB; however, *databases can also be stored as files*. These databases are referred to as **Flat-File** databases, as they are *stored as a single file on the computer*. This is much *easier than setting up a full database server*, and so could potentially be *seen in smaller web applications*.

 **Flat-File** databases are stored as a *file on the disk of a computer*. Usually this would not be a problem for a webapp, but *what happens if the database is stored underneath the root directory of the website* (i.e. one of the files that a user connecting to the website is able to access)? Well, we can *download it and query it on our own machine*, with **full access** to everything in the database. **Sensitive Data Exposure** indeed!

The most common (and simplest) format of flat-file database is an _sqlite_ database. This client is called `sqlite3`, and is installed by default on Kali Linux.

![[Pasted image 20230411180245.png]]

From here we can see the tables in the database by using the `.tables` command:
![[Pasted image 20230411180308.png]]

First let's use `PRAGMA table_info(customers);` to see the table information, then we'll use `SELECT * FROM customers;` to dump the information from the table

![[Pasted image 20230411180440.png]]

Take The First Row :-
- `custID` : 0
- `custName` : Joy Paulson
- `creditCard` : 4916 9012 2231 7905
- `password hash` : 5f4dcc3b5aa765d61d8327deb882cf99