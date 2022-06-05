Hello, these are my notes about React hooks.

Simply speaking, hooks are features that allows you using state and other react features without writing a class.
**You can't use Hooks inside class components.**

### Hooks at a Glance

### Rules of Hooks
Hooks are JavaScript functions, but they impose two additional rules:

- **Only call Hooks at the top level. Don’t call Hooks inside loops, conditions, or nested functions**.
- **Only call Hooks from React function components. Don’t call Hooks from regular JavaScript functions**. (There is just one other valid place to call Hooks — your own custom Hooks. We’ll learn about them in a moment.)

### useState()
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
- **`useState()` doesn't merge previous state value with new one like `setState()`**


### useEffect()
It serves the same purpose as `componentDidMount`, `componentDidUpdate`, and `componentWillUnmount` in React classes,
but unified into a single API.

- Effects may also optionally specify how to “clean up” after them by returning a function. Exemples:
      - the effect is to subscribe to an API, and the cleaning is to unsubscribe.
- 
