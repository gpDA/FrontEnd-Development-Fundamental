

#### TAKEAWAY : 
`Arrow functions` do not block off the scope of `this`

NOT WORKING
Changing the `print` function to an arrow function means that `this` is actually the `window`
```javascript
var tahoe = {
    resorts: ["kirkwood","squaw","alpine","heavenly","northstar"],
    print: (delay=1000) => {
        setTimeout(() => {
            console.log(this.resorts.join(","))
        }, delay)
    }
}
tahoe.print() // cannot read property resorts of undefined
```



#TODO: 2.
### Transpiling ES6
Transpiling is not compiling: our code is not compiled to binary. Instead, it is transpiled into syntax that can be interpreted by a wider range of browsers

ES6 code before Babel transpiling
```javascript
const add = (x=5, y=10) => console.log(x+y);

// after run transpiler, output...

"use strict";

var add = function add(){
    var x = arguments.length <= 0 || arguments[0] === undefined ? 5 : arguments[0];
    var y = arguments.length <= 1 || arguments[1] === undefined ? 10 : arguments[1];
    return console.log(x + y);
}
```



#TODO: 3.
### ES6 Objects and Arrays

Destructuring Assigment
_

```javascript
var sandwich = {
    bread: "dutch crunch",
    meat: "tuna",
    cheese: "swiss"
}
var { bread, meat } = sandwich
```

Destrcture incoming function arguments
_

```javascript
var regularPerson = {
    firstname: "Bill",
    lastname: "Wilson"
}

var lordify = regularperson => {
    console.log(`${regularPerson.firstname} of Canterbury`)
}

// is equal to 

var lordify = ({firstname}) => {
    console.log(`${firstname} of Canterbury`)
}

lordify(regularPerson)
```

Destructuring from arrays
_

```javascript
var [firstResort] = ["Kirkwood", "Squaw", "Alpine"]
console.log(firstResort) // Kirwood

// pass over unnecessary values with list matching using commas
var [,,thirdResort] = ["Kirwood", "Squaw", "Alpine"]
console.log(thirdResort) // Alpine
```

Object Literal Enhancement
_

- the opposite of destructuring. The process of restructuring or putting back together
- allows use to pull global variables into objects
- reduces typing by making the `function` keyword unnecessary

```javascript
var name = "Tallac"
var elevation = 9738

var funHike = {name, elevation}

console.log(funHike) // {name: "Tallac", elevation:9738}

// create object METHODS with Object Literal Enhancement
var name = "Tallac"
var elevation = 9738
var print = function(){
    console.log(`Mt. ${this.name} is ${this.elevation} feet tall`)
}

var funHike = {name, elevation, print}
funHike.print() // Mt. Tallac is 9738 feet tall
```

When defining object methods, it is no longer necessary to use the `function` keyword
_

```javascript
// OLD
var skier = {
    name: name,
    sound: sound,
    powerYell: function(){
        var yell = this.sound.toUpperCase()
        console.log(`${yell} ${yell} ${yell} !!!`)
    },
    speed: function(mph){
        this.speed = mph
        console.log('spped: ', mph)
    }
}

// NEW
const skier = {
    name,
    sounds,
    powderYell(){
        let yell = this.sound.toUpperCase()
        console.log(`${yell} ${yell} ${yell} !!!`)
    },
    speed(mph){
        this.speed = mph
        console.log('speed: ', mph)
    }
}
```

Spread Operator
_

- Copy (do not need to mutate the original array)

    - `reverse` function has actually altered or mutated the array
    - In a world with the `spread operator`, we do not have to mutate the original array;
        - we can create a copy of the array and then reverse it

```javascript
var peaks = ["Tallac", "Ralston", "Rose"]
var [last] = [...peaks].reverse()

console.log(last) // Rose
console.log(peaks.join(', ')) // Tallac, Ralston, Rose
```

- Collecting function arguments as an array

```javascript
// build a function that takes n number of arguments using the spread operator

function directions(...args){
    var [start, ...remaining] = args
    var [finish, ...stops] = remaining.reverse()
}
```

- Using spread operator with objects

```javascript
var morning = {
    breakfast: "oatmeal",
    lunch: "peanut butter and jelly"
}
var dinner = "mac and cheese"

var backpackingMeals = {
    ...morning,
    dinner
}

console.log(backpackingMeals)
// { breakfast: "oatmeal",
//   lunch: "peanut butter and jelly",
//   dinner: "mac and cheese"}
// }
```

#TODO: 4.
### Promises

- Promise give us a way to make sense out of `async` behavior
- We could also receive multiple types of errors. `Promises` give us a way to simplify back to a simple `pass` or `fail`

If the promise is successful, the data will load
If the promise is unsuccessfuly, an error will occur

```javascript
const getFakeMembers = count => new Promise((resolves, rejects) => {
    const api = `https://api.randomuser.me/?nat=US&results=${count}`
    const request = new XMLHttpRequest()
    request.open('GET', api)
    request.onload = () => 
        (request.status === 200) ?
        resolves(JSON.parse(request.response).results) :
        reject(Error(request.statusText))
    request.onerror = (err) => rejects(err)
    request.send()
})

// We can use the promise by calling function and passing in the number of members that should be loaded
// then function can be chained on to do something once the promise has been fulfilled (composition)

getFakeMembers(5).then(
    members => console.log(members),
    err => console.error(
        new Error("cannot load members from randomuser.me")
    )
)
```

#TODO: 5.
### Classes

- Previously in JavaScript, there were no official classes
- Using a class still means that you are using JavaScript's prototypal inheritance

```javascript
// OLD
function Vacation(destination, length){
    this.destination = destination
    this.length = length
}
Vacation.prototype.print = function(){
    console.log(this.destination + " | " + this.length + " days")
}

var maui = new Vacation("Maui", 7)
maui.print(); // Maui | 7

// NEW
class Vacation{
    constructor(destination, length){
        this.destination = destination
        this.length = length
    }
    print(){
        console.log(`${this.destination} will take ${this.length} days.`)
    }
}

const trip = new Vacation("Santiago, Chile", 7);
console.log(trip.print()) // Chile will take 7 days

// Classes can also be extended.
// When a class is extended, the subclass inheirts the properties and methods of the superclass

class Expedition extends Vacation{
    constructor(destination, length, gear){
        super(destination, length)
        this.gear = gear
    }
    print(){
        super.print()
        console.log(`Bring your ${this.gear.join(" and your ")}`)
    }
}

const trip = new Expedition("Mt. Whiteney", 3,
                ["sunglasses", "prayer flags", "camera"])
trip.print()
// Mt. Whitney will take 3 days
// Bring your sunglasses and your prayer flags and your camera
```

#TODO: 6.
### ES6 Modules

- JavaScript modules are stored in separate files, one file per module. There are two options when creating and exporting a module

Export multiple variable from a module

```javascript
export const print(message) => log(message, new Date())

export const log(message, timestamp) => 
    console.log(`${timestamp.toString()}: ${message}`)
```
*timestamp = new Date()*
*timestamp.toString()*

Export only One type from a module

```javascript
const freel = new Expedition("Mt. Freel", 2, ["water", "snack"])
export default freel

```

- Modules can be consumed in other JavaScript files using the `import` statement
    - modules with multiple exprots can take advatange of object destructing

```javascript
// Modules can be consumed in other JavaScript files using the `import` statement
import { print, log } from './text-helpers'

import {print as p, log as l } from './text-helpers'
p('printing a message')
l('logging a message')

// Modules that use `export default` are imported into a single variable
import freel from './mt-freel'
```