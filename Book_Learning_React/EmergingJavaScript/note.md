
#TODO: 1.
### Arrow functions do not block `this`
For example, `this` becomes something else in the `setTimeout` callback, not the `tahoe` object

```javascript
var tahoe = {
    resorts: ["kirkwood","squaw","alpine","heavely","northstar"],
    print: function(delay=1000){
        setTimeout(function(){
            console.log(this.resorts.join(','))
        }, delay)
    }
}
tahoe.print() // Cannot read property 'join' of undefined
```

This error is thrown because it is trying to use the `.join` method on what `this` is. In this case, it is the `window object`. Alternatively, we can use the `arrow function` syntax to protect the scope of `this`

```javascript
var tahoe = {
    resorts: ["kirkwood","squaw","alpine","heavenly","northstar"],
    print: function(delay=1000){
        setTimeout(() => {
            console.log(this.resorts.join(","))
        }, delay)
    }
}
```

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