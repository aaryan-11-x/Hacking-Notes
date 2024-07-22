<mark style="background: #ADCCFFA6;">Burp Suite</mark> is a framework written in <mark style="background: #FFB86CA6;">Java</mark> that aims to provide a one-stop-shop for **web application penetration testing**.

<mark style="background: #ADCCFFA6;">Burp Suite</mark> is also very commonly used when assessing *mobile applications*, as the same features which make it so attractive for web app testing translate almost perfectly into testing the *APIs* powering most *mobile apps*.

At the simplest level, Burp can *capture and manipulate* all of the *traffic between an attacker and a webserver*: this is the core of the framework. After capturing requests, we can choose to send them to various other parts of the Burp Suite framework.


# Features
-   **Proxy:** The most well-known aspect of Burp Suite, the Burp Proxy allows us to *intercept and modify requests*& responses when interacting with web applications. #proxy

-   **Target:** Used To Set The *Scope Of The Project*.

-   **Repeater:** Allows us to capture, modify, then resend the same request numerous times. This feature can be absolutely invaluable, especially when we need to craft a payload through trial and error (e.g. in an *SQLi* or when *testing the functionality of an endpoint for flaws*) #repeater 

-   **Intruder:** Although harshly rate-limited in Burp Community, Intruder allows us to *spray an endpoint with requests*. This is often used for <mark style="background: #FF5582A6;">bruteforce attacks</mark> or to fuzz endpoints. #intruder

-   **Decoder:**  Helps In *Decoding* captured information, or *encoding a payload* prior to sending it to the target. 

-   **Comparer:** Comparer allows us to *compare two pieces of data* at either word or byte level. 
   
-   **Sequencer:** We usually use Sequencer when *assessing the randomness of tokens such as session cookie values* or other supposedly random generated data. If the algorithm is not generating secure random values, then this could open up some devastating avenues for attack.


---

# Dashboard

![[Pasted image 20230326163301.png]]

1.  The **Tasks** menu allows us to *define background tasks that Burp Suite will run* whilst we use the application. 
2.  The **Event log** tells us what *Burp Suite is doing (e.g. starting the Proxy)*, as well as information about any connections that we are making through Burp.
3.  The **Issue Activity** section is *exclusive to Burp Pro*. It won't give us anything using Burp Community, but in Burp Professional it would list all of the vulnerabilities found by the automated scanner. These would be ranked by severity and filterable by how sure Burp is that the component is vulnerable.
4.  The **Advisory** section gives more *information about the vulnerabilities found*, as well as references and suggested remediations. These could then be exported into a report.


Tabs can also be *popped out into separate windows* should you prefer to view multiple tabs separately. This can be done by clicking <mark style="background: #FFB86CA6;">"Window"</mark> in the application menu at the top of the screen, then choosing to <mark style="background: #CACFD9A6;">"Detach"</mark> tabs:  
![Demonstration of the menu allowing you to detach windows](https://tryhackme-images.s3.amazonaws.com/user-uploads/5d9e176315f8850e719252ed/room-content/46310bb85e12e7b7ff2c242c168cd547.png)


**Happy Path** Is The Term For Browsing The Web Application as a *Low Privilege User*.



| Shortcut         | Does                                      |
| ---------------- | ----------------------------------------- |
| Ctrl + Shift + D | Switch to the Dashboard                   |
| Ctrl + Shift + T | Switch to the Target tab                  |
| Ctrl + Shift + P | Switch to the Proxy tab                   |
| Ctrl + Shift + I | Switch to the Intruder tab                |
| Ctrl + Shift + R | Switch to the Repeater tab                |
| Ctrl + F         | Forward Intercepted Request to the Target |
| Ctrl + R         | Send To Repeater                          |
| Ctrl + I         | Send To Intruder                          | 


---

# Scoping
- Allowing Burp to capture _everything_ can quickly become a massive pain!
- The Scope sub-tab allows us to control what we are targeting by either _Including_ or _Excluding_ domains / IPs

![[7e11c5dec4dba4336927aa7561e5c793.gif]]


# Site Map
**Site map** allows us to map out the apps we are targeting in a tree structure. Every page that we visit will show up here, allowing us to automatically generate a site map for the target simply by browsing around the web app. 
The **Site map** can be especially useful if we want to *map out an API*, as whenever we visit a page, any API endpoints that the page retrieves data from whilst loading will show up here.
![[Pasted image 20230326200903.png]]

---

# Intruder
#intruder 

**IMPORTANT**: While Bruteforcing sites with `302 code` for *all requests*, check the `Location` Header, if it changes, then consider the login to be successful!

1) Send Request To The Intruder
2) For Putting A Variable, Select a String From The **Get Request** And Press **§Add§** On The Right Side.
3) Choose Attack Type :-
	1. **Sniper**
		- Bruteforces The Payload Set On the Payload Position *one at a time* (Ex: 8 Highlighted Requests & 5 words ----> 40 Times Bruteforce)
		- Go To Payloads --> Select Payload Set & Type --> Select Payload Settings --> Start Attack
	2. **Battering Ram**
		- Bruteforces The Payload Set On *All Payload Positions*
	3. **PitchFork**
		- *Different Payload Set For Different Payload Positions*
		- Example :-

