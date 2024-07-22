There are three main ways of discovering content on a website which we'll cover :-
- **Manually
- **Automated**
- **OSINT (Open-Source Intelligence).**

---
# Manual Discovery
1) **robots.txt**
The robots.txt file is a *document* that tells *search engines which pages they are and aren't allowed to show on their search engine results or ban specific search engines from crawling the website* altogether.
These pages may be areas such as *administration portals* or files meant for the *website's customers*. This file gives us a great list of locations on the website that the owners don't want us to discover as penetration testers.

![[Pasted image 20230512201429.png]]

| Keyword    | Function                                                                                                             |
| ---------- | -------------------------------------------------------------------------------------------------------------------- |
| User-Agent | Specify the type of "Crawler" that can index your site (the asterisk being a wildcard, allowing **all User-agents**) |
| Allow      | Specify the directories or file(s) that the "Crawler" **can** index                                                  |
| Disallow   | Specify the directories or file(s) that the "Crawler" **cannot** index                                               | 
| Sitemap    | Provide a reference to where the sitemap is located (Improves SEO)                                                   |



2) **Favicon**
The Favicon is a *small icon* displayed in the browser's *address bar* or tab used for *branding a website*.
Sometimes when *frameworks are used to build a website*, a *favicon that is part of the installation gets leftover*, and *if the website developer doesn't replace this with a custom one*, this can give us a ***clue on what framework is in use***.
![[Pasted image 20230409152008.png]]


Get The *MD5 Hash Value* of the *Favicon* :-
- ***Linux***
```shell
user@machine$ curl https://static-labs.tryhackme.cloud/sites/favicon/images/favicon.ico | md5sum | cut -d ' ' -f 1
```

- ***Windows***
```markup
PS C:\> curl https://static-labs.tryhackme.cloud/sites/favicon/images/favicon.ico -UseBasicParsing -o favicon.ico
PS C:\> Get-FileHash .\favicon.ico -Algorithm MD5 
```

After This, Go to [OWASP favicon database - OWASP](https://wiki.owasp.org/index.php/OWASP_favicon_database) & Lookup The Hash Value To Get The Framework!


3) **Sitemap.xml**
Sitemap.xml file gives a list of *every file* the *website owner wishes to be listed on a search engine*. These can sometimes contain areas of the website that are a bit more *difficult to navigate to or even list some old webpages that the current site no longer uses but are still working behind the scenes*.

![[Pasted image 20230512202636.png]]

**Why are "Sitemaps" so Favourable for Search Engines?**

Search engines are lazy! Well, better yet - search engines have a lot of data to process. The efficiency of how this data is collected is paramount. Resources like "Sitemaps" are extremely helpful for "Crawlers" as the necessary routes to content are already provided! All the crawler has to do is scrape this content - rather than going through the process of manually finding and scraping. Think of it as using a wordlist to find files instead of randomly guessing their names!

4) **HTTP Headers**
Use [[Curl]] Or [[Website Hacking/Tools/Burp Suite/Basics]] to get HTTP Headers which gives information about the *webserver software/version & possible the programming language in use*.



---

# OSINT
There are also *external resources available that can help in discovering information* about your target website; these resources are often referred to as OSINT or (Open-Source Intelligence) as they're *freely available tools that collect information*.

1) [[Google Search Syntax]]
2) **Wappalyzer**
3) **Wayback Machine**
4) [[Shodan]]
5) **Github**
6) **S3 Buckets**
7) **Wigle**

S3 Buckets are a storage service provided by Amazon AWS, allowing people to *save files and even static website content in the cloud accessible over HTTP and HTTPS*. The owner of the files can *set access permissions to either make files public, private and even writable*.

The format of the S3 buckets is:  http(s)://[name].s3.amazonaws.com where [name] is decided by the owner.
One *common automation method* is by using the company name followed by common terms such as [name]-assets, [name]-www, [name]-public, [name]-private, etc.

---


### Automated Discovery
1) [[FFUF]]

2) **dirb**
```shell
user@machine$ dirb http://10.10.79.81/ /usr/share/wordlists/SecLists/Discovery/Web-Content/common.txt
```

