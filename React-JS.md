# React Documentation learning notes
I started react last year in 2021, since that time, I was working on a side project (MasterTheDocs) with, but I didn't really took time to 
go through all its documentation just to have a global overview. That is what I started and here are my notes about it.

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

**Important note:***
- While this.props is set up by React itself and this.state has a special meaning, you are free to add additional fields to the class manually if you need to store something that doesn’t participate in the data flow (like a timer ID).

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

