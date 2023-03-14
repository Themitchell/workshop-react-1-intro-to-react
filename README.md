<!---
marp: true
theme: uncover
class: invert
headingDivider: 2
paginate: true
header: '&e tech'
footer: 'Created with [Marp](https://marp.app) and [Github Pages](https://pages.github.com)'
backgroundImage: url('img/react-logo.svg')
backgroundPosition: 120% 120%
backgroundSize: 40%
style: |
  section,
  section code {
    font-size: 30px;
    text-align: left;
  }

  section ul,
  section ol,
  section img {
    margin-left: 0;
  }

  section.long p,
  section.long ul,
  section.long ol,
  section.long code, {
    font-size: 24px;
  }

  section .columns img {
    width: 100%;
  }

  section header {
    height: 100px;
    width: 100px;
    left: auto;
    right: 40px;
    background-color: #dfddd7;
    background-size: contain;
    -webkit-mask-image: url(img/and-e-tech-logo-300.svg);
    mask-image: url(img/and-e-tech-logo-300.svg);
    -webkit-mask-repeat: no-repeat;
    mask-repeat: no-repeat;
    -webkit-mask-size: contain;
    mask-size: contain;
    text-indent: -99999999px
  }

  section .columns {
    display: grid;
    grid-template-columns: repeat(2, minmax(0, 1fr));
    gap: 1rem;
  }

  section marp-pre code {

  }
--->

# React Workshop

Intro to React

# What is react?

- At it’s most basic, React is JS library for creating UIs by Facebook
- React is a JavaScript view layer
- Unlike MVC frameworks it doesn’t have
  - controllers
  - models

<!---
There are other frameworks like angular, ember etc or even Rails which have similarities / differences
--->

# What do we mean by view layer?

- React renders JSX as HTML
- React interpolates data into HTML using JSX
- Event handlers can be bound to UI events
- React redraws the DOM in response to events
- Where we get our data and what those UI events trigger is up to us
- All of these things are the UI or view layer

# A basic React app

<!--- _class: invert long --->

- HTML page with a root react element
- Include react source
- Your custom react code

```html
<div id="root"></div>

<script src="path/to/react.js" />
<script src="path/to/react-dom.js" />

<script>
  const rootElement = document.getElementById('root');
  const root = ReactDOM.createRoot(rootElement);
  root.render("Hello, world!");
</script>
```

<!---
We do not have a pipeline, we don't use any transpilers... yet!
Just plain and simple react
This is not how we would build a large app... why? Because we like lots of files and organisation
Build tools are out of scope
--->

# What is JSX?

- JSX is a templating tool for Javascript
- It allows us to write something similar to HTML (but not quite the same!)
- It allows us to interpolate JavaScript using `{}`
- It allows us to bind data or functions to JSX attributes
- It allows us to bind UI events

```jsx
const helloWorldText = "Hello World!";
root.render(<h1>{helloWorldText}</h1>);
```

<!---
React uses camel case for props so that it can bind HTML properties if we need to
for example class="" cannot accept a dynamic value. className={} can.
JSX is not the only templating tool for react - we could use handlebars for example
--->

# What is a component?

<!--- _class: invert long --->

<div class="columns">

- Components are reusable independent bits of code
- Components can be as simple as a function but we can also use classes
- Components can accept props as an argument
- Components can hold their own state
- Components return JSX to be rendered as HTML
- To interpret JSX we need babel

```jsx
function HelloWorldComponent () {
  return <h1>Hello world!</h1>;
}

class HelloWorldComponent extends React.Component {
  render() {
    return <h1>Hello world!</h1>;
  }
}

root.render(<HelloWorldComponent />);
```

</div>

<!---
The above examples are functionally the same
In new versions  of React we shouldn's use classes, there is no need. React considers classes to be outdated and with the introduction of hooks, functional components can use state. React docs here https://beta.reactjs.org/reference/react/Component
--->

# What are props?

<div class="columns">

- Props are values we pass to components
- Props can be data or functions
- Props are immutable
- When new props are passed to a react component, the component will re render


```jsx
function HelloWorldComponent (props) {
  return <h1>Hello {props.username}!</h1>;
}

class HelloWorldComponent extends React.Component {
  render(props) {
    return <h1>Hello {props.username}!</h1>;
  }
}

<HelloWorldComponent username="Andy" />
```

</div>

# propTypes

<div class="columns">

- PropTypes checks the type of props
- We can validate the shape of our props
- We can validate the presence of props

```jsx
import PropTypes from 'prop-types'

function HelloWorldComponent (props) {
  return <h1>Hello {props.username}!</h1>;
}

HelloWorldComponent.propTypes = {
  username: PropTypes.string.isRequired
}
```

</div>

<!---
We can use the browser console to view Type erros easily and it will also be highlighted in tests
We can also use Typescript which removes the need for PropTypes. Type handling is managed at compilation time not run time.
--->

# defaultProps

<div class="columns">

- defaultProps allows us to set defaults
- defaults must adhere to the type checking

