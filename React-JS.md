# React Documentation learning notes
I started react last year in 2021, since that time, I was working on a side project (MasterTheDocs) with, but I didn't really took time to 
go through all its documentation just to have a global overview. That is what I started and here are my notes about it.

##### *Next.js, Gatsby*
__________
## MAIN CONCEPTS

### Rendering
React Only Updates What’s Necessary.
React DOM compares the element and its children to the previous one, and only applies the DOM updates necessary to bring the DOM to the desired state.

### Components and props
Components allows us to split our react application into isolated, reusable and independant peaces of code called *components*.
And a component parent could interact with it children using *props*.

We have two ways to create a react component:
- a function component like this:
```javascript
function learningNotes(props) {
  return <h1> Hello, welcome to {props.name} learning notes</h1>
}
```
- a class:
```javascript
class LearningNotes extends React.Component {
  render () {
    return <h1> Hello, welcome to {this.props.name} learning notes</h1>
  }
}
```.
Then the component could be reused like this:
```javascript
<LearningNotes name="Horace FAYOMI"/>
```

**Important notes**:
- Always start component names with a capital letter.
React treats components starting with lowercase letters as DOM tags.
- **Pure function** are functions that never change their input and always return the same result given the same input.
- **All React components must act like pure functions with respect to their props**

### State and Lifecycle
- State: a component state is basically a kind of local store for that component. It's the place where we store the variables that use to change.
- Lifecycle methods: they are methods that allows us to execute action at different level of react lifecycle
    - *componentDidMount()* : runs after the component output has been rendered to the DOM
    - 
Example:
```javascript
class LearningNotes extends React.Component {
  constructor (props) {
    super(props)
    this.state = {
      notes: [
        {
          id: 1,
          payload: "React is an awesome frontend library",
        },
        {
          id: 2,
          payload: "React use JXS"
        }
      ]
    }
   
   componentDidMount () {
      console.log('Component mounted in the DOM')   
   }
   
   componentWillUnmount () {
    console.log('Component is about to unmount.')
   }

   render () {
      return (
        <ul>
          {this.state.notes.forEach(note => {
            <li key={note.id}>
              {note.paylaod}
            </li>
          })}
        </ul>
      ) 
   }
  }
}
```

**Important note:**
- While this.props is set up by React itself and this.state has a special meaning, you are free to add additional fields to the class manually if you need to store something that doesn’t participate in the data flow (like a timer ID).
- **ComponentWillUnmount** is used as a cleaner methods, used to clean **Event listeners**, **Timers** and **DOM elements** manually inserted by us
in the lifecycle of the component.

- because `setState` and `props` are updated asynchonously, we shoud not rely on their value to compute the next state like this:
```javascript
// wrong
this.setState({ result: this.state.result + this.props.increment })
```
Intead use this:

```javascript
this.setState((state, props) {
  // here we are sure *state* and *props* are already upadted
  return { result: this.state.result + this.props.increment }
})
```
- In react **data flow down** also known as **unidirectionnal** flow

### Handling Events

Handling events with React elements is very similar to handling events on DOM elements.

We have mainly 3 ways to use events:
- First way 
```javascript
class LearningNotes extends React.Component {
  construstor (props) {
    super(props)
    this.state = {
      selectedNoteId: null,
      notes: []
    }
    this.selectNote = this.selectNote.bind(this)
  }
  // simple function
  selectNote () {
    // this is available because it has been binded into the constructor
    this.setState({ selectedNoteId: ... })
  }
  render () {
    return <>
      {this.state.notes.forEach(note => (
        <button onClick={this.selectNote}> Note {this.state.note.id} </button>
      ))}
    </>
  }
 }
 
 - second way
 ```javascript
 class LearningNotes extends React.Component {
    constructor (props) {
      super(props)
      this.state = {
        notes: [],
        selectedNoteId: null
      }
      // No binding
    }
    selectNote = () => {
        console.log('called')
    }
    render () {
      return <button onClick={this.selectNote}> Select </button>
    }
 }
 ```
 - Third way
 ```javascript
 class LearningNotes extends React.Component {
    constructor (props) {
      super(props)
      this.state = {
        notes: [],
        selectedNoteId: null
      }
      // No binding
    }
    selectNote () {
        console.log('called')
    }
    render () {
      return <button onClick={() => this.selectNote()}> Select </button>
    }
 }
 ```
 
 #### Passing Arguments to Event Handlers
 - `return <button onClick={() => this.selectNote(nodeId)}> </button>`
 - `return <button onClick={this.selectNote.bind(this, noteId)} </button>`


### Input
**Important notes:**
- In react most inputs are controlled, except `<input type="file" />` which is uncontrolled by default as we can set the
file programatically, only read it. Though, we can still use input as uncontrolled components using **ref**


### Lift the state up
**Important notes:**
- In react we should always have a single source of truth for our data. If there a component in same or highter level that need a given
data it's better to move it the nearest common parent

### Composition vs Inheritance
#### Composition
In react we can have something similar to *slot* in VueJS by leveraging the default props **children** and pass the children component
like this:
```javascript
// ParentComponent
<div>
    {this.props.children}
</div>

// Use of the ParentComponent
<ParentComponent>
  <SomeChild1 />
  <SomeChild2 />
  ...
</ParentComponent>
```
All the children will be accessible inside parent as **props.children**.
- React is more flexible, as **even components** are objects so can be used like this
```javascript
<ParentComponent
  headerComponent={<Header />}
  bodyComponent={<Body />}
  footerComponent={<Footer />}
>

// Inside the parent
render () {
  return (
    <>
      <div className="header">
        {this.props.headerComponent}
      </div>
      <div className="body">
        {this.props.bodyComponent}
      </div>
      <div className="footer">
        {this.props.footerComponent}
      </div>
    </>
  )
}
```

____
## Advanced topic
### Code-Splitting
- React allow us to split up the bundle into small ones and load them on need at run time to improve the speed of our website.
- We have many 3 ways to do that:
    - *dynamic import*
    - Combination of *React.lazy* and *React.Suspense*
    - Lazyloading routes using *React router* library + Suspense + React.lazy

### Context
The context is used to share across all components down the tree a global value like theme.
We can share as well a method to update the context value.
This is possible by:
- creating a context using `React.createContext`
- Using a Context Provider from the created context `MyContext.Provider`
- use the context into children either with `MyContext.Consumer` or by setting the attribute `contextType` of the child component as `MyContext`

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

