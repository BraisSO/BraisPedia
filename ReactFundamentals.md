<span style="font-family: Times New Roman;">
<span style="text-align: justify">
<span style="font-size: medium;">

# Conceptos fundamentales de React

**Components**: React applications are built using reusable components. A component is a JavaScript class or function that returns a JSX element, which describes the component's layout and content. Here's an example of a simple component:
Copy code

```javascript
import React from 'react';

function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

export default Welcome;

```
**JSX**: JSX is a syntax extension for JavaScript that allows you to write HTML-like elements in your JavaScript code. It's used to describe the layout and content of a component. Here's an example of JSX:

```javascript
<div>
  <h1>Welcome to React</h1>
  <p>This is a simple example</p>
</div>
```

**Props**: Props (short for "properties") are used to pass data from a parent component to a child component. They're passed as attributes to the child component's JSX element. Here's an example of passing props:
```javascript
import Welcome from './Welcome';

function App() {
  return <Welcome name="John" />;
}

```
**State**: State is used to manage the local data of a component. Unlike props, state can be modified by the component itself. Here's an example of using state to keep track of a counter:

```javascript
import React, { useState } from 'react';

function Counter() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <h1>Count: {count}</h1>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}
```
**Event Handling**: React provides a way to handle events such as button clicks, form submissions, and more. An event handler is a function that is called when an event occurs. Here's an example of handling a button click:

```javascript
import React, { useState } from 'react';

function Example() {
  const [count, setCount] = useState(0);

  return (
    <button onClick={() => setCount(count + 1)}>
      You clicked {count} times
    </button>
  );
}
```
**Life cycle methods:** React provides a set of lifecycle methods that you can use to control the behavior of your components at different points in their life cycle. Here's an example of using the componentDidMount lifecycle method to fetch data:
```javascript
import React, { useState, useEffect } from 'react';

function Example() {
  const [data, setData] = useState([]);

  useEffect(() => {
    fetch('https://jsonplaceholder.typicode.com/todos')
      .then(response => response.json())
      .then(data => setData(data));
  }, []);

  return (
    <ul>
      {data.map(item => (
        <li key={item.id}>{item.title}</
```

Sure, here are a few more concepts and features of React that you should know:

**Conditional rendering:** React allows you to conditionally render elements based on certain conditions. You can use JavaScript's if statements or the ternary operator to achieve this. Here's an example of conditional rendering:

```javascript
import React, { useState } from 'react';

function Example() {
  const [isLoggedIn, setIsLoggedIn] = useState(false);

  return (
    <div>
      {isLoggedIn ? (
        <p>Welcome back!</p>
      ) : (
        <p>Please log in</p>
      )}
    </div>
  );
}
```

**Forms**: React allows you to create and manage forms in your application. You can use the form and input elements to create forms and handle their values and events. Here's an example of a simple form with a text input:

```javascript
import React, { useState } from 'react';

function Example() {
  const [value, setValue] = useState('');

  return (
    <form>
      <input
        type="text"
        value={value}
        onChange={event => setValue(event.target.value)}
      />
    </form>
  );
}
```

**Routing**: Routing allows you to define different URLs for different pages in your application. You can use a library like react-router to handle routing in your React application. Here's an example of using react-router to handle routing:

```javascript
import { BrowserRouter as Router, Route } from 'react-router-dom';

function App() {
  return (
    <Router>
      <Route path="/" exact component={Home} />
      <Route path="/about" component={About} />
      <Route path="/contact" component={Contact} />
    </Router>
  );
}
```

These are just a few examples of what you can do with React. There are many more concepts and features to learn, but mastering these will give you a solid foundation to build on.