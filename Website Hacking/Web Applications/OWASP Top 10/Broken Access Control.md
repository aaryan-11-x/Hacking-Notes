For Theory, Go To [Access control vulnerabilities and privilege escalation | Web Security Academy (portswigger.net)](https://portswigger.net/web-security/access-control)

# Vertical privilege escalation

#### Unprotected functionality
- Go to robots.txt
OR
- Bruteforce Directories to find the **admin panel**
OR
- Check the **Javascript Code**, It might leak the **admin panel**



#### Parameter-based access control methods
- Change the parameters in GET or POST Request
`https://insecure-website.com/login/home.jsp?admin=true`
`https://insecure-website.com/login/home.jsp?role=1`


#### Broken access control resulting from platform misconfiguration
1) An application might configure rules like the following:
**DENY: POST, /admin/deleteUser, managers**
This rule denies access to the `POST` method on the URL `/admin/deleteUser`, for users in the managers group. Various things can go wrong in this situation, leading to access control bypasses.

To Bypass This, Use `X-Original-URL` and `X-Rewrite-URL`
Example: If Deletion is Going From *GET Request*
![[Pasted image 20230515144555.png]]


2) **Changing the HTTP Method**

- If POST Method is Denied, Trying doing it through *GET* Method by **Right Click ---> Change Request Method**
- Or By Using Other HTTP Methods Like *PUT*


#### Broken access control resulting from URL-matching discrepancies
- Discrepancies can arise if developers using the Spring framework have enabled the `useSuffixPatternMatch` option. This allows paths with an arbitrary file extension to be mapped to an equivalent endpoint with no file extension. In other words, a request to `/admin/deleteUser.anything` would still match the `/admin/deleteUser` pattern. *Prior to Spring 5.3*, this option is *enabled by default*.

- On other systems, you may encounter discrepancies in whether `/admin/deleteUser` and `/admin/deleteUser/` are *treated as a distinct endpoints*. In this case, you may be able to bypass access controls simply by *appending a trailing slash to the path*.

---
# Horizontal privilege escalation
1) **Change** the ID Parameter
`https://insecure-website.com/myaccount?id=123`

2) If Application uses **GUID to Identify Users**, Check if the GUID is being *Disclosed somewhere* in the application
Ex: If the GUID is gettting disclosed in the GET or POST Methods, use that GUID to login to different users.

3) Check for **Redirect Responses**, They *Might Leak* Some Data

---

# Insecure direct object references (IDOR)
For Theory, [Click Here](https://portswigger.net/web-security/access-control/idor) 


### IDOR vulnerability with direct reference to database objects

Consider a website that uses the following URL to access the customer account page, by retrieving information from the back-end database:

`https://insecure-website.com/customer_account?customer_number=132355`

Here, the customer number is used directly as a record index in queries that are performed on the back-end database. If no other controls are in place, an attacker can simply modify the `customer_number value`, *bypassing access controls* to view the records of other customers. This is an **example of an IDOR vulnerability** leading to horizontal privilege escalation.


### IDOR vulnerability with direct reference to static files

IDOR vulnerabilities often arise when sensitive resources are located in static files on the server-side filesystem. For example, a *website might save chat message transcripts* to disk *using an incrementing filename*, and *allow users to retrieve these by visiting a URL* like the following:

`https://insecure-website.com/static/12144.txt`

In this situation, an attacker can simply modify the filename to retrieve a transcript created by another user and potentially obtain user credentials and other sensitive data.

---

### Access control vulnerabilities in multi-step processes

Many web sites *implement important functions over a series of steps*. This is often done when a variety of inputs or options need to be captured, or when the user needs to review and confirm details before the action is performed. For example, administrative function to update user details might involve the following steps:

1.  Load form containing details for a specific user.
2.  Submit changes.
3.  Review the changes and confirm.

Here, the **web site assumes that a user will only reach step 3 if they have already completed the first two steps**, which are properly controlled. Here, an *attacker can gain unauthorized access to the function by skipping the first two steps and directly submitting the request for the third step with the required parameters*.


### Referer-based access control

Some websites base access controls on the `Referer` header submitted in the HTTP request. The `Referer` header is generally added to requests by browsers to indicate the page from which a request was initiated.

For example, suppose an application robustly enforces access control over the main administrative page at `/admin`, but for sub-pages such as `/admin/deleteUser` only inspects the `Referer` header. If the `Referer` header **contains the main** `/admin` URL, then the **request is allowed**.

In this situation, since the `Referer` header can be fully controlled by an attacker, they can forge direct requests to sensitive sub-pages, supplying the required `Referer` header, and so gain unauthorized access.


### Location-based access control

Some web sites enforce access controls over resources based on the *user's geographical location*. This can apply, for example, to *banking applications or media services where state legislation or business restrictions apply*. These access controls can often be *circumvented by the use of web proxies, VPNs, or manipulation of client-side geolocation mechanisms*.

---


