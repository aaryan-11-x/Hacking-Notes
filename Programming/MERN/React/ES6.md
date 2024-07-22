# Modules
JavaScript modules allow you to break up your code into separate files. This makes it easier to maintain the code-base.
ES Modules rely on the `import` and `export` statements.

## Export
You can export a function or variable from any file. There are two types of exports: `Named` and `Default`.

#### Named Exports
You can create named exports two ways. In-line individually, or all at once at the bottom.

In-line individually:
```jsx
export const name = "Jesse"
export const age = 40
```

All at once at the bottom:
```jsx
const name = "Jesse"
const age = 40

export { name, age }
```


#### Default Exports
You can only have **one** default export in a file.
```jsx
const message = () => {
  const name = "Jesse";
  const age = 40;
  return name + ' is ' + age + 'years old.';
};

export default message;
```



## Import
You can import modules into a file in two ways, based on if they are named exports or default exports.
*Named exports must be destructured using curly braces. Default exports do not.*

```jsx
// Named Import
import { name, age } from "./person.js";                  // Destructuring

// Default Import
import message from "./message.js";
```
