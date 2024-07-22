- Lists all the methods inside the module.
- You have to import the module first; if you have used alias, then use the alias name only

```python
import math

for name in dir(math):
    print(name, end="\t")
```