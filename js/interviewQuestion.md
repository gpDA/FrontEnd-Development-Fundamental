# interviewQuestion



#TODO: 14.
### `typeof` vs. `instanceof`

simple comparison
_
- use `instanceof` of custom type

```javascript
var ClassFirst = function () {};
var ClassSecond = function () {};
var instance = new ClassFirst();
typeof instance; // object
typeof instance == 'ClassFirst'; // false
instance instanceof Object; // true
instance instanceof ClassFirst; // true
instance instanceof ClassSecond; // false 
```

- use `typeof` for simple built in types

```javascript
'example string' instanceof String; // false
typeof 'example string' == 'string'; // true

'example string' instanceof Object; // false
typeof 'example string' == 'object'; // false

true instanceof Boolean; // false
typeof true == 'boolean'; // true

99.99 instanceof Number; // false
typeof 99.99 == 'number'; // true

function() {} instanceof Function; // true
typeof function() {} == 'function'; // true
```


- Bonus

```javascript
typeof null; // object
```

what is `typeof`
_

- `typeof` is a unary operator that returns a string indicating the type of the unevaluated operand

```javascript
const a = "I'm a string primitive";
const b = new String("I'm a String Object");

typeof a; --> returns 'string'
typeof b; --> returns 'object'
```

what is `instanceof`
_

- `instanceof` is a binary operator, accepting an object and a constructor.
It return a boolean indicating whether or not the object has the given constructor in its prototype chain

```javascript
const a = "I'm a string primitive";
const b = new String("I'm a String Object");

a instanceof String; --> returns false
b instanceof String; --> returns true
```

How to properly test for strings?
-

- need to detect both primitive strings and Strings objects as values of the object

```javascript
const isString = (str) => {
  return typeof str === 'string' || str instanceof String
}
```



#TODO: 15.
### forEach vs for loop vs for ... in

Simple Comparison
_
1. Performance : `forEach` is little slower than `for loop`
2. Usability : There is no way we can break / return from the callback `forEach`
3. Browser compatibility : `forEach` is not supported in IE < 9

what is forEach
_
1. `forEach` is a method on the `Array` prototype. It iterates through each element of an array and passes it to a callback function
2. `forEach` can only be used on Arrays, Maps, and Sets

What is for loop
_
1. `for` statment does not necessarily involve an array

what is for ... in
_
1. `for ... in` is used to iterate over the enumerable properties of objects.
    (every property in an object will ahve an `Enumerable` value
        - if that value is set to `true`, then the property is `Enumerable`)

#TODO: 16.
### JavaScript Different Data Types

Objects
_
PRINT `key` and `value` pair
```javascript
const obj = {  
  a: 1,
  b: 2,
  c: 3,
  d: 4
}

for (let elem in obj) {
  console.log(`${elem} = ${obj[elem]}`);
}
// a = 1
// b = 2
// c = 3
// d = 4

```


Arrays
_
- DON'T FORGET, ARRAYS are objects too (which means we can also use `for ... in` loop on Arrays)
```javascript
const arr = ['cat', 'dog', 'fish'];
for (let i in arr) {  
  console.log(arr[i])
}
// cat
// dog
// fish
```

String
_
- Since each character in a stirng has an index, we can even use `for ... in` on strings

```javascript
const string = 'hello';
for (let character in string) {  
    console.log(string[character])
}
// h
// e
// l
// l
// o
```

Map (ES6 in 2015)
_

```javascript
let iterable = new Map([['a', 1], ['b', 2], ['c', 3]]);
for (let entry of iterable) {
  console.log(entry);
}
for (let [key, value] of iterable) {
  console.log(key);
  console.log(value);
}

["a", 1]
["b", 2]
["c", 3]

1
"a"
2
"b"
3
"c"
```



#TODO: 17.
### What is promise in JavaScript

What is Promise
_
- `Promise` object represents the eventual completion (or failure) xof an asynchronous operation, and its resulting value

- WRITING ASYNC CODE IN PLAIN CALLBACK SYTLE IS HARD TO DO CORRECTLY
it is easy to froget to check for an error or to allow an unexpected exception


Why use Promise
_
1. prevent callback hell 
2. prevent limitation of try ... catch


#TODO: 18.
### Promise with Callback

- 프로미스가 콜백을 대체하는 것은 아니다. 사실 프로미스에서도 콜백을 사용한다.

- 프로미스는 콜백을 예층 가능한 패턴으로 사용할 수 있게 하며, 프로미스 없이 콜백만 사용했을 때 나타날 수 있는 엉뚱한 현상 / 버그를 해결

- 프로미스는 객체이므로 어디든 전달 할 수 있다 (콜백에 비해 간편한 장점)

콜백 헬
_

```javascript
const fs = require('fs');

// first callback
fs.readFile('a.txt', function(err,dataA){
  if(err) console.error(err);
  // second callback
  fs.readFile('b.txt', function(err,dataB){
    if(err) console.log(err);
    // third callback
    fs.read.readFile('c.txt', function... )
  })
})
```
==> 중괄호로 둘러싸여 끝없이 중첩된 삼각형의 코드 블록들...

Limitation of Try and Catch
_

```javascript
const fs = require('fs');
function readSketchyFile(){
  try{
    fs.readFile('does_not_exist.txt', function(err,data){
      if(err) throw err;
    });
  }catch(err){
    console.log('warning: minor issue occurred, program continuing');
  }
}

readSketchyFile();
```

- 문제점 2가지

  1. 작동을 하지 않는다. try ... catch 블록은 readSketchyFile 함수 안에 있지만, 정작 예외는 fs.readFile이 콜백으로 호출하는 익명 함수 안에서 일어났다

  2. 콜백이 우연히 두 번 호출되거나 / 아예 호출되지 않는 경우르 방지하는 장치가 없다. (js는 콜백이 정확히 한 번만 호출될 것을 보장하지 않는다)

#TODO: 19.
### Promise Chain

```javascript
funciton launch(){
  ...
}
// 이 함수를 카운트다운에 쉽게 묶을수 있다
const c = new Countdown(5);
  .on('tick', i => console.log(i + '...'));

c.go()
  .then(launch)
  .then(function(msg)){
    console.log(msg);
  .catch(function(err){
    console.error("we have a problem...")
  })
  }
```

- 프로미스 체인을 사용하면 모든 단계에서 에러를 캐치할 필요가 없다.

- 체인 어디에서든 에러가 생기면 체인 전체가 멈추고 catch 핸들러가 동작

#TODO: 20.
### 결정되지 않는 프라미스 방지하기

- 프로미스는 비동기적 코드를 단순화하고 / 콜백이 두 번 이상 실행되는 문제를 방지

- 하지만 resolve나 reject를 호출하는 걸 잊어서 프로미스가 결정되지 않는 문제까지 자동으로 해결하지 못한다.
해결 방법!! 프로미스에 타임아웃을 건다.

- 프로미스에 타임아웃을 건다

