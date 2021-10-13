# React

A ui library.
React is a way to describe how our ui. It has it's own virtual DOM and makes only the most necessary minimum changes.

Here is some description of an ui

```jsx
const App = () => (
  <div>
    {" "}
    // Is this a dom element=> Yes => Create a dom element
    <Component /> // Is this a dom element => No=> Call and evaluate the
    component the component
  </div>
);
```

Inside the component execution context, React is going to go through the same system.

```jsx
const Component = () => (
  <div>
    {" "}
    // Is this a dom element=> Yes => Create a dom element
    <p>
      {" "}
      // Is this a dom element=> Yes => Create a dom element
      <AnotherCustomComponent /> // // Is this a dom element => No=> Call and
      evaluate the component the component
    </p>
  </div>
);
```

### React element

React.createElement('Title',{color:'red',children:["Hello, H1"]})

```js
{
  type: Title,
  props: {
    color:'red',
    children:'Hello, Title!'
  }
}
```

if type is string, it means a DOM element, while if type is a function, the element is a component.

### Nesting React elements

```js
{
  type: Title,
  props: {
    color:'red',
    children:{
      type:'h1',
      props:{
        children:'Hello, H1!'
      }
    }
  }
}
```

When it sees an element inside another element, it starts calling (component) or creating that component recursively untill it can get a tree of DOM notes that React can render on the screen. This process is called reconciliation.

### Writing React

### Component

With a given set of inputs, the outout (how the component looks on the page) will always be the same.

#### Creating a reusable and configuratble component

1. Identify the jsx that appears to be duplicated
2. What is the purpose of that block of jsx? Think of a descriptive name for what it does.

### jsx

Jsx is how we describe the ui.

#### Delimiter

JSX attributes values must be delimited by either braces or quotes. If type is important and we want to pass something other tahn string, use curly braces.

### Props

Props are immutable and owned by a component's parent. Props are like arguments to the function(component). Interviewing props with HTML is how we create dynamic data-driven React components.

### State

When props are immutable and owned by a component's parent, state is owned by the component (as evidenced by this.state) and is mutable (using this.setState()).

state or not ?
3.1) Does it come from props= > No
3.2) Does it change over time => Yes
3.3) Can it be computed based on other props or state => No

Then it is state.
Rules of state

1. State is a JS object that is relevant and required by the component.(Preferably convertable to JSON)
2. State must be initialised when a component is created.
3. State can only be updated using the function setState. (State is immutable, can only set new State, not mutate existing state)

When the state or props of a component update, the component will re-render itself.
Every react component is rendered as a function of it's props and state. This rendering is deterministic. This means, given a set of props and a set of state, a React component will always render the same way.
`Component => f(props and state).`

Updating state and immutability
We should treat this.state as immutable. Always set new state, do not update current state.

### Propagating Events

children can let parents know to set new state by propogating events.
State can be passed down as props to children. Because props are immutable, children need some way to communicate events to parents. We can pass functions as props too. Parents can pass a function to it's children to call when new state(parent's state) has to be set (which causes the children to get new props and re-render).

Children communicate with parents by calling functions that are handed to them via props. Children are not the owner of this state passed to them as props, instead it calls the function passed to it as props which updates state in the parent.
Event propogation is how children let parents know that they need new props.

Conclusions
Data flows from parent to children as props
Events flow from children to parents through functions.

component = f(props,state)
props belong to parent, immutable by component
state belong to component, mutatable by component
every update in props or state will cause the ocmponent to rerender
conclusion, for the same set of props and state, the react component will always render the same way, i,e
c = f(p,s)
state if passed to a child, becomes a prop for the child

set initial state
in react, state is described as an object

set initial state empty

the component will mount with this empty state

set default or the " state after initial empty state" with componentDidMount() lifecycle method using setState. by doing this,

After mounting with empty state, we populate state with the date from setState() in componentDidMount.

event flows from children to parent through functions, parents can pass functions to children

state and props are immutable, create new state only through setState
which causes component to rerender with new state and props
c= f(p,s) still holds

Chapter 2:
functions / components/objects should follow single responsibility principle
meaning they should only be responsible for a single
task.

Architecting the app

1. Break down the ui into components
2. Write static html inluding orchestrating components and leaf components

Leaf components are low level components with most markup,while orchestraing components are orchestating the app, like toggles 3. Determine what should be stateful

state or not ?
3.1) Does it come from props= > No
3.2) Does it change over time => Yes
3.3) Can it be computed based on other props or state => No
Then it is state.

Forms are state managers on their own, though they might take props as initial state values.

4. Determine in which component each piece of state should live:
   For each state:

- Identify all components that renders something based on that state
- Find a common owner component(a single component above all the component that need that state in the hierarchy)
- Either the common owner or another component higher in the heirarchy should own the state
- If you can't find a component where it makes sense to own a state or violate the single responsibility principle, create a common parent to manage the state.

5. Hard code initial states

6. Add inverse data flow:
   go from onClick to this component event and keep hoisting/propagating event up the components

using the id property to determine whether or not an object has been created is a more common practice
for semantics,separate the event handling and what it does into two separate functions in parent component , i.e follow single responsibility principle
=> a )function to handle event from children which might just call function b
=> b ) function to do stuff based on the event

Props vs State
Props are state's immutable accomplice. What existed as mutable state in parents might be passed as immutable props to children.

Props act as state's one-way data pipeline. State is managed in some select parent component and then that data flows down through children as props. (understanding required.)

#### Virtual DOM

ReactDOM.render expects a virtual dom representation of the UI.

We build virtual DOM with ReactNode objects

We create ReactNode objects with either of three

1. ReactElement object
2. ReactText object (a string or a number)
3. An array of ReactNodes

React Element
React.createElement accepts three arguments:

1. The DOM element type
2. The element props
3. The children of the element

The children of the ReactElement(the third argument) must be a ReactNode object(ReactElement,ReactText , or array of ReactNodes)

We write ReactElements using jsx. A pre-processor transpiles(conpiles) jsx to React.createElement calls

We can also mix jsx and javascript.

1. Attribute expressions
2. Child expressions
3. Boolean attributes
4. Comments

#### JSX Attribute expressions

we use the delimiter `{}` to put javascript expressions.
Some patterns

1. Using ternary operators

```jsx
const warningLevel = "debug";
const component = <Alert color={warningLevel === "debug" ? "grey" : "red"} />;
```

2. Conditional child expressions

```jsx
const renderAdminMenu = () => <MenuLink to="/users"> User accounts </MenuLink>;

const App = (props) => {
  const userLevel = this.props.userLevel;
  return (
    <ul>
      <li>Menu</li>
      {userLevel === "admin" && renderAdminMenu()}
    </ul>
  );
};
```

same as

```js
if (userLevel === "admin")
  return React.createElement("ul", {}, React.createELement("li", {}, "Menu"));
else
  return React.createElement(
    "ul",
    {},
    React.createELement("li", {}, "Menu"),
    renderAdminMenu() //<MenuLink to="/users"> User accounts </MenuLink>
  );
```

user ternary to conditionally choose component

```jsx
const Mnu = <ul>{loggedInUser ? <UserMenu /> : <LoginLink />}</ul>;
```

#### JSX Boolean expressions

We have to be explicit with jsx unlike boolean attributes in html

```html
<input name="Name" disabled />
```

```jsx
<input name="Name" disabled={true}>
```

#### JSX comments

```jsx
<div>
  {/*
    This is a comment in jsx
  */}
</div>
```

#### JSX Spread syntax

Sometimes when we have many props to pass to a component, it can be cumbersome to list each one individually. Thankfully, JSX has a shortcut syntax that makes this easier.
If we have a props object that has two keys

```js
const props = { msg: "Hello", recipient: "World" };
```

We could pass each prop individually like this:

```jsx
<Component msg={"Hello"} recipient={"World"}>
```

```jsx
<Component {...props}>

// is the same as this
<Component msg={"Hello"} recipient={"World}>
```

### Custom attributes

We can use data-whatever attributes with native DOM element and any attributes with custom components.

#### class Components

We use the constructor to initialise state and bind methods. We must call `super` to call parent classes constructor which binds this to the class instance. It is mandatory if we are using a constructor.

```jsx
import React from "react";

class App extends React.Component {
  constructor(props) {
    super(props); // always needed
    this.state = { name: "Samrood" };
    this.someMethod = this.someMethod.bind(this); // binds 'this' keyword to always refer the instance of this class
  }

  someMethod = function () {};

  render() {
    return <div>this.state.name </div>;
  }
}
```

But with property initialisers and arrow functions, we can drop the constructor all together.

```js
import React from 'react'
class App extends React.Component {
  state = {name:'Samrood'}

  this.someMethod = ()=>{}

  render(){
    return <div>this.state.name </div>
  }
}
```

// learn about static default props and prop types.

```js
import ReactDOM from "react-dom";
import React from "react";

class App extends React.Component {
  constructor(props) {
    super(props);
    this.state = { lat: null };
    window.navigator.geolocation.getCurrentPosition(
      (position) => {
        this.setState({ lat: position.coords.latitude });
      },
      (err) => console.log(err)
    );
  }

  render() {
    return <div>Latitude: {this.state.lat}</div>;
  }
}

ReactDOM.render(<App />, document.getElementById("root"));
```

