# Redux fundamentals from egghead by Dan Abramov

##Princples of Redux
  1. Represent state as a single JS object called the state tree.
  2. The state tree is immutable and read only. Any change to the tree has to be dispacted as actions
  3. State change is a pure function of previous state and an action. This function is called the Reducer.

  The minimal representation of the data in our application.
## First principle of Redux
  . Everything is represented as a single JS object called the state tree.
  . It is the mininal state required by our application.

## Second principle of redux
 . The state tree is read only.
 . You need to dispatch an action anytime you need a change.

### Action
  A minimal representation of the change to the data.

 . A JS object describing the change.
 . Minimal data needed to make the required change to the state tree.
 . The only requirement is that is has a type property
 . The type property's value cannot be undefined.
 . It is recommended to use String as it is Serializable.

### React and Actions
 . The component's do not need to know how the change is implemented. They just need to dispatch the action with the type describing the change and other minimal data required to get the new state tree.
 . Whether action is initiated by user interaction or a network request, any data that enters the redux state tree/applicatioon is through actions.

### Functional programming in Redux
The UI or the view layer is most predictable when described as pure functions. A component is the function of props and state. The same idea is used in redux. The state mutations in our app has to be described as a pure function. A function of previous state and action. This function is called the Reducer.

### The Reducer.
State mutations in Redux are described as a function of your previous state and the action describing the change and returns the next state of the whole application.
. It cannot modify existing state (Principle 2)
. It is not slow as references to old state is used by redux.

  StateChange = function (previous state , action )
  Component = function (props and state)

```js
function counter(state,action){
  return state
}

```
#### Writing reducer
  1. Write condition for unknown action types
  2. If the prevState is undefined, what is returned is considered as the Initial state for the application.

```js
function Reducer(state= 0 , action){
  switch(action.type){
    case 'INCREMENT':
      return state + 1
    case 'DECREMENT':
      return state - 1
    case default:
      return state
  }
}
```

# Container component
```js
//Create Context
import { createContext } from "react";
const Store = createContext();


ReactDOM.render(
  <Store.Provider value={createStore(Reducer)}>
    <App />
  </Store.Provider>,
  document.getElementById("root")
);

// A component that needs state and dispatch

class TodoListContainer extends Component {
  componentDidMount() {
    const store = this.context;     // gets store passed down as props using context api
    this.unsubscribe = store.subscribe = () => { // subscribe to forcefully update this component with updated state
      this.forceUpdate();
    };
  }

  componentWillUnmount() {
    this.unsubscribe(); // unsubscribe to updates to this component when unmounted
  }

  render() {
    const store = this.context; // gets store
    const { todos, visibilityFilter } = store.getState(); // gets application state from store
    return (
      <TodoList
        todos={getVisibleTodos(todos, visibilityFilter)}
        onTodoClick={(id) => store.dispatch({ type: "TOGGLE_TODO", id })}  // calls dispatch on store
      />
    );
  }
}

// Needed to get this.context
TodoListContainer.contextType = Store;

//functional component
const NavLinksContainer = () => {
  const store = useContext(Store);
  const { visibilityFilter } = store.getState();
  const filters = [
    { value: "SHOW_ALL", text: "All" },
    { value: "SHOW_COMPLETED", text: "Completed" },
    { value: "SHOW_ACTIVE", text: "Active" }
  ];

  return (
    <NavLinks
      filters={filters}
      currentFilter={visibilityFilter}
      onClick={(filter) => {
        store.dispatch({
          type: "SET_VISIBILITY_FILTER",
          filter
        });
      }}
    />
  );
};


```

# Full Todo Example without react redux library