```javascript
function addTimeout(fn, timeout){
  if(timeout === undefined) timeout = 1000;
  return function(...args){
    return new Promise(function(resolve, reject){
      const tid = setTimeout(reject, timout, new Error("promise timed out"));
      fn(...args)
        .then(function(...args){
          clearTimeout(tid);
          resolve(...args);
        })
        .catch(function(...args){
          clearTimeout(tid);
          reject(...args);
        })
    })
  }
}
```

- 호출

```javascript
c.go()
  .then(addtIMEOUT(LAUNCH, 11*1000));
```

#TODO: 21.
### Solution with Promise

What is Promise
_

- `Promise` wraps an asynchronous action in an object, which can be passed around and told to do certain things when the action finishes or fails

- CHECKOUT `http://www.promisejs.org/`

- Parmeters `executor`
`executor` is a function that is passed with the arguments resolve and reject.
The `executor` function is executed immediately by the Promise implmentation, passing `resolve` and `reject` functions

- `Promise` is a proxy for a value not necessarily known when the promise is created. It allows you to associate handlers with an asynchronous action's eventual success value or failure reason. 
This lets asynchronous methos return values like synchronous methods : instead of immediately returning the final value, the asynchronous method returns a promise to supply the value at some poin in the future.

- `Promise` is in one of these states (ONLY ONCE ... (settled) FULFILLED PROMISE cannot become REJECTED PROMISE)

1. pending : initial state; (neither fulfilled nor rejected)

2. fulfilled : opeartion completed successfully

3. rejected : operation failed

- A pending promise can either be `fulfilled` with a value, or `rejected` with a reason (error). When either of these options happens, the associated handlers queued up by a promise's then method are called. 

- As the `Promise.prototype.then()` and `Promise.prototype.catch()` methods return promises, they can be chained


Methods
_

1. `Promise.all(iterable)`
returns a promise that either FULFILLS when all of the promises in the iterable
OR
REJECTS as soon as one of the promises in the iterable argument rejects
-- This method can be useful for aggregating results of multiple promises

2. `Promise.race(iterable)`
returns a promise that fulfills or rejects as soon as one of the promises in the iterable fulfills or rejects, with the value or reason from that promise

3. `Promise.reject()`
Returns a `Promise` object that is rejected with the given reason

4. `Promise.resolve()`
Returns a `Promise` object that is resolve with the given value

5. `Promise.prototype.catch()`
appends a REJECTION HANDLER callback to the promise, and returns a new promise resolving to the return value of the callback if it is called
OR
to its original fulfillment value if the promise is instaed fulfilled

6. `Promise.prototype.then()`
Appends FULFILLMENT and REJECTION handlers to the promise, AND
returns a new promise resolvign to the return value of the called handler

7. `Promise.prototype.finally()`
Appends a handler to the promise, and returns a new promise which is resolved when the original promise is resolved

To create a promise object
-
we call the `Promise` constructorm giving it a function that initializes the async action

Creating a Promise
_

```javascript
function myAsyncFunction(url) {
  return new Promise((resolve, reject) => {
    const xhr = new XMLHttpRequest();
    xhr.open("GET", url);
    xhr.onload = () => resolve(xhr.responseText);
    xhr.onerror = () => reject(xhr.statusText);
    xhr.send();
  });
}

//SIMPLE EXAMPLE
let myFirstPromise = new Promise((resolve, reject) => {
  // We call resolve(...) when what we were doing asynchronously was successful, and reject(...) when it failed.
  // In this example, we use setTimeout(...) to simulate async code. 
  // In reality, you will probably be using something like XHR or an HTML5 API.
  setTimeout(function(){
    resolve("Success!"); // Yay! Everything went well!
  }, 250);
});

myFirstPromise.then((successMessage) => {
  // successMessage is whatever we passed in the resolve(...) function above.
  // It doesn't have to be a string, but if it is only a succeed message, it probably will be.
  console.log("Yay! " + successMessage);
});
```

OR

```javascript
function get(url){
  return new Promise(function(succeed, fail){
    var req = new XMLHttpRequest();
    req.open("GET", url, true);
    req.addEventListener("load", function(){
      if(req.status < 400){
        succeed(req.responseText);
      }else{
        fail(new Error("Request failed: " + req.statusText));
      }
    });
    req.addEventListener("error", function(){
      fail(new Error("Network error"));
    });
    req.send(null);
  })
}

//calling `then` produces a new promise, whose result depends on the return value of the first function
get("example/data.txt").then(function(text){
  console.log("data.txt: " + text);
}, function(error){
  console.log("Failed to fetch data.txt: "+error);
});
```

Loading an image with XHR
-

```javascript
  <script>
  function imgLoad(url) {
    // Create new promise with the Promise() constructor;
    // This has as its argument a function
    // with two parameters, resolve and reject
    return new Promise(function(resolve, reject) {
      // Standard XHR to load an image
      var request = new XMLHttpRequest();
      request.open('GET', url);
      request.responseType = 'blob';
      // When the request loads, check whether it was successful
      request.onload = function() {
        if (request.status === 200) {
        // If successful, resolve the promise by passing back the request response
          resolve(request.response);
        } else {
        // If it fails, reject the promise with a error message
          reject(Error('Image didn\'t load successfully; error code:' + request.statusText));
        }
      };
      request.onerror = function() {
      // Also deal with the case when the entire request fails to begin with
      // This is probably a network error, so reject the promise with an appropriate message
          reject(Error('There was a network error.'));
      };
      // Send the request
      request.send();
    });
  }
  // Get a reference to the body element, and create a new image object
  var body = document.querySelector('body');
  var myImage = new Image();
  // Call the function with the URL we want to load, but then chain the
  // promise then() method on to the end of it. This contains two callbacks
  imgLoad('myLittleVader.jpg').then(function(response) {
    // The first runs when the promise resolves, with the request.response
    // specified within the resolve() method.
    var imageURL = window.URL.createObjectURL(response);
    myImage.src = imageURL;
    body.appendChild(myImage);
    // The second runs when the promise
    // is rejected, and logs the Error specified with the reject() method.
  }, function(Error) {
    console.log(Error);
  });
  </script>
```

Loading until Loads
_

```javascript
function showMessage(msg){
  var elt = document.createElement("div");
  elt.textContent = msg;
  return document.body.appendChild(elt);
}

var loading = showMessage("Loading...");
getJSON("example/bert.json").then(function(bert){
  return getJSON(bert.spounse);
}).then(function(spouse){
  return getJSON(spouse.mother);
}).then(function(mother){
  showMessage("This name is  "+ mother.name);
}).catch(function(error){
  showMessage(String(error));
//(1)
}).then(function(){
  document.body.removeChild(loading);
});
```
==> the `catch` method is similar to `then`, except that it only expects a failure handler and
wille pass through the result unmodified in case of success
MUCH Like with the `catch` clause for the `try` statement, control will continu as normal after the failure is caught
`(1)` That way, the final `then`, which removes the loading message, is always executed, even if something went wrong



#TODO: 22.
### What is JavaScript Object Properties

What is Properties
_

- `properties` are the values associated with a JavaScript object

Accessing JS Properties
_

```javascript
objectName.property         // person.age
objectName["property"]      // person["age"]
```

Adding New Properties
_

```javascript
person.nationality = "English";
```

Deleting Properties
_