1. JS file gets loaded by the browser
2. Instance of App component is about to be created
3. App component 'constructor' function gets called
4. State object is created and assigned to `this.state` property
5. We call geolocation service
6. React calls the component render method
7. App returns JSX, gets renderred to page as HTML

After a while 8. We get result of geolocation 9. We update our state object with a call to this.setState 10. React sees that we updated the state of a component 11. React calls our render update method a second time 12. Render method returns some (updated) JSX 13. React takes that JSX and updates content on the screen.

#### Lifecycle Methods

functions we can optionally define inside our class based components. They will automatically by React be called at certain points during a component's lifecycle.

[Article on when react renders](https://lucybain.com/blog/2017/react-js-when-to-rerender/)

1. Constructor => When a new instance of our component is created
2. Render method => Not optional => Called after the constructor and then after every update to the component => component becomes visible
3. componentDidMount => Immediately after a component shows up on browser (When it is mounted onto the real DOM.

Called only once (even if `props` or `state` change).Unless if you add a key property which forces the component to unmount every state/prop change ([read on key property causing unmounting and mounting](https://lucybain.com/blog/2017/react-js-when-to-rerender/)).

After mounting The component will sit around and wait for updates

4. componentDidUpdate will be called everytime the component's state is updated (when `props` or `state` change.) Render will be called before componentDidUpdate

5. componentdidUnmount => When the component is no longer shown

### One time data loading.

Use data fetching and data loading code in the componentDidMount as it is the convention and helpful to new engineers. (one time only)

ComponentDidUpdate is a choice for dataloading on every state change(state or props).

### componentDidUnmount()

When a component is removed from the DOM/screen. Use when we wants to do some cleanup after it. We usually use it a lot when we use react with non react libraries. Example: google maps inside react.

There are three more (no need to learn now, ignore them for now)

1. shouldComponentUpdate
2. getDerivedStateFromProps
3. getSnapshotBeforeUpdate

### defaultProps

For functional components

```js
function Spinner() {}
Spinner.defaultProps = {};
```

For class Components

```js
class App extends {
  static defaultProps = {}
}

```

# Events

### Controlled events vs Uncontrolled Events

forms are also state handlers. But we prefer to control state than letting form handle it.

Benefits of controlled event:
We can know at anytime what state is. What we want is deterministic elements. React side is driving all the data, not the DOM.

When parents pass in functions to children, this functions still have access to the parent's state/props. This is how children communicate data to parents. They get functions that they run.

# Ref in React

Ref is used to get a single dom element and passed to a native dom element.

1. Create a ref

```js
const eleRef = React.createRef();
```

2. Pass it to a native dom element

```js
<img ref={eleRef} />
```

3 Use it

```js
eleRef.current; // gives you the DOM element, img dom note in this example
// example
eleRef.current.clientHeight; // gives you the height the image dom node is taking up.
```

# HOOKS

## useState

in class, we did three things for each state.

1. initialise it, this.state = {}
2. update it only with setState => this.setState
3. use it => this.state

## UseEffect

```jsx
//run initial only after the first render
useEffect(runMeFunction,[])
// run initial and every re-render
useEffect(runMeFunction)
// run initial and (every rerender if data has changed since last time)
useEffect(runMeFunction,[dependency1,dependency2 etc])
```

### useEffect clean

useEffect only expects a function and it is called before the next useEffect

#### Multiple effect api search pattern

```jsx
useEffect(() => {
  const timerId = setTimeout(() => {
    setDeboundedTerm(term);
  }, delay);
  return () => {
    clearTimeout(timerId);
  };
}, [term]);

useEffect(() => {
  const search = async () => {
    const { data } = await Wikipedia.get("/api.php", {
      params: { srsearch: debouncedTerm },
    });
    setResults(data.query.search);
  };

  if (debouncedTerm) {
    search();
  } else {
    setResults([]);
  }
}, [debouncedTerm]);
```

## Setting up to listen to events outside the component

1. The component needs to detect a click event on any element besides the one it created.
2. The component has a hard time setting up an event handler on elements that it does not create.
3. Event bubbling is a thing.

### Solution

1. The component can set up a manual event listener on the body element
2. A click on any element will bubble up to the body

# Programmatic navigation by ourselves

By default, in react-router-dom, BrowserRouter creates and maintains a history object. It is prop drilled to the component's children heirarchy as well. But with redux, we might have to programmatically navigate our user ourselves. One way is to pass the history object to the action creators. But a better way might be to handle the history object ourselves and use a `Router` instead of a `BrowserRouter` from `react-route-dom`.

## Steps:

1. Create and export a history object

```js
import { createBrowserHistory } from "history";
export default createBrowserHistory();
```

2. import the file and use the push method on createBrowserHistory() object.

```js
import history from "./path to file exporting createBrowserHistory";
history.push("/path you want to navigate to");
```
