#TODO: 1.
### What It Means to Be Functional

- JavaScript supports functional programming because JavaScript functions are first-class citizens
    - This means that functions can do the same things that variables can do

Since functions are variables, we can add them to objects

```javascript
const obj = {
    message: "They can be added to objects like variables",
    log(message){
        console.log(message)
    }
}
obj.log(obj.message)
// They can be added to objects like variables
```

We can also add functions to arrays in JavaScript

```javascript
const messages = [
    "They can be inserted into arrays",
    message => console.log(message),
    "like variables",
    message => console.log(message)
]

messages[1](messages[0])
// They can be inserted into arrays

messages[3](messages[2])
// like variables
```

Functions can be sent to other functions as arguments

```javascript
const insideFn = logger =>
    logger("They can be sent to other functions as arguments")

insideFn(message => console.log(message))
```

Functions can also be returned from other functions, just like variables

```javascript
var createScream = function(logger){
    return function(message){
        logger(message.toUpperCase() + "!!!")
    }
}

// More than one arrow means that we have HOC

const creatScream = logger => message =>
    logger(message.toUpperCase() + "!!!")

const scream = createScream(message => console.log(message))

scream("Functions can be returned from other functions")
// FUNCTIONS CAN BE RETURNED FROM OTHER FUNCTIONS!!!
```

#TODO: 2.
### Imperative Versus Declarative

What is imperative programming
_

- style of programming that is only concerned with how to achieve results with code

- Imperative programs require lots of commentsi n order to understand what is going on

What is declarative programming
- The declarative approach is more readable and, thus, easier to reason about

- The details of how each of these functions is implemented are abstracted away

- Those tiny functions are naed well and combined in a way that describes how member data goes from being loaded to being saved and printed on a map

- It does not require many comments

- React is declarative

Functional Concepts
_

What is Functional Programming
- Immutability
- Purity
- Data Transformation
- Higher-order Function
- Recursion