```javascript
var person = {firstName:"John", lastName:"Doe", age:50, eyeColor:"blue"};
delete person.age;   // or delete person["age"]; 
```


#TODO: 24.
### Immutable.js

GITHUB documentation summary

Characteristics
- Data cannot be changed once created
- It does not update the the data in-place RATHER yield new updated data
- has no dependency (maintain PREDICTABILITY in Browser)


Data Types
- List
- Stack
- Map
- OrderedMap
- Set
- OrderedSet
- Record
- (lazy) Seq. (map // filter) --> allow efficient changing of collection method


Function
- `.equals()` ==> ===
```javascript
const { Map } = require('immutable')
const map1 = Map({a: 1, b: 2, c: 3})
const map2 = map1.set('b',2)

console.log(map1.equals(map2)) // true
console.log(map1 === map2) // true
```

- copy
* If an object is immutable, it can be "copied" simply by making another reference to it instead of copying the entire object
* Because a reference is much smaller than the object itself
  this results in 1) memory savings 2) potential boost in execution speed for programs
```javascript
const map = Map({
  a:1,
  b:2,
  c:3
})
const mapCopy = map
console.log(mapCopy)
// Map { "a":1, "b":2, "c":3 }
```

- can use function that MUTATE the collection
* the methods which would mutate the collection, like `push`, `set`, `unshift`, `splice` return a NEW IMMUTABLE COLLECTION
```javascript
const { List } = require('immutable')
const list1 = List([1,2])
const list2 = list1.push(3,4,5)
const list3 = list2.unshift(0)

console.log(list1) // List [1,2]
console.log(list2) // List [1,2,3,4,5]
console.log(list3) // List [0,1,2,3,4,5]
```


- example of map of Map
```javascript
const alpha = Map({a:1, b:2, c:3, d:4})
const alphaOutput = alpha.map((v,k) => k.toUpperCase()).join())
console.log(alphaOutput) // 'A,B,C,D'
```


- Convert from raw JS objects
* Immutable.js can treat any JS Array or Object as a Collection
```javascript
const map1 = Map({a:1, b:2, c:3, d:4})
const map2 = Map({c:10, a:20, t:30})

const map3 = map1.merge(map2) --> find key in map2 If Exists, get the value of map2. If not, get the value of map1
// Map {"a": 20, "b":2, "c":10, "d":4, "t":30}

const map4 = map2.merge(map1) --> find key in map1 If Exists, get the value of map2. If not, get the value of map2
// Map {"c":3, "a":1, "t":30, "b":2, "d":4}
```

- Convert from raw JS array
```javascript
const list1 = List([1,2,3])
const list2 = List([4,5,6])
const list3 = List([7,8,9])

const array = list1.concat(list2, list3)
// List [1,2,3,4,5,6,7,8,9]
```


- Seq
* Because Seq evaluates lazily and does not cache intermediate results, these operations can be extremely efficient
```javascript
const { Seq } = require('immutable')
const myObject = {a:1, b:2, c:3}
const result = Seq(myObject).map(x => x*x).toObject()
console.log(result) // {a:1, b:4, c:9}
```


- fromJS
* KEEP IN MIND, when using JS objects to construct Immutable Maps,
  JS Object properties are always STRINGS (even if written in a quote-less shorthand)
* Immutable Maps accept keys of any type
* Property access for JS Object first converts the key to a STRING
* BUT since Immutable Map kesy can be of any type of the argument to `get()` is not altered
```javascript
const obj = {1:"one"}
// JS Object
console.log(obj["1"], obj[1]) // "one", "one"

// vs 

// Immutable Maps
const { fromJS } = require('immutable')
console.log(map.get("1"), map.get(1)) // "one", undefined
```

- Convert back to raw JS objects

* Shallowly Using `toArray()` && `toObject()`
* Deeply Using `toJSON()`
* All Immutable Collections also implement `toJSON()` allowing them to be passed to
  `JSON.stringify` directly
```javascript
const { Map, List } = require('immutable')
const deep = map({a:1, b:2, c: List([3,4,5])})
console.log(deep.toObject()) // {a:1, b:2, c: List [3,4,5]}
console.log(deep.toArray()) // [1,2, List [ 3, 4, 5]]
console.log(deep.toJS()) // {a:1, b:2, c: [3,4,5]}
JSON.stringify(deep) // '{"a":1,"b":2,"c":[3,4,5]}'
```


- Spreading into an Array
```javascript
const aList = List([1,2,3])
const anArray = [0, ...aList, 4,5] // [0,1,2,3,4,5]
```


- Nested Structures
METHODS available for `List`, `Map`, `OrderedMap`
1) mergeDeep
2) getIn
3) setIn
4) updateIn

1) mergeDeep
```javascript
const nested = fromJS({a: {b: {c: [3,4,5]}}})
const nested2 = nested.mergeDeep({a: {b: {d: 6}}})
// Map { a: Map { b: Map { c: List [3,4,5], d: 6}}}
```

2) getIn
```javascript
console.log(nested2.getIn(['a',;'b',;'d'])) // 6
```

4) updateIn
```javascript
const nested3 = nested2.updateIn(['a','b','d'], value => value + 1)
console.log(nested3)
// Map {a: Map {b: Map {c: List [3,4,5], d: 7}}}

const nested4 = nested3.updateIn(['a','b','c'], list => list.push(6))
// Map {a: Map {b: Map {c: List [3,4,5,6], d: 7}}}
```


- Equality treats Collections as Values
* Immutable.js collections are treated as pure data values
  differs from JS's typical `reference` equal (`===` OR `==`) for Objects and Arrays, which only determines if two variables represent references to the same object instance
```javascript
// typical JS Object
const obj1 = {a:1, b:2, c:3}
const obj2 = {a:1, b:2, c:3}
obj1 !== obj2 // two different instances are always NOT equal with ===

// Map (Immutable.js)
const { Map, is } = require('immutable')
const map1 = Map({a:1, b:2, c:3})
const map2 = Map({a:1, b:2, c:3})
map1.equals(map2) // true (same value)
is(map1, map2) // true (FUNCTION comparison)
```


- Lazy Seq
* Seq is immutable (Once a Seq is created, it cannot be changed. ANY mutative method called on a `Seq` will return a new `Seq`)
* Seq is lazy 





#TODO: 1.
### JavaScript call vs. apply vs. bind vs. reduce
CALL - 
with the `call()` method, you cna write a



#TODO: 18.
### What is callback

- "CALL US BACK"

What is callback
_

- Simply, A callback is a function that is to be executed after another function has finished executing


- More Complexly, In JavaScript, functions are objects. Because of this, functions can take functions as arugments, and can be returned by other function (HIGHER-ORDER FUNCTIONS)

- ANY FUNCTION that is passed as an arugment is CALLBACK FUNCTION

Why need Callback
_

- JS is an event driven language
  - instead of waiting for a response before moving on, JS will keep executing while listening for other events

- what if the function sheduled to be executed before cannot be executed immediately?

```javascript
function first(){
  // Simulate a code delay
  setTimeout( function(){
    console.log(1);
  }, 500 );
}
function second(){
  console.log(2);
}
first();
second();

//2
//1
```

