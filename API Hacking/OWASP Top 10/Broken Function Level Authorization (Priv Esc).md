Complex access control policies with different hierarchies, groups, and roles, and an unclear separation between administrative and regular functions, tend to lead to authorization flaws. By exploiting these issues, attackers can gain access to other users’ resources and/or administrative functions.

Often involves replacing passive methods (`GET`) with active (`PUT`, `DELETE`)

- Use **OPTIONS** HTTP method to check which *methods are allowed on this request*.
![[Pasted image 20230928075718.png]]

- We can use the **DELETE** method, great!
- But we still can't delete the video as the permission is only available for admin
- In REST APIs, it's easy to guess which attribute must be changed in-order to get the admin access
- Change the `/api/v2/user` to `/api/v2/admin`

```json
{
"role": "user"
}
// Change role to admin
{
"role": "admin"
}
```
