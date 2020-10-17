## React Notes

**Free React Resources - https://github.com/markerikson/react-redux-links**

JSX

- JSX is converted into JSON objects, these objects are called React Elements. React use these json objects(elements) to construct DOM
- Babel converts JSX into `React.createElement()` function call, which returns json object/react element of given jsx
- We can put any valid javascript expression inside curly braces in JXS. `<h1>{name}</h1>`
- If we split jsx in multiple lines we should wrap it in parentheses
- JSX itself is an expression, we can assign it to a variable, return it from a function, use it inside of if or for loop, accept it as an argument

Props and Components

- All component attributes are passed as properties of props object
- React component name must start with Capital letter, except hooks, hooks start with `useHookName`
- Since react components should/must be pure functions, props must be read only. At least _with respect to prop a react component must be a pure function_.

  > How [environment variable are set in create-react-script](https://create-react-app.dev/docs/adding-custom-environment-variables/)
  > .tsx allow jsx elements in files while .ts is for plain old JS files only

- React components automatically re-render whenever there is a change in their state or props.
- Component vs PureComponent </br> `shouldComponentUpdate()` lifecycle method of component always returns true while `shouldComponentUpdate()` method of PureComponent do shallow comparison of prop and state variables. In case of parent re-render a pure component don't re-render if prop or state variable don't change for pure component. While a Component will always re-render on the rendering of parent component.
- `React.memo` do same for a functional component that a pure-component do for a class based component
- Higher Order Component - are wrapper components, which provide additional functionality to child components. Like if we want to implement a increment counter functionality in multiple places, we create a higher order component which will implement this functionality and wrap all child component in this HOC.</br>
  i.e. `const ironman = withSuit(TonyStark)` </br>
  i.e. react-redux connect is a HOC, styledComponent are HOC

Lifecycle

- class constructor must alway call the base constructor with props.

  ```
  constructor(props){
     super(props);
  }
  ```

- **mounting** whenever a component render first time in a DOM.
- Lifecycle Steps

  1. When A Component is used somewhere
  2. **Constructor** - of Component is called with props
  3. **Render** - method is called for the first time, and it will be called again when there is change in state
  4. **ComponentDidMount** - called after render method. is called only once when component render for first time
  5. **ComponentDidUpdate** - called after render method call. only if there is change is state, get called every time whenever we update the state of the component
  6. **shouldComponentUpdate** - runs after every setState call, before any other method

State

- `setState(([prevState],props)=>newState, callback)`
- Why we need **state** - states allow to change the component output over time based on external events,network,actions. State is private to the component and fully controlled by the component
- multiple set state calls are executed at once
- Every time when we set the **state** render method is called again
- **setState** is executed in asynchronous
- **setState** shallow merge the provided object into existing state
- `setState()` will always lead to a re-render unless `shouldComponentUpdate()` returns false.

Event Handlers

- `e.preventDefault();`
- how do we pass function/even handler in react JSX `<button onClick={activateUser}/>`
- [Binding this with a callback function, why and how](https://www.andreasreiterer.at/bind-callback-function-react/), By default class method are not bound
- Dont need to bind arrow functions

useEffect Hook

- useEffect() = componentDidMount + componentDidUpdate + componentWillUnmount
- useEffect() use when need to manually change the dom, fetch the data
  > In react we have two type of side effects, one which require clean up and another which doesn't(network call, manual dom update)
- by default useEffect() runs after each render(after first mount and updates), but it can be controlled
- Unlike componentDidMount and componentDidUpdate, effects scheduled with useEffect don't block the browser from updating the screen **?**
- if the function(effect) passed in useEffect, returns another function. react will executed the returned function when it's time to cleanup the component(same as componentWillUnmount)
- we can use multiple useEffect hooks, and they will execute in the order they were written in the function
  > Forgetting to handle componentDidUpdate properly is a common source of bugs in React applications.
- How we can skip use effect, We can tell react to skip applying an effect if a certain value didn't change between re-renders, by passing an array as second argument to useEffect hook. Only pass values from props and state

[useState](https://blog.logrocket.com/a-guide-to-usestate-in-react-ecb9952e406c/)

- If we pass the same value as he previous in setState react doesn't re-render the DOM.

[Accessing React State right after setting it](https://dev.to/dance2die/accessing-react-state-right-after-setting-it-2kc8)

Misc

- To hide a component return null instead of its render output.
