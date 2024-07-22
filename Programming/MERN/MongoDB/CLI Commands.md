![[Pasted image 20240705125637.png]]
##### Connect to MongoDB
```js
// Connection
mongo <HOST>
mongo <HOST>:<PORT>
mongo <HOST>:<PORT>/<DB>
mongo <database> -u <username> -p '<password>'

// Database query
show dbs
use [database-name]
show collections
db["collection-name"].find().pretty()
```

##### Commands
https://blog.e-zest.com/basic-commands-for-mongodb


---
# Error Fixes
1) Mongod installation error/service run error
https://nareshkr22.medium.com/setup-mongodb-in-kali-linux-3ab86731e3ec