3) **JavaScript Files**
Check JS Files as they might contain some **useful website links**.
-   Most basic usage to find endpoints in an online JavaScript file and *output the HTML results to results.html*:
`python linkfinder.py -i https://example.com/1.js -o results.html`

-   CLI/STDOUT output (doesn't use jsbeautifier, which makes it *very fast*):
`python linkfinder.py -i https://example.com/1.js -o cli`

-   Analyzing an entire *domain* and its JS files:
`python linkfinder.py -i https://example.com -d`

-   Burp input (select in target the files you want to save, right click, *Save selected items*, feed that file as input):
`python linkfinder.py -i burpfile -b`

-   Enumerating an entire folder for JavaScript files, while looking for endpoints starting with /api/ and finally saving the results to results.html:
`python linkfinder.py -i 'Desktop/*.js' -r ^/api/ -o results.html`


4) **host** Command
```sh
# Gives IPv4, IPv6 & Mail address of the server
host google.com
```

5) **wafw00f**
```sh
# Check if WAF is present or not
wafw00f google.com
```


6) **Sniper**
All-in-one offensive security framework
```sh
# [*] NORMAL MODE
sniper -t <TARGET>

# [*] NORMAL MODE + OSINT + RECON
sniper -t <TARGET> -o -re

# [*] STEALTH MODE + OSINT + RECON
sniper -t <TARGET> -m stealth -o -re

# [*] DISCOVER MODE
sniper -t <CIDR> -m discover -w <WORSPACE_ALIAS>

# [*] SCAN ONLY SPECIFIC PORT
sniper -t <TARGET> -m port -p <portnum>

# [*] FULLPORTONLY SCAN MODE
sniper -t <TARGET> -fp

# [*] WEB MODE - PORT 80 + 443 ONLY!
sniper -t <TARGET> -m web

# [*] HTTP WEB PORT MODE
sniper -t <TARGET> -m webporthttp -p <port>

# [*] HTTPS WEB PORT MODE
sniper -t <TARGET> -m webporthttps -p <port>

# [*] HTTP WEBSCAN MODE
sniper -t <TARGET> -m webscan 

# [*] ENABLE BRUTEFORCE
sniper -t <TARGET> -b

# [*] AIRSTRIKE MODE
sniper -f targets.txt -m airstrike

# [*] NUKE MODE WITH TARGET LIST, BRUTEFORCE ENABLED, FULLPORTSCAN ENABLED, OSINT ENABLED, RECON ENABLED, WORKSPACE & LOOT ENABLED
sniper -f targets.txt -m nuke -w <WORKSPACE_ALIAS>

# [*] MASS PORT SCAN MODE
sniper -f targets.txt -m massportscan

# [*] MASS WEB SCAN MODE
sniper -f targets.txt -m massweb

# [*] MASS WEBSCAN SCAN MODE
sniper -f targets.txt -m masswebscan

# [*] MASS VULN SCAN MODE
sniper -f targets.txt -m massvulnscan

# [*] PORT SCAN MODE
sniper -t <TARGET> -m port -p <PORT_NUM>

# [*] LIST WORKSPACES
sniper --list

# [*] DELETE WORKSPACE
sniper -w <WORKSPACE_ALIAS> -d

# [*] DELETE HOST FROM WORKSPACE
sniper -w <WORKSPACE_ALIAS> -t <TARGET> -dh

# [*] GET SNIPER SCAN STATUS
sniper --status

# [*] LOOT REIMPORT FUNCTION
sniper -w <WORKSPACE_ALIAS> --reimport

# [*] LOOT REIMPORTALL FUNCTION
sniper -w <WORKSPACE_ALIAS> --reimportall

# [*] LOOT REIMPORT FUNCTION
sniper -w <WORKSPACE_ALIAS> --reload

# [*] LOOT EXPORT FUNCTION
sniper -w <WORKSPACE_ALIAS> --export

# [*] SCHEDULED SCANS
sniper -w <WORKSPACE_ALIAS> -s daily|weekly|monthly

# [*] USE A CUSTOM CONFIG
sniper -c /path/to/sniper.conf -t <TARGET> -w <WORKSPACE_ALIAS>

# [*] UPDATE SNIPER
sniper -u
```
