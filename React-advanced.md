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
