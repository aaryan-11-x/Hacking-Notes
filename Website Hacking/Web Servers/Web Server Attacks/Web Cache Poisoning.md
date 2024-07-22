#### Cache Keys
Caches *identify equivalent requests by comparing a predefined subset of the request's components*, known collectively as the **"cache key"**. Typically, this would contain the `request line` & `Host Header`. Components of the request that are not included in the cache key are said to be **"unkeyed"**.

If the *cache key of an incoming request matches the key of a previous request*, it will serve a copy of the cached response that was generated for the original request. This applies to all subsequent requests with the matching cache key, *until the cached response expires.*

Here, Cache-Control is 1800, which means *cache will expire after 1800 seconds*.

![[Pasted image 20230406131026.png]]

Always Inject Your *Malicious Payload* In the **"Un-Keyed Input"**
![[Pasted image 20230406131452.png]]

Thus, **All other people who visit the website will get the JS!**

### Constructing a Web Cache Poisoning Attack
***Identify & Evaluate Unkeyed Inputs***
- Use Param Miner ([[Website Hacking/Tools/Burp Suite/Basics]] Extension)
- Right Click ----> "Guess Headers"
- Check For Output: "Extensions" ----> "Installed" ----> "Param Miner" ----> "Output"

%% Note: Most of the times, Unkeyed Inputs are `X-Forwarded-Host` %%

#### Exploiting Cache Design Flaws
Websites are vulnerable to web cache poisoning if they *handle unkeyed input* in an unsafe way and allow the *subsequent HTTP responses* to be cached.

1) **XSS Attack**
Consider the following request and response:

=== multi-column-start: ID_hglt
```column-settings
Number of Columns: 2
Largest Column: standard
```
##### Request
GET /en?region=uk HTTP/1.1
Host: innocent-website.com
X-Forwarded-Host: innocent-website.co.uk

=== end-column ===
##### Response
HTTP/1.1 200 OK
Cache-Control: public
< meta property="og:image" content="https://innocent-website.co.uk/cms/social.png" />

=== multi-column-end

Here, the value of the `X-Forwarded-Host` header is being used to *dynamically generate an Open Graph image URL*, which is then reflected in the response. Crucially for web cache poisoning, the `X-Forwarded-Host` header is often *unkeyed*.

Let's *Poison The Cache* with a simple XSS Payload!

=== multi-column-start: ID_rs5e
```column-settings
Number of Columns: 2
Largest Column: standard
```
##### Request
GET /en?region=uk HTTP/1.1
Host: innocent-website.com
X-Forwarded-Host: a.">< script>alert(1)< /script>"

=== end-column ===
###### Response
HTTP/1.1 200 OK
Cache-Control: public
< meta property="og:image" content="https://a.">< script>alert(1)< /script>"/cms/social.png" />

=== multi-column-end

If this response was cached, all users who accessed `/en?region=uk` would be served this *XSS payload*.


2) **Exploit Unsafe Handling of Resource Imports**
Some websites use *unkeyed headers to dynamically generate URLs for importing resources*, such as externally hosted JavaScript files. In this case, if an ***attacker changes the value of the appropriate header to a domain that they control, they could potentially manipulate the URL to point to their own malicious JavaScript file instead***.

Here, `X-Forwarded-Host` is used to generate a URL
![[Pasted image 20230407004841.png]]

Craft Your Own Malicious URL
![[Pasted image 20230407010556.png]]

Paste the Subdomain in `X-Forwarded-Host`