# Theory
Vulnerabilities that allow attackers to bypass access controls & gain unauthorized access to resource or functionalities. This can occur when access controls are not properly implemented or enforced, or when user input is not validated properly.

###### Difference b/w BAC & IDOR
- In IDOR, you can access other user's details but you *still need to authorize*!
- In BAC, you can access other user's details *without any authorization*!

---
# Practical
In this example, you need a **JWT** to access any contents in the API.
![[Pasted image 20230929180626.png]]

BUT, we can access the contents without using `Authorization` header!
![[Pasted image 20230929180822.png]]