- It is not that JS did not execute our functions in the order we wanted it to,
it is instead that JS did not wait for a reponse from `first()` before movingon to execute `second()`

- YOU CANNOT JUST CALL ONE FUNCTION AFTER ANOTHER AND HOPE THEY EXECUTE IN THE RIGHT ORDER

- CALLBACK is a way to make sure certain code does not execute until other code has already finished execution

EXAMPLE
```javascript
function doHomework(subject, callback) {
  alert(`Starting my ${subject} homework.`);
  callback();
}

doHomework('math', function() {
  alert('Finished my homework');
});
```

OR

```javascript
function doHomework(subject, callback) {
  alert(`Starting my ${subject} homework.`);
  callback();
}
function alertFinished(){
  alert('Finished my homework');
}
doHomework('math', alertFinished);
```

Why Callback in API Request
-

- When you make requests to an API, you have to wait for the response before you can act on that response

```javascript
T.get('search/tweets', params, function(err, data, response) {
  if(!err){
    // This is where the magic will happen
  } else {
    console.log(err);
  }
})
```


TO LOOK UP
http://www.clearboth.org/43_the_principles_of_unobtrusive_javascript/
http://insanehong.kr/post/front-end-developer-interview-javascript/
https://joshua1988.github.io/web-development/javascript/javascript-interview-3questions/
https://medium.com/@jimkimau/%EC%9D%B4%EB%B2%88-%EA%B8%B0%EC%88%A0-%EB%A9%B4%EC%A0%91-%EC%A4%91-%EA%B8%B0%EC%96%B5%EB%82%98%EB%8A%94-%EC%A7%88%EB%AC%B8%EA%B3%BC-%EB%8B%B5%EB%B3%80%EB%93%A4-712daa9a2dc
https://velog.io/@tmmoond8/%ED%94%84%EB%A1%A0%ED%8A%B8%EC%97%94%EB%93%9C-%EA%B0%9C%EB%B0%9C%EC%9E%90-%EC%9D%B8%ED%84%B0%EB%B7%B0-%ED%9B%84%EA%B8%B0-%EB%A9%B4%EC%A0%91-%EC%A7%88%EB%AC%B8-%EC%A0%95%EB%A6%AC-%EC%9E%91%EC%84%B1-%EC%A4%91
https://velog.io/@honeysuckle/%EC%8B%A0%EC%9E%85-%ED%94%84%EB%A1%A0%ED%8A%B8%EC%97%94%EB%93%9C-%EB%A9%B4%EC%A0%91-%EC%A7%88%EB%AC%B8-%EB%AA%A8%EC%9D%8C


Reference
https://www.toptal.com/javascript/interview-questions
123 - Essential JS interview questions
https://github.com/ganqqwerty/123-Essential-JavaScript-Interview-Questions
Top 85 JS questions
https://www.guru99.com/javascript-interview-questions-answers.html
65 JS interview Questions
https://www.webcodegeeks.com/javascript/javascript-interview-questions-answers/
Tricky JS interview questions Asked by Google and Amazon
https://medium.com/coderbyte/a-tricky-javascript-interview-question-asked-by-google-and-amazon-48d212890703
5 more JS interview Questions
https://www.sitepoint.com/5-javascript-interview-exercises/
EDUCATIVE
What is hosting
What is closure
Expected Output

PROBLEM

What is the potential pitfall with using `typeof bar === "object"` to determine if `bar` is an object? How can this pitfall be avoided?

OUTPUT / EXPLANATION
Although `typeof bar === "object"` is a reliable way of checking if bar is an object, JS also consider `null` as an object
The problem can easily be avoided by also checking if `bar` is `null`
`console.log((bar !== null) && (typeof bar === "object"))` 


PROBLEM
var myObject = {
   foo: "bar",
   func: function() {
       var self = this;
       console.log("outer func:  this.foo = " + this.foo);
       console.log("outer func:  self.foo = " + self.foo);
       (function() {
           console.log("inner func:  this.foo = " + this.foo);
           console.log("inner func:  self.foo = " + self.foo);
       }());
   }
};
myObject.func();

OUTPUT / EXPLANATION
outer func:  this.foo = bar
outer func:  this.foo = bar
inner func:  this.foo = undefined
inner func:  this.foo = bar
In the outer function, both `this` and `self` refer to `myObject` and therefore both can properly reference and access `foo`
In the inner function, thought `this` no longer refers to `myObject`. As result, `this.foo` is undefined in the inner function, whereas the reference to the local variable `self` remains in scope and is accessible there


PROBLEM
What is the significant of, adn reason for, wrapping the entire content of a JS source file in a function block?

OUTPUT / EXPLANATION
This is common practice, employed by many popular JS libraries (jQuery, Node.js, etc.).
This technique creates a closure around the entire contents of the file which, creates a private namespace and thereby helps avoid potential name clashes between different JS modules and libraries


PROBLEM
What is the significance, and what are the benefits, of including `use strict` at the beginning of a JavaScript source file?

OUTPUT / EXPLANATION
`use strict` is a way to voluntarily enforce stricter parsing and error handling on JS code at runtime
Otherwise, code errors have been ignored or failed silently
Prevents accidental globals
eliminates `this` coercion : without strict mode, a reference to `this` value of null or undefined is automatically coerced to the global


PROBLEM
Consider the two functions below. Will they both return the same thing?
function foo1()
{
 return {
     bar: "hello"
 };
}

function foo2()
{
 return
 {
     bar: "hello"
 };
}

OUTPUT / EXPLANATION
foo1 returns:
Object {bar: "hello"}
foo2 returns:
undefined 
`foo2()` returns undefined without any error being thrown
Semicolons are technically optional in JS
As a result, when the line container the `return` statement (with nothing else on the line) is encountered in `foo2()`, a semicolon is automatically inserted immediately after the return statement
No ERROR because syntax is perfectly valid


PROBLEM
What is `NaN`? What is its type? How can you reliably test if a value is equal to `NaN`?

OUTPUT / EXPLANATION
`NaN` represents a value that is "not a number". 
CAUSED WHEN An operation could not be performed 
Non-numeric ( `"abc" / 4` )
Because result of the operation is non-numeric
Although `NaN` means “not a number”, its type is, believe it or not, `Number`
console.log(typeof NaN === "number");  // logs "true"
`NaN` compared to anything -- even itself -- is false
console.log(NaN === NaN);  // logs "false"
Reliable to test is built-in function : `Number.isNaN()`


PROBLEM
OUTPUT
console.log(0.1 + 0.2);
console.log(0.1 + 0.2 == 0.3);

OUTPUT / EXPLANATION
0.30000000000000004
false
CAN’T SURE; it might print out `0.3` and `true`
Number in JS are all treated with floating point precision; such that may not always yield the expected results
SOLUTION : `Number.EPSILON` ⇒ represents the different between 1 and the smallest floating point number greater than 1


PROBLEM
Discuss possible ways to write a function `isInteger(x)` that determines if `x` is an integer

