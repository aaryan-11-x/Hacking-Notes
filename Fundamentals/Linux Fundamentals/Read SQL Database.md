# MYSQL

```shell
systemctl enable mysql
systemctl start mysql
```

##### Connect to remote SQL database
```shell
# No space b/w -p & password
mysql -u [username] -p[password] -h [host ip] 
# -P [Port number if default port isnt used]
```
![[Pasted image 20230606162415.png]]


##### Open SQL database file locally
To open mysql file/files locally, simply *change to the directory of the mysql file* and type mysql as shown below.
```sh
1) mysql -u [username] -p
2) source [sql filename]
3) SHOW DATABASES;              # Display the Database
4) USE [database name]          # Choose the Database to View
5) SHOW TABLES;                 # Displaying the tables in the selected database
6) DESCRIBE [table name]        # Describing the table data structure
.
.
.
And Further are SQL Commands
```

---
# PostgreSQL
https://hasura.io/blog/top-psql-commands-and-flags-you-need-to-know-postgresql