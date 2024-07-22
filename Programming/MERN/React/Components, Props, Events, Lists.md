# React Components
Components are like *functions* that return HTML elements. They serve the same purpose as JavaScript functions, but work in isolation and return HTML.
Components come in two types, Class components and Function components.

%% Note : In older React code bases, you may find Class components primarily used. It is now suggested to use Function components along with Hooks, which were added in React 16.8 %%

### Function Component
```jsx
function Car() {
  return <h2>Hi, I am a Car!</h2>;
}
```


## Rendering a Component
Now your React application has a component called Car, which returns an `<h2>` element.
To use this component in your application, use similar syntax as normal HTML : `<Car />`

```jsx
const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(<Car />);
```


## Components in Components
We can refer to components inside other components
```jsx
function Car() {
  return <h2>I am a Car!</h2>;
}

function Garage() {
  return (
    <>
      <h1>Who lives in my Garage?</h1>
      <Car />
    </>
  );
}

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(<Garage />);
```

## Components in Files
React is all about re-using code, and it is *recommended to split your components into separate files*.
To do that, create a new file with a `.js` file extension and put the code inside it.

```jsx
export default function Car() {
  return <h2>Hi, I am a Car!</h2>;
}
```

To be able to use the Car component, you have to *import* the file in your application.
```jsx
import React from 'react';
import ReactDOM from 'react-dom/client';
import Car from './Car.js';

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(<Car />);
```

---
# Props
Props are arguments passed into React components. `props` stands for properties.
```jsx
function Car(props) {
  return <h2>I am a { props.brand }!</h2>;
}

const myElement = <Car brand="Ford" />;
```

Props are also how you pass data from one component to another, as parameters.
```jsx
function Car(props) {
  return <h2>I am a { props.brand }!</h2>;
}

function Garage() {
  return (
    <>
      <h1>Who lives in my garage?</h1>
      <Car brand="Ford" />
    </>
  );
}

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(<Garage />);
```

Or if it was an **object**
```jsx
function Car(props) {
  return <h2>I am a { props.brand.model }!</h2>;
}

function Garage() {
  const carInfo = { name: "Ford", model: "Mustang" };
  return (
    <>
      <h1>Who lives in my garage?</h1>
      <Car brand={ carInfo } />
    </>
  );
}

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(<Garage />);
```

%% Note React Props are read-only! You will get an error if you try to change their value. %%

---
# React Events
React events are written in *camelCase* syntax : `onClick` instead of `onclick`
React event handlers are written inside curly braces : `onClick={shoot}`  instead of `onClick="shoot()"`.

```jsx
function Football() {
  const shoot = () => {
    alert("Great Shot!");
  }

  return (
    <button onClick={shoot}>Take the shot!</button>
  );
}

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(<Football />);
```

##### Passing Arguements
```jsx
function Football() {
  const shoot = (a) => {
    alert(a);
  }

  return (
    <button onClick={() => shoot("Goal!")}>Take the shot!</button>
  );
}

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(<Football />);
```


##### React Event Object
Event handlers have access to the React event that triggered the function.
```jsx
function Football() {
  const shoot = (a, b) => {
    alert(b.type);
    /*
    'b' represents the React event that triggered the function,
    in this case the 'click' event
    */
  }

  return (
    <button onClick={(event) => shoot("Goal!", event)}>Take the shot!</button>
    // When you press "Take the shot!" button, it'll alert 'click'
  );
}

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(<Football />);
```

---
# Lists
The JavaScript `map()` array method is generally the preferred method.
```jsx
function Car(props) {
  return <li>I am a { props.brand }</li>;
}

function Garage() {
  const cars = ['Ford', 'BMW', 'Audi'];
  return (
    <>
      <h1>Who lives in my garage?</h1>
      <ul>
        {cars.map((car) => <Car brand={car} />)}
      </ul>
    </>
  );
}

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(<Garage />);


// Output
Who lives in my garage?

-   I am a Ford
-   I am a BMW
-   I am a Audi
```


## Keys
Keys allow React to keep track of elements. This way, if an item is updated or removed, only that item will be re-rendered instead of the entire list.
Keys need to be *unique* to each sibling. But they can be duplicated globally.

```jsx
function GroceryList() {
  const items = [
    {id: 1, name: 'bread'},
    {id: 2, name: 'milk'},
    {id: 3, name: 'eggs'}
  ];

  return (
    <>
      <h1>Grocery List</h1>
      <ul>
        {items.map((item) => <li key={item.id}>{item.name}</li>)}
      </ul>
    </>
  );
}

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(<GroceryList />);
```