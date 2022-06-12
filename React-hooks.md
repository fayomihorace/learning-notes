## React Hooks
Hello, these are my notes about React hooks.

Simply speaking, hooks are features that allows you using state and other react features without writing a class.
**You can't use Hooks inside class components.**

#### Rules of Hooks

***Helpfull extension: eslint-plugin-react-hooks***

Hooks are JavaScript functions, but they impose two additional rules:

- **Only call Hooks at the top level. Don’t call Hooks inside loops, conditions, or nested functions**.
- **Only call Hooks from React function components. Don’t call Hooks from regular JavaScript functions**. (There is just one other valid place to call Hooks — your own custom Hooks. We’ll learn about them in a moment.)

____
#### useState()
The more used hook. It's allows manipulating state variables.
It's a function that takes one parameter (initial value) and return a array of a pair:
```javascript
MyFunctionComponent function () {
  const [count, setCount] = useState(0)
  return (
    <>
      <h1> count: {count} </h1>
      <button onClick={() => setCount(count + 1)}> Increaze count</button>
    </>
  )
}
```
- **`useState()` doesn't merge previous state value with new one like `setState()m instead it replace it.`**

____
#### useEffect()
It serves the same purpose as `componentDidMount`, `componentDidUpdate`, and `componentWillUnmount` in React classes,
but unified into a single API. It accept a function as argument.
```javascript
useEffect(didUpdate)
```
**Side effects are not allowed inside the main body of a function component (referred to as React’s render phase). Doing so will lead to confusing bugs and inconsistencies in the UI.**

That why we should do them inside **useEffect** instead.

***Note: A sie effect is anything that affects something outside the scope of the function being executed.***.
Example of sides effects are:
- Setting the page title imperatively
- Working with timers like setInterval or setTimeout
- Measuring the width, height, or position of elements in the DOM
- Inserting element to the dom 
- Logging messages to the console or other service (Error tracking calls to Sentry)
- Setting or getting values in local storage
- Interacting with an API (authentication, fetch data, mutations, ...)
- Subscribing and unsubscribing to services of third party libraries
- Direct dom event listeners


- **useEffect run after every render. By default, it runs both after the first render and after every update**. This is usefull to keep consistency accross the rendering if for instance a prop has changed.
- 
But we can customize that
- Unlike componentDidMount or componentDidUpdate, effects scheduled with useEffect don’t block the browser from updating the screen. This makes your app feel more responsive.
- **Listen to change of a particular given state or prop**
   ```javascript
   useEffect(() => {
    document.title = `You clicked ${count} times`;
  }, [count]); 
  ```
- If you want to run an effect and clean it up only once (on mount and unmount), you can pass an empty array ([]) as a second argument. This tells React that your effect doesn’t depend on any values from props or state, so it never needs to re-run
- **We recommend using the exhaustive-deps rule as part of our eslint-plugin-react-hooks package. It warns when dependencies are specified incorrectly and suggests a fix.**


##### Effects Without Cleanup
```javascript
useEffect(() => console.log('Something has changed'))
```

##### Effects with cleanup
**Some side effects need to be clean up otherwise that will introduce memory leaks**.

Effects may also optionally specify how to “clean up” after them by returning a function. Exemples:
- the effect is to subscribe to an API, and the cleaning is to unsubscribe.
- manually created dom Element that will only be used by the component
- timers
- event handlers
**React also cleans up effects from the previous render before running the effects next time. This helps avoid bugs.**



____
### Additional Hooks

___
#### useContext()
useContext(MyContext) is equivalent to static `contextType = MyContext` in a class, or to `<MyContext.Consumer>`.
`useContext(MyContext)` only lets you read the context and subscribe to its changes. You still need a `<MyContext.Provider>` above in the tree to provide the value for this context.

____
#### useReducder()
`useReducer` is usually preferable to useState when you have complex state logic that involves multiple sub-values or when the next state depends on the previous one. useReducer also lets you optimize performance for components that trigger deep updates because you can pass dispatch down instead of callbacks.

```javascript
const initialState = {count: 0};

function reducer(state, action) {
  switch (action.type) {
    case 'increment':
      return {count: state.count + 1};
    case 'decrement':
      return {count: state.count - 1};
    default:
      throw new Error();
  }
}

function Counter() {
  const [state, dispatch] = useReducer(reducer, initialState);
  return (
    <>
      Count: {state.count}
      <button onClick={() => dispatch({type: 'decrement'})}>-</button>
      <button onClick={() => dispatch({type: 'increment'})}>+</button>
    </>
  );
}
```

- **Lazy initialization**
We can also create the initial state lazily. To do this, you can pass an init function as the third argument. 
It lets you extract the logic for calculating the initial state outside the reducer. This is also handy for resetting the state later in response to an action.

```javascript
function init(initialCount) {
  return {count: initialCount};
}

function Counter({initialCount}) {
  const [state, dispatch] = useReducer(reducer, initialCount, init);
  ...
}
```


____
#### useCallback()
Syntax: 

```javascript
const memoizedCallback = useCallback(() => { //.. do something with a & b }, [a, b])
```
It returns a memoized function (or callback).

It accepts two arguments - "function" and "dependency" array. It will return new i.e. re-created function only when one of the dependencies has changed, or else it will return the old i.e. memoized one.

**Every time a component re-renders, its functions get recreated, so they change**.


____
#### useMemo()
It returns a memorized value that change only if one of the dependencies change.
It's usefull to avoid expensive computation (map, filter, ...) unnecessarily.

____
#### useRef()


____
#### useImperativeHandle()


____
#### useLayoutEffect()

____
#### useDebugValue()


____
#### useDeferredValue()


____
#### useTransition()


____
#### useId()


### Library Hooks
useSyncExternalStore
useInsertionEffect


____
### FAQ