| Payload 1 | Payload 2 |
| --------- | --------- |
| 1         | a         |
| 2         | b         |
| 3         | c         |
  1 --> a,  2 --> b,  3--> c
_
	4. **Cluster Bomb**
		- Uses *Multiple Payload Sets*, *Different Payload Set for Each Position*
		- Ex: There are 2 Payload Positions, 1st has Numbers & 2nd has alphanumeric characters. Then 1 -> a, 1 -> b, 1-> c............. 9->z


To Search for *Responses* Having a *Specific String* :-
- On the **Settings** tab, under **Grep - Extract**, click **Add**. 
- Use the mouse to highlight the **text content** of the message, the other settings will be automatically adjusted.
- Click **OK** and then start the attack.
- When the attack is finished, notice that there is an **additional column** containing the error message you extracted. **Sort the results using this column** to notice that one of them is subtly different.


***Payload Processing Rules*** :-
- **Hash**: To Hash the Payloads into different Hashing Algorithms
- **Add Prefix**: To add a string in the beginning of a Payload
- **Add Suffix**: To add a string at the end of a Payload
- **Encode** : Encode the Payloads into your Desired Format


***Resource Pool*** :-
To send only 'n' number of requests at a time, `Create a New Resource Pool --> Maximum Concurrent Requests = n`


---
# Repeater
#repeater

We have four <mark style="background: #BBFABBA6;">Display</mark> options here:-
1.  **Pretty:** This is the default option. It takes the *raw response and attempts to beautify it slightly*, making it easier to read.
2.  **Raw:** The pure, *un-beautified response* from the server.
3.  **Hex:** This view takes the raw response and gives us a byte view of it -- especially useful if the response is a binary file.
4.  **Render:** The render view renders the page as it would appear in your browser.

Just to the right of the view buttons is the **"Show non-printable characters"** button (`\n`)
every line in the response will end with the characters `\r\n` -- these signify a **carriage return followed by a newline** and are part of how HTTP headers are interpreted.


<mark style="background: #BBFABBA6;">Inspector</mark> Options :-
-   **Request Attributes**, we can edit the parts of the request that deal with location, method and protocol; e.g. changing the resource we are looking to retrieve, altering the request from GET to another HTTP method, or switching protocol from HTTP/1 to HTTP/2
![[Pasted image 20230326210224.png]]

-   **Query Parameters**, which refer to data being sent to the server in the URL. For example, in a GET request to `https://admin.tryhackme.com/?redirect=false`, there is a query parameter called "redirect" with a value of "false".  
-   **Body Parameters**, which do the same thing as Query Parameters, but for POST requests. Anything that we send as data in a POST request will show up in this section, once again allowing us to modify the parameters before re-sending.
-   **Request Headers** allow us to view, access, and modify (including outright adding or removing) any of the headers being sent with our requests. Editing these can be very useful when attempting to see how a webserver will respond to unexpected headers.  
-   **Response Headers** show us the headers that the server sent back in response to our request. These cannot be edited (as we can't control what headers the server returns to us!).