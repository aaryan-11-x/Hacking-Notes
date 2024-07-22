## What is React?
React, sometimes referred to as a *frontend JavaScript framework*, is a JavaScript library created by **Facebook**.
React is a tool for building UI components.

## How does React Work?
React creates a `VIRTUAL DOM` in memory.
Instead of manipulating the browser's DOM directly, React creates a virtual DOM in memory, where it does all the necessary manipulating, before making the changes in the browser DOM.

In order to use React in production, you need **npm** and **Node.js** installed.


## What is ES6?
ES6 stands for **ECMAScript 6**.
ECMAScript was created to standardize JavaScript, and ES6 is the 6th version of ECMAScript, it was published in 2015, and is also known as ECMAScript 2015.
**React** uses ES6!

In **regular functions** the `this` keyword represented the object that called the function, which could be the window, the document, a button or whatever.
With **arrow functions**, the `this` keyword _always_ represents the object that defined the arrow function.

Checkout more on [[ES6]]

---
# React Render HTML
React's goal is in many ways to *render* HTML in a web page.
React renders HTML to the web page by using a function called `createRoot()` and its method `render()`.

### The createRoot Function
The `createRoot()` function takes one argument, an **HTML element**.
The purpose of the function is to define the HTML element *where a React component should be displayed*.

## The render Method
The `render()` method is then called to *define the React component that should be rendered*.

***But render where?***
There is an `index.html` file.
You'll notice a single `<div>` in the body of this file. This is where our React application will be rendered.

Display a paragraph inside an element with the id of "root":
```jsx
// main.jsx file
const container = document.getElementById('root');
const root = ReactDOM.createRoot(container);
root.render(<p>Hello</p>);
```

The result is displayed in the `<div id="root">` element:
```html
<!-- index.html file -->
<body>
  <div id="root"></div>
</body>
```
%%   Note that the element id does not have to be called "root", but this is the standard convention   %%

```jsx
import React from 'react';
import ReactDOM from 'react-dom/client';

const myelement = (
  <table>
    <tr>
      <th>Name</th>
    </tr>
    <tr>
      <td>John</td>
    </tr>
  </table>
);

const container = document.getElementById('root');
const root = ReactDOM.createRoot(container);
root.render(myelement);
```

---
# React JSX

### What is JSX?
JSX stands for **JavaScript XML**.
JSX allows us to write HTML in React.
JSX makes it easier to write and add HTML in React.

### Coding JSX
JSX allows us to write HTML elements in JavaScript and place them in the DOM without any `createElement()` and/or `appendChild()` methods.
JSX *converts* HTML tags into react elements.

`With` JSX:
```jsx
const myElement = <h1>I Love JSX!</h1>;

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(myElement);
```

`Without` JSX:
```jsx
const myElement = React.createElement('h1', {}, 'I do not use JSX!');

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(myElement);
```

## Expressions in JSX
With JSX you can write **expressions** inside curly braces `{ }`.
The expression can be a React variable, or property, or any other valid JavaScript expression. JSX will execute the expression and return the result:
```jsx
// Execute the expression `5 + 5`
const myElement = <h1>React is {5 + 5} times better with JSX</h1>;
```

To write HTML on multiple lines, put the HTML inside parentheses`()`
JSX will throw an error if the HTML is not correct, or if the HTML misses a parent element.


## One Top Level Element
The HTML code must be wrapped in _ONE_ top level element.
So if you like to write _two_ paragraphs, you must put them inside a parent element, like a `div` element.

Wrap two paragraphs inside one DIV element
```jsx
const myElement = (
  <div>
    <p>I am a paragraph.</p>
    <p>I am a paragraph too.</p>
  </div>
);
```

Alternatively, you can use a "fragment" to wrap multiple lines. This will prevent unnecessarily adding extra nodes to the DOM.
A fragment looks like an empty HTML tag: `<></>`.
```jsx
// Wrap two paragraphs inside a fragment
const myElement = (
  <>
    <p>I am a paragraph.</p>
    <p>I am a paragraph too.</p>
  </>
);
```


JSX follows [[XML]] rules, and therefore HTML elements must be ***properly closed***.
Close empty elements with `/>`
```jsx
const myElement = <input type="text" />;
```


#### Attribute class = className
The `class` attribute is a much used attribute in HTML, but since JSX is rendered as JavaScript, and the `class` keyword is a reserved word in JavaScript, you are not allowed to use it in JSX.

Use attribute `className` instead.
```jsx
const myElement = <h1 className="myclass">Hello World</h1>;
```

---
# Conditions
## If Statements
React supports `if` statements, but not _inside_ JSX.
To be able to use conditional statements in JSX, you should
- Put the `if` statements *outside* of the JSX
- You could use a `ternary expression`.

```jsx
// If statement
const x = 5;
let text = "Goodbye";
if (x < 10) {
  text = "Hello";
}
const myElement = <h1>{text}</h1>;

// Ternary Expression
const x = 5;
const myElement = <h1>{(x) < 10 ? "Hello" : "Goodbye"}</h1>;
```

**Note** that in order to embed a JavaScript expression inside JSX, the JavaScript must be wrapped with curly braces, `{}`.

```jsx
function MissedGoal() {
  return <h1>MISSED!</h1>;
}

function MadeGoal() {
  return <h1>Goal!</h1>;
}


function Goal(props) {
  const isGoal = props.isGoal;
  if (isGoal) {
  return <MadeGoal/>;
  }
  return <MissedGoal/>;
}

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(<Goal isGoal={false} />);
```


## Logical `&&` Operator
```jsx
// If `cars.length > 0` is equates to true, the expression after `&&` will render.
function Garage(props) {
  const cars = props.cars;
  return (
    <>
      <h1>Garage</h1>
      {cars.length > 0 &&
        <h2>
          You have {cars.length} cars in your garage.
        </h2>
      }
    </>
  );
}

const cars = ['Ford', 'BMW', 'Audi'];
const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(<Garage cars={cars} />);
```

---
