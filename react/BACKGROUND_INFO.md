# BACKGROUND INFO

#TODO: 1.
### Composition vs Inheritance
Composition
_
- Code reuse is primarily achieved through composition rather than inehritance in REACT

- object composition is a way to combine objects or data types into more complex ones

Inheritance 
_
- (At Facebook, we use React in thousands of components, and we have not found any use cases where we would recomment creating component inehritance hierarchies)

{children} props
_
- Sidebar / Dialog ...

- some components do not know their children ahead of time (special children props)


### React Component LifeCycle
Mounting
_
- called in following order when an instance of a component is being created and inserted into the DOM

1) constructor()
2) static getDerivedStateFromProps()
3) render()
4) componentDidMount()

xx componentWillMount() xx

Updating
_
- update can be caused by changes to props or state. Called in following order

1) static getDerivedStateFromProps()
2) shouldComponentUpdate()
3) render()
4) getSnapshotBeforeUpdate()
5) componentDidUpdate()

xx componentWillUpdate() xx

Unmounting
_

- this method is called when a component is being removed from the DOM

1) componentWillUnmount()

SetState()
_
- enqueues changes to the component state and tells React that this component and its children need to be re-rendered with the updated state

- "request" rather than an immediate command to update the component

- does not always immediately update the component.
    (This makes reading this.state right after calling setState() a potential pitfall)

    Instead, use `componentDidUpdate` or a `setState` callback (setState(updater, callback)) guarantee to fire after the update has been applied

- setState() will always lead to a re-render unless shouldComponentUpdate() returns `false`

- This form of `setState()` is also asynchronous, and multiple calls during the same cycle may be batched together
SOLUTION: updater function form

- Updater Argument : If you need to set the state based on the previous state, read about the updater argument


