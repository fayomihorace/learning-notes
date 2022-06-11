## React Hooks
Hello, these are my notes about React hooks.

Simply speaking, hooks are features that allows you using state and other react features without writing a class.
**You can't use Hooks inside class components.**

#### Rules of Hooks
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
but unified into a single API.

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
**Some action need to be clean up otherwise that will introduce memory leaks**.

Effects may also optionally specify how to “clean up” after them by returning a function. Exemples:
- the effect is to subscribe to an API, and the cleaning is to unsubscribe.
- manually created dom Element that will only be used by the component
- timers
- event handlers
**React also cleans up effects from the previous render before running the effects next time. This helps avoid bugs.**
But we can remove that behavior in case it creates performance issues later below.


___
#### useContext()


____
#### useReducder()


____
### Additional Hooks
useReducer
useCallback
useMemo
useRef
useImperativeHandle
useLayoutEffect
useDebugValue
useDeferredValue
useTransition
useId

### Library Hooks
useSyncExternalStore
useInsertionEffect


____
### FAQ

