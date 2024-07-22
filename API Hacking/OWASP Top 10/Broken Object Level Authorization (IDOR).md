# Pre-Requisites
- Install **JWT Editor** in [[Website Hacking/Tools/Burp Suite/Basics]]

---
# Basic Info
APIs tend to expose *endpoints* that handle *object identifiers*, creating a wide attack surface Level Access Control issue. Object-Level authorization checks should be considered in every function that access a data source using input from the user.

---
# Practical

![[Pasted image 20230927191300.png]]

In [[Website Hacking/Tools/Burp Suite/Basics]] :-
This is another vulnerability called **Excessive Data Exposure**
![[Pasted image 20230927191629.png]]

Send a request to the <mark style="background: #FFB8EBA6;">report link</mark> with an Authorization token!
![[Pasted image 20230927192955.png]]