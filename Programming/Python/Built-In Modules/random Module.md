A random number generator takes a value called a *seed*, treats it as an input value, calculates a "random" number based on it (the method depends on a chosen algorithm) and produces a new seed value.

1) `random`
```python
# Random method produces a floating point number from the range (0.0, 1.0)
from random import random

for i in range(5):
    print(random())
```
![[Pasted image 20240318000823.png]]


2) `seed`
**seed()** - sets the seed with the current time
**seed(int_value)** - sets the seed with the integer value int_value.
```python
# Set the generators seed
from random import random, seed
seed(0)

for i in range(5):
    print(random())
```

Output will be the same due to the same seed!


3) `randrange`
Get random integer values
```python
randrange(end)
randrange(beg, end)
randrange(beg, end, step)

# Gives an integer b/w beg and end + 1
randint(beg, end+1)
```


4) `choice` & `sample`
```python
from random import choice, sample

my_list = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

print(choice(my_list))        # Chooses any number from the list
print(sample(my_list, 5))     # Selects 5 numbers randomly from the list
print(sample(my_list, 10))    # Selects 10 numbers randomly from the list
```
