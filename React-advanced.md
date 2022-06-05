# React Documentation learning notes
I started react last year in 2021, since that time, I was working on a side project (MasterTheDocs) with, but I didn't really took time to 
go through all its documentation just to have a global overview. That is what I started and here are my notes about it.

##### *Next.js, Gatsby, chakra-ui.com, *
__________
## Advanced topic
### Code-Splitting
- React allow us to split up the bundle into small ones and load them on need at run time to improve the speed of our website.
- We have many 3 ways to do that:
    - *dynamic import*
      ```javascript
      import("./math").then(math => {
        console.log(math.add(16, 26));
      });
      ```
    - Combination of *React.lazy* and *React.Suspense*
        ```javascript
          <div>
            <Suspense fallback={<div>Loading...</div>}>
              <OtherComponent />
            </Suspense>
          </div>
        ```
    - Lazyloading routes using *React router* library + Suspense + React.lazy
    - We can laso name the routes for the DevTools by overriding `displayName` of the 

### Context
The context is used to share across all components down the tree a global value like theme.
We can share as well a method to update the context value.
A component could consume more than one context.

This is possible by:
- creating a context using `React.createContext`
- Using a Context Provider from the created context `MyContext.Provider`
- use the context into children either with `MyContext.Consumer` or by setting the attribute `contextType` of the child component as `MyContext`
- We can also names the context for the DevTools
```javascript
const MyContext = React.createContext(/* some value */);
MyContext.displayName = 'MyDisplayName';

<MyContext.Provider> // "MyDisplayName.Provider" in DevTools
<MyContext.Consumer> // "MyDisplayName.Consumer" in DevTools
```

**Important note:**
- **If the context value is static**
```javascript
value={{somehting: 2}}
```
Then each time the Context provider's parent will re-render , all the children consumers will rerender as well because a new object will be created
each time. So because react works using refs, it's better to link the value to a variable in the state:
```javascript
value={this.state.contextValue}
```
- A child component could override the context before passing it down to his children
- a child component listen to the nearest context in the tree

### Error boundary
Basicallly, **Error boundaries** are just react components that catch any errors that occurs in their child component
tree and display a fallback UI. They catch error during **rendering**, **lifecycle** methods and **constructor**.
But **they don't catch errors that occurs in**:
- asynchronous functions
- event call back
- in their host parent or above it.

## Refs
Refs allow to access the dom elements to performs some actions outside React flow like:
- focus, text selection, media manipulation
- triggering animation
- integrating with third party libraries

- You can set ref only on class component. Not on function components because they don't have instance. instead use forwarding ref
if you want to use it in function components. But we can still use *ref* inside a function component.
```javascript
let ref = React.CreateRef
<Component ref={ref} />
```
- We can also use ***Callback Refs*** a function version of refs.

### Forwarding Refs
Ref forwarding is a technique for automatically passing a ref through a component to one of its children.
It's a feature of react that allows a parent component to access the DOM of a element among his children in order to perform
DOM manipulation like:
- focus
- selection
- animation
- ...

### Fragments
In React, fragments are components `<React.Fragment />` that allows you to group some elements without adding extra nodes to the DOM.
And they have a shortcut that is
```javascript
<>
</>
```
Currently the component allow the `key` attribute.

### Higher-Order component
It's a pure javascript function, that takes a component as argument, improves it, and return a new component.
It allows reusablity of a given pattern or logic.

**Caveats:**
- **Don't use HOC insde render method**, because as it will create a new component each time when render update, instead if diffing and only
update what has changed.
- Instead Apply HOC outside the component definition
- **Static methods of the wrapped components are not passed by default**. You need to pass them manually like this:
```javascript
function hocEnhancer (wrappedComponent) {
  class EnhancedComponent extends React.Component {
      ...
      render() {
          // ... and renders the wrapped component with the fresh data!
          // Notice that we pass through any additional props
          return <WrappedComponent data={this.state.data} {...this.props} />;
        }
  }
  EnhancedComponent.staticMethod = wrappedComponent.staticMethod
  return EnhancedComponent
}
```
or we can use *hoistAllNonReactMethods*

```javascript
import hoistNonReactStatic from 'hoist-non-react-statics';
function enhance(WrappedComponent) {
  class Enhance extends React.Component {/*...*/}
  hoistNonReactStatic(Enhance, WrappedComponent);
  return Enhance;
}
```
- **key** and **ref** are not really **props**. They are handled specially by React.
- So **Refs Aren’t Passed Through**, and to circumvent that, you can use **Forwarding ref**.

