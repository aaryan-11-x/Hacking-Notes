# Compiler
A JIT compiler converts program source code into native machine code just before program execution.
The Dart VM has a `just-in-time compiler (JIT)` that supports both **pure interpretation** (as required on iOS devices, for example) and **runtime optimization**.

The `AOT compiler` works by compiling your code before it is “delivered” to whatever runtime environment runs the code. The AOT compiler is typically *used* when the **app is ready to be deployed to either an Appstore** or an in-house production backend.
The AOT-compiled code runs inside an efficient Dart runtime that enforces the sound Dart type system and *manages memory* using *fast object allocation* and a *generational garbage collector*.

---
# Terminologies
###### Higher-Order Functions
Higher-order functions are functions that can take *one or more functions as arguments*, and/or return a function as a result.
```dart
// Higher-order function that takes a function as an argument
int operate(int a, int b, Function(int, int) operation) {
  return operation(a, b);
}

// Function to add two numbers
int add(int a, int b) {
  return a + b;
}

// Function to multiply two numbers
int multiply(int a, int b) {
  return a * b;
}

void main() {
  int result1 = operate(5, 3, add); // Pass the 'add' function as an argument
  int result2 = operate(5, 3, multiply); // Pass the 'multiply' function as an argument
  
  print('Result of addition: $result1'); // Output: Result of addition: 8
  print('Result of multiplication: $result2'); // Output: Result of multiplication: 15
}
```


###### Lexical Closure
A lexical closure is a function that *captures the variables from its surrounding environment*, even after the outer function has finished executing. This *allows the inner function to access and manipulate the captured variables*.
```dart
Function counter() {
  int count = 0; // Variable captured by the closure
  
  return () {
    count++; // Accesses and manipulates the captured 'count' variable
    print(count);
  };
}

void main() {
  var increment = counter();
  
  increment(); // Output: 1
  increment(); // Output: 2
  increment(); // Output: 3
}
```


###### Type Promotion
Type promotion allows you to assign a value to a nullable variable.
```dart
void main(){
String? name;
name = 'Dhruv';
print(name.length);
}
```
Therefore, Dart implicitly promotes the type of the message variable from **String? to String** automatically.

###### Flow Analysis
Dart uses a sophisticated flow analysis to check every possible case the code would take. And if **none** of these cases come up with the **possibility of being null**, it promotes the variable to a non-nullable type using Type Promotion.