```jsx
import PropTypes from 'prop-types'

function HelloWorldComponent (props) {
  return <h1>{props.salutation} {props.username}!</h1>;
}

HelloWorldComponent.defaultProps = {
  salutation: 'Hello'
}
```

</div>

# Lots of small components

<div class="columns">

- We can call components inside other components
- We can pass data down to other components as props
- We can forward all of the parent component’s props to child component using `...props`

```jsx
function SayHello (props) {
  return <p>Hello {props.username}!</p>;
}

function HelloWorldComponent (props) {
  return (
    <React.Fragment>
      <SayHello username={props.username} />
      <SayHello username="World" />
      <SayHello {...props} />
    </React.Fragment>
  )
}
```
</div>

<!---
We can also use <div> instead of <React.Fragment> if we want to wrap our elements in a container. React.Fragment will not render any unneccessary wrapping element in the DOM if we don't need it. This can help with certain types of styling which targets :children for example.
The wrapping element or component is necessary as each React component render must have one root element or component
--->

# Component children

<div class="columns">

- Sometimes we want to wrap components in another component
- We can use the special `props.children`  to create a wrapper component
- This is called composition


```jsx
function PopUpWindow (props) {
  return (
    <div style={{ border: '1px solid grey' }}>
      {props.children}
    </div>
  );
}

function SayHello(props) {
  return <h1>Hello {props.username}!</h1>;
}

function HelloWorldComponent () {
  return (
    <PopUpWindow>
      <SayHello username="Andy" />
    </PopUpWindow>
  );
}
```

</div>

# What are events?

<div class="columns">

- DOM events are triggered by user interactions (button clicks, input changes etc)
- DOM events can be bound to functions (event handlers)
- Event handlers can modify state, or trigger ui changes
- Event handlers can be passed as props

```jsx
function HelloWorldComponent () {
  function onClickHandler (event) {
    alert('Hello world');
  }

  return (
    <button onClick={onClickHandler}>
      Say hello
    </button>
  );
}
```

</div>

<!---
All event handlers take an event as an argument, we used it to wrap the functionality we want to perform. We can also define these inline.
Note the missing `(event)` from the onClick assignment
--->

# What is state?

<!--- _class: long invert --->


<div class="columns">

- State is data internal to a component
- State is immutable
- Modifying state re renders the component
- State is accessed and modified in functional components using the `useState` hook
- State is accessed in a class components using `this.state`
- State is modified in a class components using `this.setState`

```jsx
import React, {useState} from "react";

function HelloWorldComponent () {
  const [username, setUsername] = useState("Andy");

  function setUsernameHandler (event) {
    setUsername(event.target.value);
  }

  return (
    <div>
      <p>Hello {username}!</p>
      <input onChange={setUsernameHandler} value={username} />
    </div>
  );
}
```

</div>

<!---
We haven’t talked about hooks yet - this is slightly more advanced but we can touch on it here
--->

# Why not mutate state?

<div class="columns">

- It’s simpler to rebuild new state than mutate old state
- It’s easier to detect that the entire state has changed than to detect if some part of the state has changed
- If it’s easy to detect if state has changed, it’s easy to determine when components re render
- Modifying state directly will not re render the component

```jsx
function HelloWorldComponent () {
  let [username, setUsername] = useState("Andy");

  function setUsernameHandler (event) {
    username = event.target.value;
  }

  ...
}
```

</div>

<!---
You cannot modify state this way in a functional component you can only use hooks which avoid this problem, note in the console when we try to modify state directly
--->

# Data down, actions up!

<!--- _class: invert long --->

<div class="columns">

- Data down
  - Passed to component via props
  - Props changes re render the component
- Actions up
  - Callbacks functions passed to component via props
  - Component calls a callback using events
  - Callback modifies a value in parent component
  - Modified value passed back to the child as a prop

![data down actions up](img/data-down-actions-up.jpg)

</div>

<!---
Look at the difference between the original HelloWorldComponent and the newer HelloWorldComponent which uses the InputComponent. Note how the state is being "lifted up" a level from the input.
--->


# Lists and keys

<div class="columns">

- Rendering lists is simple
- Each item in a list requires a `key`
- Keys help react identify items which are added, changed, or removed
- Keys should be unique to a set of sibling elements
- Keys should be stable
- Keys must on the iterated component, not inside a child

```jsx
function ListItem (props) {
  // Do not add the key here
  return <li>{props.children}</li>;
};

function UsersList (props) {
  const listItems = props.users.map(function (user) {
    // Keys must go here
    return (
      <ListItem key={user.id}>
        {user.username}
      </ListItem>
    );
  });

  return <ul>{listItems}</ul>;
}
```

</div>

<!---
What do we mean by stable? indexes are bad, they can change if items are reordered. Ids are good, they are fixed and unique. You can use whatever you like as long as you can ensure it is stable and unchanging but generally indexes are not the best way.

It is possible to use indexes in a fix but not for items that may reorder. See https://robinpokorny.com/blog/index-as-a-key-is-an-anti-pattern/
--->
