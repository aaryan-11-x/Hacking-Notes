# CAUTION
It is important to know that when reading an email header every line can be forged, so only the **Received:** lines that are created by your service or computer should be completely trusted.

### From

-   This displays who the message is from, however, this can be easily forged and can be the least reliable.

### Subject

-   This is what the sender placed as a topic of the email content.

### Date

-   This shows the date and time the email message was composed.

### To

-   This shows to whom the message was addressed, but may not contain the recipient's address.

### Return-Path

-   The email address for return mail. This is the same as "Reply-To:".

### Envelope-To

-   This header shows that this email was delivered to the mailbox of a subscriber whose email address is user@example.com.

### Delivery Date

-   This shows the date and time at which the email was received by your (mt) service or email client.

### Received

-   The received is the most important part of the email header and is usually the most reliable. They form a list of all the servers/computers through which the message traveled in order to reach you.
    
    The received lines are best read from bottom to top. That is, the first "Received:" line is your own system or mail server. The last "Received:" line is where the mail originated. Each mail system has their own style of "Received:" line. A "Received:" line typically identifies the machine that received the mail and the machine from which the mail was received.
    

### Dkim-Signature & Domainkey-Signature

-   These are related to domain keys which are currently not supported by (mt) Media Temple services. You can learn more about these by visiting: [http://en.wikipedia.org/wiki/DomainKeys](http://en.wikipedia.org/wiki/DomainKeys).

### Message-id

-   A unique string assigned by the mail system when the message is first created. These can easily be forged.

### Mime-Version

-   Multipurpose Internet Mail Extensions (MIME) is an [Internet standard](http://en.wikipedia.org/wiki/Internet_standard) that extends the format of [email](http://en.wikipedia.org/wiki/Electronic_mail). Please see [http://en.wikipedia.org/wiki/MIME](http://en.wikipedia.org/wiki/MIME) for more details.

### Content-Type

-   Generally, this will tell you the format of the message, such as html or plaintext.

### X-Spam-Status

-   Displays a spam score created by your service or mail client.

### X-Spam-Level

-   Displays a spam score usually created by your service or mail client.

### Message Body

-   This is the actual content of the email itself, written by the sender.

## FINDING THE ORIGINAL SENDER

The easiest way for finding the original sender is by looking for the **X-Originating-IP** header. This header is important since it tells you the IP address of the computer that had sent the email. If you cannot find the **X-Originating-IP** header, then you will have to sift through the **Received** headers to find the sender's IP address.

Once the email sender's IP address is found, you can search for it at [http://www.arin.net/](http://www.arin.net/). You should now be given results letting you know to which ISP (Internet Service Provider) or webhost the IP address belongs. Now, if you are tracking a spam email, you can send a complaint to the owner of the originating IP address. Be sure to include all the headers of the email when filing a complaint.

---
# Base64 Decoding
![[Pasted image 20230529210852.png]]
-   **Content-Type** is **application/pdf**. 
-   **Content-Disposition** specifies it's an **attachment**. 
-   **Content-Transfer-Encoding** tells us it's **base64** encoded.

Decode the Below portion with Base64 to reveal additional text!

---
# Defange a URL
Defanging is a way of **making the URL/domain or email address unclickable to avoid accidental clicks**, which may result in a serious security breach. It replaces special characters, like "@" in the email or "." in the URL, with different characters.

[URL Defang Tool | Trustifi](https://trustifi.com/url-defang-tool/)

---
