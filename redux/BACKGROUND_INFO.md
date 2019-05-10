# BACKGROUND INFO


# TODO: 1.
### Redux vs. Flux
Redux
_
- Reducers are pure functions that return a new state based on the current state and an action
```javascript
(state, action) => newState
```
- Redux only has ONE store



Flux
_
- Flux design pattern allows for MANY stores that each focus on a specific set of data


# TODO: 2.
### What is Redux
Redux Rule

- application state should be stored in a single immutable object
    - We will update this state object by replacing it entirely


# TODO: 3.
### What is Redux actions
Actions
_
- In order to do this, we will need instructions about "what changes" `actions`
- `actions` provide : 1) instructions about what should change in the application state along with necessary data to make those changes
- `actions` are the only way to update the state of a Redux application
- We can also look at them like receipts about the history of what has changed over time
- Most state changes also required some data : this data --> `action's payload`
    - For example, when we dispatch an action like RATE_COLOR, we will need to know 1) what color to rate 2) what rating to apply to that color ...


# TODO: 4.
### What is Redux reducers
Reducers
_
- Entire state is stored in a single object
- Redux acheives modularity via functions
    - Functions are used to update parts of the state tree; functions are called `reducers`
- `reducers` are functions that take the current state along with an action as arguments and use them to create and return a new state
- We can compose reducers into one reducer that can handle updating the entire state of our app given any action
- Each function is focused on a specific part of our state tree. FOR EXAMPLE,

```javascript
import C from '../constants'
export const color = (state={}, action) => {
    return {}
}
export const colors = (state=[], action) => {
    return []
}
export const sort = (state="SORTED_BY_DATE", action) => {
    return ""
}
```

Example of reducers

```javascript
export const colors = (state=[], action) => {
    switch(action.typ){
        case C.ADD_COLOR:
            return [
                ...state,
                color({}, action)
            ]
        case C.RATE_COLOR:
            return state.map(
                c => color(c, action)
            )
        // creates a new array by filtering out
        case C.REMOVE_COLOR:
            return state.filter(
                c => c.id !== action.id
            )
        default:
            return state
    }
}
```

*`combineReducers` combines all of the reducers into a single reducer*
```javascript
const store = createStore(
    combineReducers({color, sort})
)
conosle.log(store.getState())
// {
//  colors: [],
//  sort: "SORTED_BY_DATE"
//}
```

```javascript
const store = createStore(
    combineReducers({color, sort}),
    initialState 
)
```


# TODO: 5.
### What is Redux Dispatching actions
Dispatching actions
_
- The only way to change the state of your application is by dispatching actions through the store
    - The store has `dispatch` method that is ready to take actinos as an argument
```javascript
store.dispatch({
    type: 'ADD_COLOR',
    id: ...,
    title: ...,
    color: ...,
    timestamp: ...
})
```
- Actions creators simplify the task of dispatching actions; we only need to
1) call a function
2) send it the necessary data
```javascript
export const removeColor = id => 
    ({
        type: C.REMOVE_COLOR,
        id
    })

store.dispatch(removeColor("id...."))
```

```javascript
// action SORTBY
import C from './constants'

export const sortColors = sortedBy =>
    (sortedBy === "rating") ?
        ({
            type: C.SORT_COLORS,
            sortBy: "SORTED_BY_RATING"
        }) :
    (sortedBy === "title") ?
        ({
            type: C.SORT_COLORS,
            sortBy: "SORTED_BY_TITLE"
        }) :
        ({
            type: C.SORT_COLORS,
            sortBy: "SORTED_BY_DATE"
        })

store.dispatch(sortColors("title"))
```


# TODO: 6.
### What is Redux Subscription to Stores
Subscribing to Stores
_
- Stores allow you to subscribe handler functions that are invoked every time the store completes dispatching an action
```javascript
store.subscribe(() => 
    console.log('color count:', store.getState().colors.length)
)

// unsubscribe ALSO available
```


# TODO: 1. 
*No side effect in Reducers*
- Generating random data, calling APIs, and other asynchronous processes should be handled outside of reducers


# TODO: 2. 
*Use of constants instead of a string*
```javascript
const constants = {
    SORT_COLORS: "SORT_COLOR",
    //...
}
export default constants

// INSTEAD OF
{type: 'ADD_COLOR'}
// USE
import C from "./constants"
{type: C.ADD_COLOR}
```


# TODO: 3. 
*ES7 object spread operator allows us to assign the value of the current state to a new object*


# TODO: 4.
*Treat State as an Immutable Object*
NO state.push({}) OR state[index].rating


# TODO: 5.
*getState method will return the present application state*
```javascript
import {createStore} from 'redux'
import {color} from './reducers'

const store = createStore(color)
console.log(store.getState()) // {}
```