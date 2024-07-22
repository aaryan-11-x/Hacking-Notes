Shodan scans the whole internet and indexes the services run on each IP address.

### Finding services
Let’s say we are performing a pentest on a company, and we want to find out what services one of their servers run. We need to grab their **IP address**.
- We can *ping tryhackme.com* and the ping response will tell us their **IP address**.
- *TryHackMe* runs on *Cloudflare in the United States* and they have *many ports open*.
- *Cloudflare acts as a proxy between TryHackMe and their real servers*. If we were pentesting a *large company, this isn’t helpful*. We need some way to get their IP addresses.
- We can do this using *Autonomous System Numbers*.


### Autonomous System Numbers
An **Autonomous System Number (ASN)** is a global identifier of a range of IP addresses. If you are an enormous company like *Google* you will likely *have your own ASN* for all of the *IP addresses you own*.
- Use **Netcraft** for getting **ASN** of a company.
- Then Lookup for the **ASN Number** On Shodan Using ASN:AS[number]

Knowing the ASN is helpful, because we can search *Shodan for things such as coffee makers or vulnerable computers within our ASN*, which we know (if we are a large company) is on our network.


## Filters
On the Shodan.io homepage, we can click on `explore` to view the most up voted search queries.


1) asn:[number]
To find the IP Addresses associated with asn number!


2) product:[servicename]
Example -  **product:MySQL** for searching MYSQL Database


3) vuln:[exploitID]
Example - **vuln:ms17-010** to search for *IP addresses vulnerable* to *Eternal Blue*
**NOTE: This is only available for academic or business users, to prevent bad actors from abusing this!**


4) city:["City name"]
To Search by City



#### Dorks
1) has_screenshot:true encrypted attention
Uses optical character recognition and remote desktop to *find machines compromised by ransomware* on the internet.


2) vuln:CVE-2014-0160
Internet connected machines *vulnerable to heartbleed*

3) http.favicon.hash:-1776962843
*Solar Winds Supply Chain Attack by using Favicons*