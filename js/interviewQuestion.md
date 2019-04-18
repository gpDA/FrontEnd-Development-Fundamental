# interviewQuestion



# TODO:
### 1. `typeof` vs. `instanceof`

##### simple comparison
1. use `instanceof` of custom type
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

2. use `typeof` for simple built in types
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

BONUS - 
```javascript
typeof null; // object
```

##### what is `typeof`
`typeof` is a unary operator that returns a string indicating the type of the unevaluated operand
```javascript
const a = "I'm a string primitive";
const b = new String("I'm a String Object");

typeof a; --> returns 'string'
typeof b; --> returns 'object'
```

##### what is `instanceof`
`instanceof` is a binary operator, accepting an object and a constructor.
It return a boolean indicating whether or not the object has the given constructor in its prototype chain
```javascript
const a = "I'm a string primitive";
const b = new String("I'm a String Object");

a instanceof String; --> returns false
b instanceof String; --> returns true
```

##### how to properly test for strings?
need to detect both primitive strings and Strings objects as values of the object
```javascript
const isString = (str) => {
  return typeof str === 'string' || str instanceof String
}
```



# TODO:
### 2. forEach vs for loop vs for ... in

##### simple comparison
1. Performance : `forEach` is little slower than `for loop`
2. Usability : There is no way we can break / return from the callback `forEach`
3. Browser compatibility : `forEach` is not supported in IE < 9

##### what is forEach
1. `forEach` is a method on the `Array` prototype. It iterates through each element of an array and passes it to a callback function
2. `forEach` can only be used on Arrays, Maps, and Sets

##### what is for loop
1. `for` statment does not necessarily involve an array

##### what is for ... in
1. `for ... in` is used to iterate over the enumerable properties of objects.
    (every property in an object will ahve an `Enumerable` value
        - if that value is set to `true`, then the property is `Enumerable`)

OBJECTS - 
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
ARRAYS - 
DON'T FORGET, ARRAYS are objects too (which means we can also use `for ... in` loop on Arrays)
```javascript
const arr = ['cat', 'dog', 'fish'];
for (let i in arr) {  
  console.log(arr[i])
}
// cat
// dog
// fish
```

STRING -
Since each character in a stirng has an index, we can even use `for ... in` on strings
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

MAP - (ES6 in 2015)
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



# TODO:
### 3. What is promise in javascript

##### what is promise
`Promise` object represents the eventual completion (or failure) xof an asynchronous operation, and its resulting value

**
WRITING ASYNC CODE IN PLAIN CALLBACK SYTLE IS HARD TO DO CORRECTLY
it is easy to froget to check for an error or to allow an unexpected exception
**

##### why use promise 1. prevent callback hell 2. prevent limitation of try ... catch

1. 콜백 헬
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

2. limitation of try catch

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
문제점 2가지
==> 1. 작동을 하지 않는다. try ... catch 블록은 readSketchyFile 함수 안에 있지만, 정작 예외는 fs.readFile이 콜백으로 호출하는 익명 함수 안에서 일어났다
==> 2. 콜백이 우연히 두 번 호출되거나 / 아예 호출되지 않는 경우르 방지하는 장치가 없다. (js는 콜백이 정확히 한 번만 호출될 것을 보장하지 않는다)

##### Promise with Callback

프미스가 콜백을 대체하는 것은 아니다. 사실 프로미스에서도 콜백을 사용한다.
프로미스는 콜백을 예층 가능한 패턴으로 사용할 수 있게 하며, 프로미스 없이 콜백만 사용했을 때 나타날 수 있는 엉뚱한 현상 / 버그를 해결
프로미스는 객체이므로 어디든 전달 할 수 있다 (콜백에 비해 간편한 장점)

##### Promise Chain

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
==> 프로미스 체인을 사용하면 모든 단계에서 에러를 캐치할 필요가 없다.
체인 어디에서든 에러가 생기면 체인 전체가 멈추고 catch 핸들러가 동작

##### 결정되지 않는 프라미스 방지하기
프로미스는 비동기적 코드를 단순화하고 / 콜백이 두 번 이상 실행되는 문제를 방지
하지만 resolve나 reject를 호출하는 걸 잊어서 프로미스가 결정되지 않는 문제까지 자동으로 해결하지 못한다.
해결 방법!! 프로미스에 타임아웃을 건다.

프로미스에 타임아웃을 건다
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

호출
```javascript
c.go()
  .then(addtIMEOUT(LAUNCH, 11*1000));
```


##### SOLUTION WITH PROMISE
`Promise` wraps an asynchronous action in an object,
which can be passed around and told to do certain things when the action finishes or fails

CHECKOUT `http://www.promisejs.org/`

Parmeters `executor`- 
`executor` is a function that is passed with the arguments resolve and reject.
The `executor` function is executed immediately by the Promise implmentation, passing `resolve` and `reject` functions

`Promise` is a proxy for a value not necessarily known when the promise is created. It allows you to associate handlers with an asynchronous action's eventual success value or failure reason. 
This lets asynchronous methos return values like synchronous methods : instead of immediately returning the final value, the asynchronous method returns a promise to supply the value at some poin in the future.

`Promise` is in one of these states (ONLY ONCE ... (settled) FULFILLED PROMISE cannot become REJECTED PROMISE)
1. pending : initial state; (neither fulfilled nor rejected)
2. fulfilled : opeartion completed successfully
3. rejected : operation failed

==> A pending promise can either be `fulfilled` with a value, or `rejected` with a reason (error). When either of these options happens, the associated handlers queued up by a promise's then method are called. 

**
As the `Promise.prototype.then()` and `Promise.prototype.catch()` methods reutnr promises, they can be chained
**

METHODS -
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

CREATING A PROMISE
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

LOADING AN IMAGE WITH XHR -
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

LOADING until loads
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



# TODO:
### 4. What is JavaScript Object Properties

##### what is Properties
`properties` are the values associated with a JavaScript object

ACCESSING JS Properties
```javascript
objectName.property         // person.age
objectName["property"]      // person["age"]
```

ADDING New Properties
```javascript
person.nationality = "English";
```

DELETING properties
```javascript
var person = {firstName:"John", lastName:"Doe", age:50, eyeColor:"blue"};
delete person.age;   // or delete person["age"]; 
```



# TODO:
### 5. JavaScript call vs. apply vs. bind vs. reduce
CALL - 
with the `call()` method, you cna write a



# TODO:
### 6. What is callback
-
"CALL US BACK"

##### what is callback
simple: 
A callback is a function that is to be executed after another function has finished executing

more complexly:
In JavaScript, functions are objects.
Because of this, functions can take functions as arugments, and can be returned by other function (HIGHER-ORDER FUNCTIONS)
ANY FUNCTION that is passed as an arugment is CALLBACK FUNCTION

##### why need callback
JS is an event driven language
==> instead of waiting for a response before moving on, JS will keep executing
while listening for other events

what if the function sheduled to be executed before cannot be executed immediately?
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
==> It is not that JS did not execute our functions in the order we wanted it to,
it is instead that
JS did not wait for a reponse from `first()` before movingon to execute `second()`
**
YOU CANNOT JUST CALL ONE FUNCTION AFTER ANOTHER AND HOPE THEY EXECUTE IN THE RIGHT ORDER
**
CALLBACK is a way to make sure certain code does not execute until other code has already finished execution

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

WHY CALLBACK in API Request
--
When you make requests to an API, you have to wait for the response before you can act on that response

```javascript
T.get('search/tweets', params, function(err, data, response) {
  if(!err){
    // This is where the magic will happen
  } else {
    console.log(err);
  }
})
```