OUTPUT / EXPLANATION
ECMAscript6 which introduces a new `Number.isInteger()` function
But, not in ECMAScript6 : integers only exist conceptually
Numeric values are always stored as floating point values
function isInteger(x) { console.log(Math.round(x)); }

isInteger(4.4);

OR

function isInteger(x) { return (typeof x === 'number') && (x % 1 === 0); }


PROBLEM
Is what order will the numbers 1 - 4 be logged to the console when the code below is executed? Why?

(function() {
   console.log(1);
   setTimeout(function(){console.log(2)}, 1000);
   setTimeout(function(){console.log(3)}, 0);
   console.log(4);
})();


OUTPUT / EXPLANATION
1
4
3
2

`1` and `4` are display first since they are logged by simple calls to `console.log()` without any delay
`2` is displayed after `3` because `2` is being logged after a delay of 1 second where `3` is being logged after a delay of 0


PROBLEM
Write a simple function that returns a boolean indicating whether or not a string is a palindrome


OUTPUT / EXPLANATION
function isPalindrome(str) {
   str = str.replace(/\W/g, '').toLowerCase();
   return (str == str.split('').reverse().join(''));
 }


PROBLEM
Write a `sum` method which will work properly when invoked using either syntax below

OUTPUT / EXPLANATION
function sum(x) {
   if (arguments.length == 2) {
     return arguments[0] + arguments[1];
   } else {
     return function(y) { return x + y; };
   }
 }
`else`, we assume it was called in the form `sum(2)(3)`, so we return an anonymous function that adds together the argument passed to `sum()` (in this case 2) and the argument passed to the anonymous function (in this case 3)

 


PROBLEM
What gets logged to the console when the user clicks on “Button 4” and why?
Provide one or more alternate implementation that will work as expected
for (var i = 0; i < 5; i++) {
   var btn = document.createElement('button');
   btn.appendChild(document.createTextNode('Button ' + i));
   btn.addEventListener('click', function(){ console.log(i); });
   document.body.appendChild(btn);
 }

OUTPUT / EXPLANATION
No matter what button the user clicks the Number `5` will always be logged to the console because at the point that `onclick` method is invoked, the `for` loop has already completed and the variable `i` already has a value of 5
The key to making this works is to capture the value of `i` at each pass through the `for` loop by passing it into a newly created
OPTION1
 for (var i = 0; i < 5; i++) {
   var btn = document.createElement('button');
   btn.appendChild(document.createTextNode('Button ' + i));
   btn.addEventListener('click', (function(i) {
     return function() { console.log(i); };
   })(i));
   document.body.appendChild(btn);
 }

