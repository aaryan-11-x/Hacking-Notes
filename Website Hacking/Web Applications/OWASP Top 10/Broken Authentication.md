# Vulnerabilities in password-based login
-   **Status codes**: During a brute-force attack, the returned HTTP status code is likely to be the same for the vast majority of guesses because most of them will be wrong. If a guess returns a different status code, this is a strong indication that the username was correct. It is best practice for websites to always return the same status code regardless of the outcome, but this practice is not always followed.
-   **Error messages**: Sometimes the returned error message is *different depending on whether both the username AND password are incorrect or only the password was incorrect*. It is best practice for websites to use identical, generic messages in both cases, but small typing errors sometimes creep in. Just one character out of place makes the two messages distinct, even in cases where the character is not visible on the rendered page.
- **Response Times**: If most of the requests were handled with a similar response time, any that deviate from this suggest that something different was happening behind the scenes. This is another indication that the guessed username might be correct. For example, **a website might only check whether the password is correct if the username is valid. This extra step might cause a slight increase in the response time.** This may be subtle, but an attacker can *make this delay more obvious* by **entering an excessively long password** that the website takes noticeably longer to handle.


### Bypass IP-Based Bruteforce Protection
- Use **X-Forwarded-For** in Your Request
- Put any IP in the X-Forwarded-For Header & send the request
- If you don't get any error in your response, the website *supports* it. This allows you to spoof your IP Address.
- Use [[Website Hacking/Tools/Burp Suite/Basics]] to Frequently Change your *IP Address* using #intruder 
- Use *Pitchfork*, Enumerate Username First Then Password.

OR

- Login Attempts **Resets** when you Enter the *Correct Credentials*
- Insert the *Correct Username & Password* after each word in your wordlist(s)


### Bypass User Account Lockout
[Username enumeration via account lock - Cyber Security / Ethical Hacking (gitbook.io)](https://666isildur.gitbook.io/ethical-hacking/web-app-pentesting/authentication/vulnerabilities-in-password-based-login/username-enumeration-via-account-lock)



### JSON Input Field
- If there is a JSON Input Field, try *putting all passwords in an array of the value pair* of the "password" key.
- Same goes for Username or Other Input Fields!


---
# Vulnerabilities in multi-factor authentication

## Flawed two-factor verification logic
For example, the *user logs in with their normal credentials* in the first step as follows :-

`POST /login-steps/first HTTP/1.1
 `Host: vulnerable-website.com 
 `... `
 `username=carlos&password=qwerty`

They are then *assigned a cookie that relates to their account*, before being taken to the second step of the login process :-

`HTTP/1.1 200 OK 
`Set-Cookie: account=carlos`
`GET /login-steps/second HTTP/1.1`
`Cookie: account=carlos`

When submitting the verification code, the *request uses this cookie to determine which account the user is trying to access* :-

`POST /login-steps/second HTTP/1.1
`Host: vulnerable-website.com`
`Cookie: account=carlos`
`...`
`verification-code=123456`

In this case, an attacker could log in using their own credentials but then change the value of the `account` cookie to any arbitrary username when submitting the verification code.

`POST /login-steps/second HTTP/1.1
`Host: vulnerable-website.com`
`Cookie: account=[victim-user]`
`...`
`verification-code=123456`

This is extremely dangerous if the attacker is then able to brute-force the verification code as it would allow them to log in to arbitrary users' accounts based entirely on their username. They would **never even need to know the user's password**.



## Brute-forcing 2FA verification codes when CSRF Token Expires

Use **Macros** of [[Website Hacking/Tools/Burp Suite/Basics]] to Generate a new **CSRF Token** from the previous Response everytime the token *becomes invalid*.


---

# Vulnerabilities in other authentication mechanisms

## Exploiting "Remember Me"

This functionality is often implemented by generating a "remember me" token of some kind, which is then stored in a persistent cookie. As possessing this cookie effectively allows you to bypass the entire login process, it is *best practice for this cookie to be impractical to guess*. However, some websites *generate this cookie based on a predictable concatenation of static values*, such as the **username and a timestamp**. Some even use the **password as part of the cookie**. This approach is particularly dangerous if an *attacker is able to create their own account* because *they can study their own cookie* and potentially deduce how it is generated. Once **they work out the formula**, they can try to brute-force other users' cookies to gain access to their accounts.

**Encoding Cookie Techniques to Look Out For** :-
- Base64
- MD5 Hash
- Username/Password With Timestamp
- Username/Password in Reverse
- Shifting characters of Username/Password with one character Left or Right


## Resetting user passwords
It should go without saying that *sending users their current password should never be possible* if a website handles passwords securely in the first place. Instead, some websites *generate a new password and send this to the user via email*.

Generally speaking, *sending persistent passwords over insecure channels is to be avoided*. In this case, the security relies on either the generated password expiring after a very short period, or the user changing their password again immediately. Otherwise, this approach is highly susceptible to [[Application Level MITM & MITB]].

**Email** is also generally *not considered secure given that inboxes are both persistent and not really designed for secure storage of confidential information*. Many users also automatically sync their inbox between multiple devices across insecure channels.


### Resetting passwords using a URL

`http://vulnerable-website.com/reset-password?user=victim-user`

1) In this example, an *attacker could change the user parameter to refer to any username they have identified*. They would then be taken straight to a page where they can potentially set a new password for this arbitrary user.

A better implementation of this process is to *generate a high-entropy, hard-to-guess token and create the reset URL* based on that. In the best case scenario, this URL should provide no hints about which user's password is being reset :-

`http://vulnerable-website.com/reset-password?token=a0ba0d1cb3b63d13822572fcff1a241895d893f659164d4cc550b421ebdd48a8`

However, some **websites fail to also validate the token again when the reset form is submitted**. In this case, an attacker could simply visit the reset form from their own account, delete the token, and leverage this page to reset an arbitrary user's password.


2) If Token is getting validated, then another form of attack is **getting the host of the forgot password session of that user**. This can be done by [[Host Header Injection]]
If a **Middleware** is present, it'll *convert POST to it's subsequent GET Request*. By using X-Forwarded-Host, we will get the *GET Request on our domain instead of the original domain*.


---
# Improper Cookie Validation
Let's take this example :-
```js
if (statusOrCookie === "Incorrect credentials") {
        loginStatus.textContent = "Incorrect Credentials"
        passwordBox.value=""
    } else {
        Cookies.set("SessionToken",statusOrCookie)
        window.location = "/admin"
    }
}
```
Here, the application isn't checking what's in `SessionToken`, it just checks for the field. So, we can randomly insert a cookie on `/admin` to get access.

```sh
# In cURL
curl http://10.10.66.103/admin -b "SessionToken=[random_cookie]" -L
```
```js
// In Browser (Console)
Cookies.set("SessionToken", "")
```
```
Or In Burp Suite
```
