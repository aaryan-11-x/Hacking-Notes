![[Pasted image 20230301115544.png]]


#### Predicting Session Tokens

![[Pasted image 20230301115659.png]]
![[Pasted image 20230301115722.png]]


| Session Token     | Back-End Language |
| ----------------- | ----------------- |
| **JSESSIONID**        | Java              |
| **ASPSESSIONID**      | IIS Server        |
| **ASP.NET_SessionId** | ASP.NET           |
| **PHPSESSID**         | PHP               | 

---

###### Example

To Check For <mark style="background: #CACFD9A6;">Cookies</mark> :-
**Developer Tools** --> **Applications** --> **Storage** --> **Cookies**

| Username | Cookie       |
| -------- | ------------ |
| webgoat  | 65432ubphcfx |
| aspect   | 65432udfqtb  |
| alice    | 65432fdjmb   |

Here, The **Algorithm** To Assign A <mark style="background: #CACFD9A6;">Cookie</mark> Is :-1
- 65432 Is Constant
- *Reverse* The Username, For Ex: webgoat ---> taogbew
- *Shift* Each Letter *One Time* To The *Next Letter*. For Ex: taogbew ---> ubphcfx


Thus, We Get The **Login** of Alice By ***Changing The Cookie!***
![[Pasted image 20230325200254.png]]