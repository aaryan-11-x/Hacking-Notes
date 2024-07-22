###### findOne()
The `findOne()` function is used to find one document according to the condition. If multiple documents match the condition, then it returns the first document satisfying the condition. 


```sql
-- In SQL
SELECT * FROM users WHERE username = 'admin' AND password = 'admin'
```
```js
// In MongoDB
User.find({"username":"admin", "password":"admin"})
```