OPTION2 (wrapping entire call to `btn.addEventListener` in the new anonymous function
for (var i = 0; i < 5; i++) {
   var btn = document.createElement('button');
   btn.appendChild(document.createTextNode('Button ' + i));
   (function (i) {
     btn.addEventListener('click', function() { console.log(i); });
   })(i);
   document.body.appendChild(btn);
 }

OPTION3 (ES6 -- use `let i` instead of `var i`:var is defined through the program
for (let i = 0; i < 5; i++) {
   var btn = document.createElement('button');
   btn.appendChild(document.createTextNode('Button ' + i));
   btn.addEventListener('click', function(){ console.log(i); });
   document.body.appendChild(btn);
 }


PROBLEM
Assuming `d` is an “empty” object in scope
var d = {};
What is accomplished using the following code?
[ 'zebra', 'horse' ].forEach(function(k) {
    d[k] = undefined;
});

OUTPUT / EXPLANATION
Any lookup performed on a JS object with an unset key evaluates to `undefined`. But running this code marks those properties as “own properties” of the object
This is a useful strategy for ensuring that an object has a given set of properties.
Passing this object to `Object.key` will return an array with those set keys as well (even if their values are `undefined`


PROBLEM
What will the code below output to the console and why?
var arr1 = "john".split('');
var arr2 = arr1.reverse();
var arr3 = "jones".split('');
arr2.push(arr3);
console.log("array 1: length=" + arr1.length + " last=" + arr1.slice(-1));
console.log("array 2: length=" + arr2.length + " last=" + arr2.slice(-1));


OUTPUT / EXPLANATION
"array 1: length=5 last=j,o,n,e,s"
"array 2: length=5 last=j,o,n,e,s"
Calling an array object’s `reverse()` method does not only return the array in reverse order, it also reverses the order of the array itself
The `reverse()` method returns a reference to the array itself (in this case, `arr1`). As a result, `arr2` is simply a referent to (rather than a copy of) `arr1`. Therefore, when anything is done to `arr2`, `arr1` will be affected as well since `arr1` and `arr2` are simply reference to the same object
Passing an array to the `push()` method of another array pushes that entire array as a single element onto the end of the array. As a result, the statement `arr2.push(arr3);` add `arr3` in its entirety


PROBLEM
What is Output
console.log(1 +  "2" + "2");
console.log(1 +  +"2" + "2");
console.log(1 +  -"1" + "2");
console.log(+"1" +  "1" + "2");
console.log( "A" - "B" + "2");
console.log( "A" - "B" + 2);


OUTPUT / EXPLANATION
"122"
"32"
"02"
"112"
"NaN2"
NaN

“122” == String Concatenation
“32” == First operation to be performed is `+"2"` (extra `+` before the first `"2"` is treated as a unary operator). Thus JS converts the type of "2" to numeric and then applies the unary `+` sign to it
“02” == … same as above
“112” == unary ⇒ then converts back to string when it is concatenated with the second `"1"` operand
“NaN2” == `-` operator can not be applied to strings, `"A" - "B"` yields `NaN` which is then concatenated ...


PROBLEM
The following recursive code will cause a stack overflow if the array list is too large. How can you fix this and still retain the recursive pattern?
var list = readHugeList();

var nextListItem = function() {
   var item = list.pop();

   if (item) {
       // process the list item...
       nextListItem();
   }
};


OUTPUT / EXPLANATION
The potential overflow can be avoided by modifying the `nextListItem` function as follows
var list = readHugeList();

var nextListItem = function() {
   var item = list.pop();

   if (item) {
       // process the list item...
       setTimeout( nextListItem, 0);
   }
};
The overflow is eliminated because the event loop handles the recursion, not the call stack.


PROBLEM
What is a “closure” in JS? 

OUTPUT / EXPLANATION

var globalVar = "xyz";

(function outerFunc(outerArg) {
   var outerVar = 'a';
  
   (function innerFunc(innerArg) {
   var innerVar = 'b';
  
   console.log(
       "outerArg = " + outerArg + "\n" +
       "innerArg = " + innerArg + "\n" +
       "outerVar = " + outerVar + "\n" +
       "innerVar = " + innerVar + "\n" +
       "globalVar = " + globalVar);
  
   })(456);
})(123);
A closure is an inner function that has access to the variables in the outer function’s scope chain. The closure has access to variables in three scopes
Variable in its own scope
Variables in enclosing functions’ scope
Global variables



PROBLEM
What will be the output of the following code
for (var i = 0; i < 5; i++) {
    setTimeout(function() { console.log(i); }, i * 1000 );
}

OUTPUT / EXPLANATION
Not 0, 1, 2, 3, and 4 as might be expected; rather, it will display 5, 5, 5, 5, and 5
The reason for this is that each function executed within the loop will be executed after the entire loop has completed and therefore reference the last value stored in `i` which was 5
CLOSURE can can used to prevent this problem
for (var i = 0; i < 5; i++) {
   (function(x) {
       setTimeout(function() { console.log(x); }, x * 1000 );
   })(i);
}
In ES2015, `let`
for (let i = 0; i < 5; i++) {
    setTimeout(function() { console.log(i); }, i * 1000 );
}


PROBLEM
What would the following lines of code output to the console?
console.log("0 || 1 = "+(0 || 1));
console.log("1 || 2 = "+(1 || 2));
console.log("0 && 1 = "+(0 && 1));
console.log("1 && 2 = "+(1 && 2));



OUTPUT / EXPLANATION
console.log("1 && 2 = "+(1 && 2)); ----> 2
It counts as “true” in logical expressions, but also can be used to return that value when you care to do so


PROBLEM
What will be the output when the following code is executed? Explain
console.log(false == '0')
console.log(false === '0')


OUTPUT / EXPLANATION
True
false
PROBLEM
What is the output
var a={},
   b={key:'b'},
   c={key:'c'};

a[b]=123;
a[c]=456;

console.log(a[b]);

OUTPUT / EXPLANATION
Ouput of this code `456`
The reason; when setting an object property, JS will implicitly stringify the parameter value
In this case, since `b` and `c` are both objects, they will both be converted to “[object Object”]
Therefore, setting or referencing `a[c]` is precisely the same as setting or referencing `a[b]`


PROBLEM
What is the code output
console.log((function f(n){return ((n > 1) ? n * f(n-1) : n)})(10));

OUTPUT / EXPLANATION
10!
PROBLEM
What is the code output and why
(function(x) {
   return (function(y) {
       console.log(x);
   })(2)
})(1);

OUTPUT / EXPLANATION
The output is 1 
even though the value of x is never set in the inner function
Since x is not defined in the inner function, the scope of the outer function is searched for a defined varialbe x, which is found to have a value of 1 
CLOSURE - a closure is implemented as a “inner function” 
An important feature of closures is that an inner function still has access to the outer function’s variables


PROBLEM
What is the code output and why
var hero = {
   _name: 'John Doe',
   getSecretIdentity: function (){
       return this._name;
   }
};

var stoleSecretIdentity = hero.getSecretIdentity;

console.log(stoleSecretIdentity());
console.log(hero.getSecretIdentity());


OUTPUT / EXPLANATION
Undefined
John Doe
→ is being invoked in the global context FIXATION (var stoleSecretIdentity = hero.getSecretIdentity.bind(hero);)


PROBLEM
What is the code output and why

var length = 10;
function fn() {
    console.log(this.length);
}

var obj = {
 length: 5,
 method: function(fn) {
   fn();
   arguments[0]();
 }
};

obj.method(fn, 1);


OUTPUT / EXPLANATION
10
2 → arguments[0]() is nothing but calling fn(). Inside fn now, the scope of this function becomes the arguments array, and logging the arguments[] will return 2



PROBLEM
(function () {
   try {
       throw new Error();
   } catch (x) {
       var x = 1, y = 2;
       console.log(x);
   }
   console.log(x);
   console.log(y);
})();


OUTPUT / EXPLANATION
Var statements are hoisted to the top of the global or function scope it belongs to, even when it’s inside a with or catch block.
But the error’s identifier is only visible inside the catch block



CLONE
var obj = {a: 1 ,b: 2}
var objclone = Object.assign({},obj);



Theory
Event Delegation
이벤트 위임은 이벤트 리스너를 하위 요소에 추가하는 대신 상위 요소에 추가
리스너는 DOM의 bubbling된 이벤트로 인해 하위 요소에서 이벤트가 발생될 때 마다 실행됩니다.
장점
각 하위 항목에 이벤트 핸드폰를 연결하지 않고 상위 요소에 하나의 단일 핸들러만 필요하기 때문에 메모리 사용 공간이 줄어든다
제거된 요소에서 핸들러를 제거하고 새 요소에 대해 이벤트를 바인딩할 필요가 없다



This 가 JS에서 어떻게 작동하지는지 설명하세요
this의 값은 함수의 호출되는 방식에 따라 달라진다
함수를 호출할 때 new 키워드를 사용하는 경우 함수 내부에 있는 this는 완전히 새로운 객체
Apply, call, bind가 함수의 호출 / 작성에 사용되는 경우 함수 내의 this는 인수로 전달된 객체
Obj.method() 와 같이 함수를 메서드로 호출하는 경우 this는 함수가 프로퍼티인 객체
함수가 자유함수로 호출되는 경우 … this는 전역 객체. 브라우저에서는 window 객체입니다. 엄격모드 ‘use strict’일 경우 this는 전역 객체 대신 undefined가 됩니다
함수가 ES15 화살표 함수인 경우 위의 모든 규칙을 무시하고 생성된 시점에서 주변 스코프의 this 값을 받습니다


프로토타입 상속이 어떻게 작동하는지 설명하세요
모든 JS 객체는 다른 객체에 대한 참조인 prototype 프로퍼티를 가지고 있습니다.
해당 객체에 해당 프로퍼티가 없으면 JS 엔진은 프로퍼티가 정의 될 때까지 찾고 만약 객체의 프로퍼티에 접근할 때 해당 객체에 해당 프로퍼티가 없으면 프로토타입 체인 중 하나에 있거나 프로토타입 체인의 끝에 도달할 때까지 찾습니다.


AMD 대 CommonJS
CommonJs는 동기식 // 서버-사이드 개발
AMD는 비동기식 // 모듈의 비동기 로딩 (브라우저용)


IIFE -- Why function foo(){{(); is not working
Uncaught SyntaxError : Unexpected token
SOLUTION : (function foo(){{)() ⇒ 전자는 함수 선언이며 후자는 함수를 호출하려고 시도했지만 이름이 지정되지 않았기 때문에 ERROR
JS parser는 function foo() {} (); 를 function foo () {} 와 (); 로 읽는다


Null, undefined, 선언되지 않는 변수의 차이점
선언되지 않는 변수는 현재 범위 외부에서 전역으로 정의
strict모드에서는 선언되지 않은 변수에 할당하려고 할 때 ReferenceError
function foo() {
  x = 1; // strict 모드에서 ReferenceError를 발생시킵니다.
}

foo();
console.log(x); // 1


클로저는 무엇이며, 어떻게 / 왜 사용합니까?
데이터 프라이버시 / 클로저로 private method 모방 (모듈 패턴)
Closures are created every time a function is created, at function creation time
To use a closure, simply define a function inside another function and expose it

Is JS multithreaded
No JS is not multi-threaded. It is event driven and your assumption of the events firing sequentially is what you will see


.forEach() 루프와 .map() 루프 사이의 주요 차이점
forEach
배열의 요소를 반복
콜백 실행
값을 반환하지 않는다
const a = [1, 2, 3];
const doubled = a.forEach((num, index) => {
  // num 및 / 또는 index로 무엇이든 해보세요.
});

// doubled = undefined

Map
배열을 요소를 반복
각 요소에서 함수를 호출하여 결과를 새 배열을 작성하여 각 요소를 새 요소에 매핑
새로운 배열 반환 (결과가 필요하지만 원본 배열을 변경하고 싶지 않으면 .map()
const a = [1, 2, 3];
const doubled = a.map(num => {
  return num * 2;
});

// doubled = [2, 4, 6]


익명 함수
익명 함수는 IIFE로 사용되어 지역 범위 내에서 일부 코드를 캡슐화하므로 선언된 변수가 전역 범위로 누출되지 않는다
한 번 사용되며 다른 곳에서는 사용할 필요가 없는 콜백으로 사용

호스트 객체와 내장 객체의 차이점
내장 객체는
ESMAScript 사양에 정의된 JS 언어의 일부인 객체 (String, math, RegeXP, Object, Function)


person(){}, var person = Person(), var person = new Person()의 차이점은 무엇
질문의 의도는 JS 생성자에 대해 묻는 것
Var person = Person()은 생성자가 아니며 Person을 함수로 호출
Var person = new Person()은 Person.prototype을 상속받은 new 연산자를 사용하여 Person 객체의 인스턴스를 생성


.call 과 .apply의 차이점
.call 과 .apply는 모두 함수를 호출하는데 사용되며 첫 번째 매개 변수는 함수 내에서 this 값으로 사용됩니다. 
.call은 쉼표 뒤 인수를 취하고
.apply는 배열!
function add(a, b) {
  return a + b;
}

console.log(add.call(null, 1, 2)); // 3
console.log(add.apply(null, [1, 2])); // 3
Function.prototype.bind에 대해 설명하세요
bind는 메소드 호출될 때 this 키워드가 제공된 값으로 설정되고 ...	


언제 document.write()를 사용합니까?
document.write()가 실행되면 document.open을 호출하여 문서 전체를 지우고 (<head>와 <body>를 지웁니다!).
문자열로 주어진 매개 변수 값으로 대체합니다
그러므로 일반적으로 위험하고 오용…!!!


Feature detection, feature inference, UA String의 차이점은 무엇입니까?
Feature Detection
브라우저가 특정 코드 블록을 지원하는지에 따라 다른 코드를 실행하도록 하여, 일부 브라우저에서 항상 오류가 발생하도록 합니다
if ('geolocation' in navigator) {
  // navigator.geolocation를 사용할 수 있습니다
} else {
  // 부족한 기능 핸들링
}
modernizr는 feature detection을 처리 할 수 있는 훌륭한 라이브러리입니다
Feature inference
권장하지 않습니다
Feature detection이 더 확실합니다
UA String
권장하지 않습니다 security risk


Ajax에 대해 설명하세요
Ajax는 비동기 웹 응용 프로그램을 만들기 위해 클라이언트 측에서 여러 웹 기술을 사용하는 웹 개발 기술의 집합입니다. Ajax를 사용하면 웹 애플리케이션은 기존 페이지의 화면 및 동작을 방해하지 않으면서 백그라운드에서 비동기적으로 서버로 데이터를 보내고 서버에서 데이터를 받아올 수 있습니다
Ajax는 프리젠테이션 레이어에서 데이터 교환 레이어를 분리함으로써 웹 페이지 및 확장 웹 애플리케이션이 전체 페이지를 다시 로드 할 필요 없이 동적으로 컨텐츠를 변경할 수 있도록 합니다
최근에는 일반적으로 네이티브 JS의 장점 때문에 XML 대신 JSON을 사용합니다.
Ajax의 장단점
장점
상호작용성이 좋아진다
스크립트 및 스타일 시트는 한 번만 요청하면 되므로 서버에 대한 연결을 줄여준다
SPA의 대부분의 장점
단점
브라우저에서 JS가 비활성화된 경우 작동하지 않는다
SPA의 대부분의 단점


JSONP의 작동 방식 (ajax가 아닌 방법)
JSONP (JSON with padding)은 현재 페이지에서 cross-origin 도메인으로의 ajax 요청이 허용되지 않기 때문에 웹 브라우저에서 cross-domain 정책을 우회하는 데 일반적으로 사용되는 방법
JSONP는 <script> 태그를 통해 cross-origin 도메인에 요청하고 보통 callback 쿼리 매개 변수 (예 : http://example.com?callback=printData)로 요청합니다. 그러면 서버는 printData라는 함수 안에 데이터를 래핑하여 클라이언트로 반환
<!-- https://mydomain.com -->
<script>
function printData(data) {
  console.log(`My name is ${data.name}!`);
}
</script>

<script src="https://example.com?callback=printData"></script>
JSON는 안전하지 않을 수 있으며 보안... 
JSONP 데이터 공급자를 신뢰해야해야만 합니다
CORS가 권장되는 접근 방식이며 JSONP는 해킹으로 간주


호이스팅에 대해 설명하세요
호이스팅은 코드에서 변수 선언의 동작을 설명하는데 사용하는 용어
Var 키워드로 선언 혹은 초기화된 변수는 현재 스코프의 최상위까지 호이스팅 됩니다,
그러나 선언문만 호이스팅 되며 할당은 그대로입니다
// var 선언이 호이스팅됩니다
console.log(foo); // undefined
var foo = 1;
console.log(foo); // 1

// let / const 선언은 호이스팅되지 않습니다.
console.log(bar); // ReferenceError: bar는 정의되지 않았습니다
let bar = 2;
console.log(bar); // 2
함수 선언은 바디를 호이스팅 BUT 변수 선언 형태로 작성된 함수 표현식은 변수 선언만 호이스팅
// 함수 선언
console.log(foo); // [Function: foo]
foo(); // 'FOOOOO'
function foo() {
  console.log('FOOOOO');
}
console.log(foo); // [Function: foo]

// 함수 표현식
console.log(bar); // undefined
bar(); // Uncaught TypeError: bar는 함수가 아닙니다
var bar = function() {
  console.log('BARRRR');
};
console.log(bar); // [Function: bar]


Event bubbling에 대해 설명하세요
DOM요소에서 이벤트가 트리거되면 리스너가 연결되어 있는 경우 이벤트 처리를 시도한 다음 해당 이벤트가 부모에게 bubbling되고 같은 이벤트가 발생
bubbling은 요소의 조상 document 까지 계속적으로 발생시킵니다


“attribute”와 “property”의 차이점
Attribute 속성은 HTML 마크업에 정의되지만 property 속성은 DOM에 정의
<input type="text" value="Hello">
const input = document.querySelector('input');
console.log(input.getAttribute('value')); // Hello
console.log(input.value); // Hello
//텍스트 필드에 “World!”를 추가하면
console.log(input.getAttribute('value')); // Hello
console.log(input.value); // Hello World!


Document load 이벤트와 document DOMContentLoaded 이벤트의 차이점은 무엇인가?
DOMContentLoaded 이벤트는 스타일시트, 이미지, 서브프레임이 로딩을 기다리지 않고 초기 HTML 문서가 완전히 로드되고 파싱될 때 발생
window의 load 이벤트는 DOM과 모든 종속 리소스와 에셋들이 로드된 후에 만 발생합니다


== 와 ===의 차이점
== 는 추상 동등 연산자 (automatically 타입 변환을 해준다)
===는 완전 동등 연산자 (type checking)


JS와 관련하여 same-origin 정책을 설명하세요
Same-origin 정책은 JS가 도메인 경계를 넘어서 요청하는 것을 방지
이 정책은 한 페이지의 악의적인 스크립트가 해당 페이지의 문서 객체 모델을 통해 다른 웹 페이지의 중요한 데이터에 접근하는 것을 방지


“Use strict”;이 무엇입니까? 사용시 장단점
‘Use strict’는 전체 스크립트나 개별 함수에 엄격 모드를 사용하는데 사용되는 명령문
장점
실수로 전역변수를 만드는 것이 불가능
함수의 매개변수 이름은 고유해야한다
this는 전역 컨텍스트에서 undefined
단점
서로 다른 엄격한 모드로 작성된 스크립트를 병합하면 문제를 발생


일반적으로 웹 사이트의 전역 스코프를 그대로 두고 건드리지 않는 것이 좋은 이유는 무엇입니까?
모든 스크립트는 전역 스코프에 접근할 수 있으며 모든 사람이 전역 네임스페이스를 사용하여 변수를 정의하면 충돌
모듈 패턴 (IIFEs)을 사용하여 변수를 로컬 네임스페이스 내에 캡슐화하십시오


왜  load 이벤트를 사용하는가? 단점? 다른 대안

Load 이벤트는 문서로딩 프로세스가 끝날 때 발생
이 시점에서 문서의 모든 객체가 DOM에 있고, 모든 이미지, 스크립트, 링크 및 하위 프레임로딩이 완료
DOM 이벤트 DomcontentLoaded는 페이지의 DOM이 생성된 후에 발생하지만 다른 리소스가 로딩되기를 기다리지 않습니다


SPA이 무엇인지 설명하고 SEO-friendly 한 앱을 만드는 방법
현대 SPA에서는 대신 클라이언트 측 렌더링이 사용된다
브라우저는 전체 애플리케이션에 필요한 스크립트(프레임워크, 라이브러리, 앱 코드) 및 스타일시트와 함께 서버의 초기 페이지를 로드.
사용자가 다른 페이지로 이동하면 페이지 새로고침이 발생하지 않습니다
페이지의 URL은 HTML5 History API를 통해 업데이트 됩니다
일반적으로 JSON 형식의 새 페이지에 필요한 새 데이터는 브라우저에서 AJAX 요청을 통해 서버로 전송
장점
전체 페이지 새로고침으로 인해 페이지 탐색 사이에 하얀 화면이 보이지 않아 앱이 더 반응적으로 느껴진다
동일한 애셋을 페이지 로드마다 다시 다운로드할 필요가 없으므로 서버에 대한 HTTP 요청이 줄어든다
단점
여러 페이지에 필요한 프레임워크, 앱 코드, 애셋로드로 인해 초기 페이지로드가 무거워진다
모든 검색 엔진이 크롤링 중에 JS를 실행하지는 않으며 페이지에 빈 콘텐츠가 표시 될 수 있다. (SEO) 가 어려워진다
Prerender / code splitting


Promises 와 또는 polyfill
PROMISES
어느 시점에 resolve된 값 또는 resolve되지 않은 이유(예 : 네트워크 오류) 중 하나의 값을 생성할 수 있는 객체
Fulfilled, rejected, pending (3가지 상태 중 하나)
Promise 사용자는 콜백을 붙여서 fulfill된 값이나 reject된 이유를 처리할 수 있다


Callback 대신 promise를 사용할 때의 장 / 단점
장점
이해하기 어려운 콜백 지옥을 피할 수 있다
읽기 쉬운 .then()을 이용하여 연속적인 비동기 코드를 쉽게 작성
promise.all()을 사용하여 병렬 비동기 코드를 쉽게 작성
단점
ES2015를 지원하지 않는 이전 브라우저에서 이를 사용하기 위해서는 polyfill을 로드애햐 한다


JS로 컴파일되는 언어로 JS 코드를 작성하는 경우의 장 / 단점
Coffeescript, elm, clojurescript, purescript 및 typescript
장점
JS의 오랜 문제점들을 수정 / 안티 - 패턴을 방지
syntatic sugar를 제공함으로써 더 짧은 코드 작성
정적 타입은 시간 경과에 따라 유지 관리해야 하는 대규모 프로젝트에 대해 훌륭하다 (TypeScript)
단점
브라우저는 오직 JS만 실행하기 때문에 빌드 / 컴파일 프로세스가 필요하며 브라우저에 제공되기 전 JS로 코드를 컴파일해야 한다
JS 표준보다 뒤처진다

ddddd


JS 코드를 디버깅하기 위해 어떤 도구와 기술을 사용하십니까?
React and Redux
React Devtools
Redux Devtools
JS
Chrome Devtools
Debugger statement
Console.log debugging


오브젝트 속성 및 배열 항목을 반복할 때 사용하는 언어 구조
오브젝트
for 반복문 - for (var property in obj) { console.log(property); }. 그러나 이것은 상속된 속성도 반복되며, 사용하기 전에 obj.hasOwnProperty(property) 체크를 추가해야 합니다.
Object.keys() - Object.keys(obj).forEach(function (property) { ... }). Object.keys ()는 전달하는 객체의 열거 가능한 모든 속성을 나열하는 정적 메서드입니다.
Object.getOwnPropertyNames() - Object.getOwnPropertyNames(obj).forEach(function (property) { ... }). Object.getOwnPropertyNames()는 전달하는 객체의 열거 가능한 속성과 열거되지 않는 모든 속성을 나열하는 정적 메서드입니다.
배열
for 반복문 - for (var i = 0; i < arr.length; i++). 여기에 있는 일반적인 함정은 'var'이 함수 범위에 있고 블록 범위가 아니며 대부분 블록 범위의 반복자 변수를 원할 것이라는 점입니다. ES2015에는 블록 범위가 있는 let을 도입하고 대신 사용할 것을 권장합니다. 그래서 다음과 같이 됩니다. for (let i = 0; i < arr.length; i++).
forEach - arr.forEach(function (el, index) { ... }). 필요한 모든 것이 배열 요소라면 index를 사용할 필요가 없기 때문에 이 구조가 더 편리 할 수 ​​있습니다. 또한 every과 some 메서드를 이용하여 반복을 일찍 끝낼 수 있습니다.

For 루프는 break를 사용하여 루프를 조기 종료 또는 루프 당 두번 이상 반복자를 증가시키는 것과 같이 더 많은 유연성을 허용

Refererence : https://github.com/yangshun/front-end-interview-handbook/blob/master/Translations/Korean/questions/javascript-questions.md#%EC%9D%B4%EB%B2%A4%ED%8A%B8-%EC%9C%84%EC%9E%84%EC%97%90-%EB%8C%80%ED%95%B4-%EC%84%A4%EB%AA%85%ED%95%98%EC%84%B8%EC%9A%94