### JXS in deep
- JXS is not a requirement to use React. It's just more convenient. Using React without JSX could be convenient especially when you don’t want to set up compilation in your build environment.
- Each JSX element is just syntactic sugar for calling ***`React.createElement(component, props, ...children)`***
- JXS support dot (`.`) naming
```javascript
function BlueDatePicker() {
  return <MyComponents.DatePicker color="blue" />;
}
```
- JXS can't choose component at runtime
```javascript
function Story(props) {
  // Wrong! JSX type can't be an expression.
  return <components[props.storyType] story={props.story} />;
}

function Story(props) {
  const SpecificStory = components[props.storyType];
  // Correct! JSX type can be a capitalized variable.
  return <SpecificStory story={props.story} />;
}
```
- **If we define a props and pass no value to it, it automatically take value `true`**. Though it's advised to always set a value.
```javascript
<MyTextBox autocomplete />  === <MyTextBox autocomplete={true} />
```
- We can spead attributes:
```javascript
const props = {firstname: "Horace", lastname: "FAYOMI"}
return <User {...props} />
// or
function CustomButton (props) {
  let {color, otherProps} = props
  let class = color === 'blue' ? 'primary' : 'secondary'
  return <button  className={class} {...otherProps} />
}
```
- **A React component can also return an array of elements**:
```javascript
render() {
  // No need to wrap list items in an extra element!
  return [
    // Don't forget the keys :)
    <li key="A">First item</li>,
    <li key="B">Second item</li>,
  ];
}
```
- **React components can take functions as children**:
```javascript
return (
  <Repeat numTimes={10}>
    {(index) => <div key={index}>This is item {index} in the list</div>}
  </Repeat>
);
```
- Booleans, Null, and Undefined Are Ignored. **false, null, undefined, and true are valid children. They simply don’t render**. 
```javascript
// Here 0 will be printed if messages = []. Make sure the first expression is boolean.
<div>
  {props.messages.length && <MessageList messages={props.messages} />}
</div>
```

### Optimizing Performance
- User production files
- User DevTool profiling tool
- Virtualize Long Lists
- Define **shouldComponentUpdate** lifecycle method to avoid useless **Reconciliation** that you migh notice:
```javascript
shouldComponentUpdate(nextState, nextProps) {
   if (this.props.color !== nextProps.color) return true;
   return false;
}
```
Or we can use ***React.PureComponent***.
React.PureComponent is similar to React.Component. The difference between them is that **React.Component doesn’t implement shouldComponentUpdate(), but React.PureComponent implements it** with a shallow prop and state comparison.

*If these contain complex data structures, it may produce false-negatives for deeper differences. For instance if one of the props is an arrays, the shallow comparision will end up with true even if the array items change. A solution is the not mutate the data but assign the new created object by using either `Object.assign({}, state.data)` or `array.contact` or `[...araray]`, `{...data}`*.

If your React component’s render() function renders the same result given the same props and state, you can use React.PureComponent for a performance boost in some cases.

### Portals
A portal allow a component to render some children into a root node different than the parent root node.
***A typical use case for portals is when a parent component has an overflow: hidden or z-index style, but you need the child to visually “break out” of its container. For example, dialogs, hovercards, and tooltips.***
**Important:**
- Even though a portal can be anywhere in the DOM tree, it behaves like a normal React child in every other way. Features like context work exactly the same regardless of whether the child is a portal, as the portal still exists in the React tree regardless of position in the DOM tree. So even if ComponentA and ComponentB are siblings in the dom node, if ComponentA render ComponentB (ComponentB is a children in the React tree of componentA), ComponentB will have the same behavior as another child that is on the same root as ComponentA.
```javascript
ReactDOM.createPortal(child, domNode)
```

### Profiler API
The Profiler measures how often a React application renders and what the “cost” of rendering is. Its purpose is to help identify parts of an application that are slow and may benefit from optimizations such as memoization.
```javascript
render () {
    return (
        <Profiler id="Myprofiler" onRender={renderCallback}>
            ...
        </Profile>
    )
}
```
**Important:**
- Profilers can be nested
- Profiling adds some additional overhead, so it is disabled in the production build.


### reconciliation algorithm
**Important:**
- Inserting an element at the beginning has worse performance.


### Render prop
It's a prop that carry a function that return a component. It's usefull when a component that has it state or logic encapsulated whant to share it with another in a flexible way.

**Important**
- Render props should be used carefully when dealing with `PureComponents` because the comparision of changes could most of times falls down to `false`
because at each render, new object might be created (`render={() => return <SomeComponent/>}`, Instead it's better to have a named class method that return the component so the reference is kept.)


### Strict mode
StrictMode is a tool for highlighting potential problems in an application. Like Fragment, StrictMode does not render any visible UI. It activates additional checks and warnings for its descendants.
Strict mode checks are run in development mode only; they do not impact the production build.

### Typechecking With PropTypes
It's the internal type checking of react.
- We can **Requiring Single Child With PropTypes.element**
- We can also define default values for props
- They could also be added into function components