```js
import { createStore } from "redux";
import Reducer from "./reducers";
import ReactDOM from "react-dom";
import "./styles.css";
import { Component } from "react";
import { createContext, useContext } from "react";
const Store = createContext();

const Todo = ({ completed, onClick, text }) => (
  <li
    style={{ textDecoration: completed ? "line-through" : "none" }}
    onClick={onClick}
  >
    {text}
  </li>
);

const getVisibleTodos = (todos, filter) => {
  switch (filter) {
    case "SHOW_ACTIVE":
      return todos.filter((todo) => !todo.completed);
    case "SHOW_COMPLETED":
      return todos.filter((todo) => todo.completed);
    default:
      return todos;
  }
};

const TodoList = ({ todos, onTodoClick }) => {
  return todos.map((todo) => (
    <Todo key={todo.id} onClick={() => onTodoClick(todo.id)} {...todo} />
  ));
};

const Link = ({ active, onClick, children }) => {
  if (active) {
    return <span>{children}</span>;
  }
  return (
    <button
      href="#"
      onClick={(e) => {
        e.preventDefault();
        onClick();
      }}
    >
      {children}
    </button>
  );
};

class FilterLink extends Component {
  componentDidMount() {
    const store = this.context;
    this.unsubscribe = store.subscribe(() => {
      this.forceUpdate();
    });
  }

  componentWillUnmount() {
    this.unsubscribe();
  }

  onClick = () => {
    this.props.onClick(this.props.filter);
  };

  render() {
    const state = this.context.getState();
    const active = state.visibilityFilter === this.props.filter;
    return (
      <Link active={active} onClick={this.onClick}>
        {this.props.children}
      </Link>
    );
  }
}
FilterLink.contextType = Store;

const AddTodo = () => {
  let input;
  const store = useContext(Store);
  return (
    <>
      <input ref={(node) => (input = node)} />
      <button
        onClick={() => {
          store.dispatch({
            type: "ADD_TODO",
            id: store.getState().counter,
            text: input.value
          });
          input.value = "";
        }}
      >
        Add Todo
      </button>
    </>
  );
};

const NavLinks = ({ filters, currentFilter, onClick, store }) => {
  return filters.map((item) => (
    <FilterLink
      filter={item.value}
      key={item.value}
      currentFilter={currentFilter}
      onClick={onClick}
    >
      {item.text}
    </FilterLink>
  ));
};

const NavLinksContainer = () => {
  const store = useContext(Store);
  const { visibilityFilter } = store.getState();
  const filters = [
    { value: "SHOW_ALL", text: "All" },
    { value: "SHOW_COMPLETED", text: "Completed" },
    { value: "SHOW_ACTIVE", text: "Active" }
  ];

  return (
    <NavLinks
      filters={filters}
      currentFilter={visibilityFilter}
      onClick={(filter) => {
        store.dispatch({
          type: "SET_VISIBILITY_FILTER",
          filter
        });
      }}
    />
  );
};

class TodoListContainer extends Component {
  componentDidMount() {
    const store = this.context;
    this.unsubscribe = store.subscribe = () => {
      this.forceUpdate();
    };
  }

  componentWillUnmount() {
    this.unsubscribe();
  }

  render() {
    const store = this.context;
    const { todos, visibilityFilter } = store.getState();
    return (
      <TodoList
        todos={getVisibleTodos(todos, visibilityFilter)}
        onTodoClick={(id) => store.dispatch({ type: "TOGGLE_TODO", id })}
      />
    );
  }
}

TodoListContainer.contextType = Store;

const App = () => {
  return (
    <div>
      <AddTodo />
      <NavLinksContainer />
      <TodoListContainer />
    </div>
  );
};

ReactDOM.render(
  <Store.Provider value={createStore(Reducer)}>
    <App />
  </Store.Provider>,
  document.getElementById("root")
);
```

#  What a container Component essentially does
A container component does the following
1. Rerender component with updated state using subscribe and this.forceUpate()
2. Use State in the component to pass down as props to presentational component
3. Use dispatch methods to pass down functions such as onClick etc to update app state down to presentational components as props as well.

# Connect API from React-Redux
Connect abstracts away what a container component does and gives you back a container component that calls a presentational components.

# Designing store with typescript
1. Design State
-Resource
* Data
* Error
* Loading
2. Design folder structure
  -Components
    App
  -Redux
    reducers
    actions_creators
    middle-wares
    __________
    index.page


