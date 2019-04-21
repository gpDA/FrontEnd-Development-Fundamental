# BACKGROUND INFO



# TODO:
#### HTTP status

EXAMPLE - 
`HTTP/1.1 200 OK`

STATUS CODE
2 --> success
4 --> something wrong (404 NOT FOUND)
5 --> error happens on the server



# TODO:
#### HTTP response / request header

> Content-Length: 65585
> Content-Type: text/html
> Last-Modified: Wed, 09 Apr 2014 10:48:09 GMT

==> HTML document of 65,585 bytes (size & type & when was last modified)

For the most part, a client / server decides which headers to include in a rquest / response

** 
The browser will automatically add some request headers, such as "Host"
and those needed for the server to figure out the size of the body
BUT you can add your own header witt the `setRequestHeader` method
**

REQUIRED
- HOST header (hostname)
    should be included in a request because a server might be serving multiple hostnames on a single IP address
    and without that header, the server won't know which host the client is trying to talk to

After the headers, both requests and response may include a blank line followed by a `body`
`body` contains the data being sent.
`GET` and `DELETE` requests do not send along any data
`PUT` and `POST` requests do



# TODO:
#### form submit via JS
```html
<form action="example/message.html">
...
</form>
```

`GET /example/message.html?name=Jean&message=Yes%3F HTTP/1.1`

WHEN you click the SEND button, the information in those field will be encoded into a `query string`

`GET` METHOD : adding the parameters in the uRL

`POST` METHOD : put query string in body of the request, rather than adding it to the URL

> POST /example/message.html HTTP/1.1
> Content-length: 24
> Content-type: application/x-www-form-urlencoded
>
>name=Jean&mesasge=Yes%3F



# TODO:
#### XMLHttpRequest

XMLHttpRequest
-
BENEFIT : 
WHEN the XMLHttpRequest interface was added to Internet Explorer, it allowed people to do things with JS
that had been very hard before

E.G.) websites started showing lists of suggestions when the user was typing something into a text field.
The script would send the text to the server over HTTP as the user typed
The server, which had some database possible inputs, would match the database entries against the partial input
and send back possible completions to show the user
`specacular` -- because people were used to waiting for a full page reload for every interaction with a website

SENDING A REQUEST `open` && `send`
-
```javascript
var req = new XMLHttpRequest();
// if 3rd argument to open was false,
// send will return only after the response to our request was received
req.open("GET", "example/data.txt", false);
// After opening we can send it with the send method.
// The argument to send is the request body
// for GET method we can pass null
req.send(null);
console.log(req.status, req.statusText);
// 200 OK
```



# TODO:
#### Asynchronous Requests
In Synchronous request,
1. program suspended while browser <--> server communicating 
    (when connection is bad / server is slow / file is big ... it might take quite a while)
2. no event handlers can fire while our program is suspended, the whole document will become unresponsive

```javascript
// asynchronous
// means that when we call `send`, the only thing that happens right away is that
// the request is scheduled to be sent
req.open("GET", "example/data.txt", true);
// but as long as the request is running, we won't be able to access the response.
// we need a mechanism that will notify us when the data is available.
// for this, we must listen for the `load` event on the request object
req.addEventListener("load", function(){
    console.log("Done", req.status);
});
req.send(null);
// it is often a better idea to communicate using JSON data,
// which is easier to read and write, both for programs and for humans
console.log(JSON.parse(req.responseText));
```



# TODO:
#### HTTP sandboxing
-
SECURITY
Browsers disallow scripts to make HTTP requests to other domains (themafia.org / mybank.com)

To build systems that want to access several domains for legitimaste reasons
`Access-Control-Allow-Orign: *`



# TODO:
#### RPC (remote procedure call)
What is RPC
-
RPC is a protocl that one program can use to request a service from a program located in another computer on a network
without having to understand the network's details (function call) OR (subroutine call)

* RPC uses the client-server model
* RPC is a synchronous operation requiring the requesting program to be suspended until the result of the remote procedure are returned
* The use of lightweight processes or threads that share the same address space allows multiple RPCs to be performed concurrently


Procedure
-

Client(CALLER)                               Server(CALLEE)


                                            waiting for request
             Request meesage w/(parameter)
              ---------------------->

                                            receive request and start procedure execution

waiting for a REPLY

                                            send reply


              Reply message (contains full of procedure execution)
              <------------------------

resume execution

                                            waiting for next request


WHEN thinking in terms of RPC, HTTP is just a vehicle for communication,
and you will most likely write an abstraction layer that hides it entirely

Another approach (build communication around the concept of resources and HTTP methods)
-

Instead of a remote procedure called `addUser`, use PUT request to `/users/larry`
==> Instead of encoding that user's properties in function arguments, you define a document format or
use an existing format that represents a user

EVENTUALLY,
A resource is fetched by making a `GET` request to source's URL (e.g. `/user/larry`),
which returns the document representing the resource

BENEFIT
-
This approach makes it easier to use some of the features that HTTP provides, such as support for caching resource
(keeping a copy on the client side)



# TODO:
## 비동기적 프로그래밍
사용자의 행동은 전적으로 비동기적이다. (사용자가 언제 클릭할지 / 터치할지 / 전혀 모른다)

Javascript is single thread ==> 비동기적으로 생각해야한다
BENEFIT:
    멀티스레드 프로그래밍에서 겪어야 하는 정말 골치 아픈 문제 신경을 안 써도 된다

JS의 비동기적 프로그래밍
1. callback
2. promise
3. generator

=> 제너레이터 자체는 비동기적 프로그래밍을 전혀 지원하지 않습니다.
제너레이터를 비동기적으로 사용하려면 `promise` OR 특수한 `callback`과 함께 사용해야 합니다 (promise 역시 콜백에 의존)

사용자 입력 외에, 비동기적 테크닉을 사용해야 하는 경우
- ajax 호출을 비롯한 네트워크 요청
- 파일을 읽고 쓰는 등의 파일시스템 작업
- 의도적으로 시간 지연을 사용하는 기능(알람)



# TODO:
#### setInterval 과 clearInterval
setInterval vs. clearinterval
_
setTimeout은 콜백 함수를 한 번만 실행하고 멈추지만,
setInterval은 콜백을 정해진 주기마다 호출하며 clearinterval을 사용할 때까지 멈추지 않습니다.

다음은 10회째가 될 때까지 5초마다 콜백을 실행한다
```javascript
const start = new Date();
let i = 0;
const intervalId = setInterval(function(){
    let now = new Date();
    if(now.getMinutes() !== start.getMinutes() || ++i > 10)
        return clearinterval(intervalId);
    console.log(`${i}: ${now}`)
}, 5*1000)
```
==> setInterval이 ID를 반환한다. 이 ID를 써서 실행을 멈출 수 있다



# TODO:
#### 스코프와 비동기적 실행

함수를 호출하면 항상 클로저가 만들어진다. (매개변수를 포함해 함수 안에 만든 변수는 모두 무언가가 자신에 접근할 수 있는 한 계속 존재)

COUNTDOWN을 순수 callback을 이용해서 만들자
```javascript
// let을 for 루프 밖에서 선언할시 / for loop이 끝나고 i의 값이 -1이 되고 나서야 콜백이 실행된다
function countdown(){
    for(let i=5; i>=0; i--){
        setTimeout(function(){
            console.log(i==0? 'GO': i)
        }, (5-i)*1000);
    }
}
countdown();
```

COUNTDOWN을 promise을 이용해서 만들자
```javascript
function countdown(seconds){
    return new Promise(function(resolve, reject){
        for(let i =seconds; i>=0; i--){
            setTimeout(function(){
                if(i>0) console.log(i + '...');
                else: resolve(console.log('GO!'));
            }, (seconds-i)*1000);
        }
    })
}
```
== not perfect : 너무 장황하고 / 웹 페이지에서 카운트다운이 끝나면 페이지 요소롤 업데이트하는 목적에 쓰기도 별로 알맞지 않다

사용법
```javascript
countdown(5).then(
    function(){
        console.log("countdown completed successfully");
    },
    function(err){
        console.log("countdown experienced an error: "+ err.message);
    }
)
```
==> then 핸들러는 성공 / 에럴 콜백을 받습니다

OR

```javascript
const p = countdown(5);
p.then(function(){
    console.log("countdown completed successfully");
});
p.catch(function(err){
    console.log("countdown experienced an error: "+ err.message);
})
```
==> 프로미스는 catch 핸들러도 지원하므로 핸들러를 둘로 나누어 써도 된다

**
프로미스는 비동기적 작업이 성공 / 실패 하도록 확정한는, 매우 안전하고 잘 정의된 메커니즘을 제공하지만
현재 진행 상황을 전혀 알려주지 않습니다
즉, 프로미스는 완료되거나 / 파기될 뿐, '50% 진행됐다' 라는 개념은 아예 없다 Q 프로미스 라이브러리 (https://github.com/kriskowal/q)
**

프로미스를 사용할 시 reject가 나오면 프로미스를 파기하게 하는 countdown code
```javascript
const EventEmitter = require('events').EventEmitter;

class Countdown extends EventEmitter{
    constructor(seconds, superstitious){
        super();
        this.seconds = seconds;
        this.superstitious = !!superstitious;
    }
    go(){
        const countdown = this;
        const timeoutIds = [];
        return new promise(function(resolve, reject){
            for(let i=countdown.seconds; i>=0; i--){
                timeoutIds.push(setTimeout(function(){
                    if(countdown.superstitious && i === 13){
                        // 대기중인 타임아웃을 모두 취소합니다.
                        timeoutIds.forEach(clearTimeout);
                        return reject(new Error('Oh my god'));
                    }
                    countdown.emit('tick',i);
                    if(i===0) resolve();
                }, (countdown.seconds-i)*1000));
            }
        })
    }
}
```
===> 



# TODO:
#### 오류 우선 콜백 (error-first callback)

콜백을 사용하면 예외 처리가 어려워지므로, 콜백과 관련된 에러를 처리할 방법의 표준
_
콜백의 첫 번째 매개변수에 에러 객체를 쓰자! 에러가 null / undefined이면 에러가 업슨ㄴ 것

파일을 읽는 함수
```javascript
// fs module is responsible for all async / sync file IO
const fs = require('fs');

const fname = 'may_or_may_not_exist.txt';
fs.readFile(fname, function(err,data){
  if(err) return console.log(`error reading file ${fname}: ${err.message}`);
  console.log(`${fname} contents: ${data}`)
});
```
==> 콜백에서 가장 먼저 하는 일은 err이 true 인지 확인
promise를 사용하지 않으면 오류 우선 콜백은 노드 개발의 표준!!



# TODO: 진짜 투두
https://davidwalsh.name/es6-generators
#### 제너레이터
제너레이터는 함수와 호출자 사이의 양방향 통신을 가능하게 한다
제너레이터는 원래 동기적인 성격 / 프로미스와 결합하면 비동기 코드를 효율적으로 관리할 수 있다

ddd

# TODO:
#### 함수 --> 순수 함수
순수함수 장점
_
순수한 함수를 쓰면 1. 코드를 테스트하기 쉽고 / 2. 이해하기 쉽고 / 3. 재사용하기 쉬우니깐
함수가 상황에 따라 다른 값을 반환하거나 부작용이 있다면 그 함수는 컨텍스트에 좌우되는 함수입니다

```javascript
const colors = ['red', 'orange', 'yellow', 'green', 'blue', 'indigo', 'violet'];
let colorIndex = -1;
function getNextRainbowColor(){
    if(++colorIndex >= colors.length) colorIndex =0;
    return colors[colorIndex];
}
```
==> 이 함수는 순수한 함수의 두 가지 정의를 모두 어깁니다
1. 입력이 같아도 (매개변수가 업다) 겨로가가 항상 다르고)
2. 변수 colorIndex를 바꾸는 부수 효과가 있습니다. colorIndex 변수는 getNextRainbowColor 함수에 속하지 않는데도 함수를 호출하면 변수가 바뀝니다 (부수효과)

순수 함수
-
입력이 같으면 결과도 항상 같고 / 다른 효과는 전혀 없는

```javascript
// closure로 감싸는
const getNextRainbowColor = (function(){
    const colors = ['red','orange','yellow','green','blue','indigo','violet'];
    let colorIndex = -1;
    return function(){
        if(++colorIndex >= colors.length) colorIndex = 0;
        return colors[colorIndex];
    };
})();

//사용 (아직 순수한 함수가 아니다)
setInterval(function(){
    document.querySelector('.rainbow')
    .style['background-color'] = getNextRainbowColor();
}, 500);
```
==> 아직 순수한 함수가 아니다 --> 문제는 만약 프로그램의 다른 부분에서 getNextRainbowColor()를 호출한다면 이 코드도 그 영향을 받는다는 겁니다

해결법!!
이터레이터 사용
```javascript
function getRainbowIterator(){
    const colors = ['red','orange','yellow','green','blue','indigo','violet'];
    let colorIndex = -1;
    return{
        next(){
            if(++colorIndex >= colors.length) colorIndex = 0;
            return {value: colors[colorIndex], done: false};
        }
    }
}

//사용
const rainbowIterator = getRainbowIterator();
setinterval(function(){
    document.querySeletor('.rainbow')
    .style['background-color'] = rainbowIterator.next().value;
}, 500);
```
==> 결국 next() 메서드는 매번 다른 값을 반환할 테니, 문제를 뒤로 미루었을 뿐 아니냐고 생각할 수도 있습니다.
틀린 말은 아니지만,
next()는 함수가 아니라 메서드!
메서드는 자신이 속한 객체라는 컨텍스트 안에서 동작하므로
메서드의 동작은 그 객체에 의해 좌우됩니다
프로그램의 다른 부분에서 getRainbowIterator를 호출하더도 독립적인 이터레이터가 생성되므로 다른 이터레이터를 간섭하지 않습니다.

**
함수도 객체다
자바스크립 함수는 Function 객체의 인스턴스

v가 함수 일때 / typeof(v) => function
v가 배열 일때  typeof(v) => object
v가 함수 일때, v instanceof Object => true

.. 변수가 함수인지 아닌지 확인하고 싶다면 먼저 typeof를 써보는 편이 좋습니다
**



# TODO:
#### IIFE / 비동기적 코드
IIFE를 이용해서 클로저를 만들 수 있다

IIFE 이용사례
_
비동기적 코드가 정확히 동작할 수 있도록 새 변수를 새 스코프에 만든다

블록 스코프 변수가 도입되기 전에는 이런 문제를 해결하기 위해 함수를 하나 더 썻다
```javascript
function loopBody(i){
    setTimeout(function(){
        console.log(i==0 ? "go!": i);
    }, (5-i)*1000);
}
// 루프의 각 단계에서 함수에 전달되는 것은
//변수 i가 아니라 i의 값
var i;
for(i=5; i>-0; i--){
    loopBody(i);
}
```
==> 스코프 입골 개가 만들어 졌고 변수도 일곱 개가 만들어졌다 하나는(외부) / 여섯개는(loopBody)를 호출할 때마다

IIFE way
```javascript
var i;
for(i=5; i>=0; i--){
    //익명함수
    (function(i){
        setTimeout(function(){
            console.log(i==0? "go!":i);
        }, (5-i)*1000);
    })(i);
}
```

BLOCK SCOPE way
```javascript
// let 키워드를 이런 식으로 사용하면
// 자바스크립트는 루프의 단계마다 변수 i의 복사본을 새로 만든다
for(let i=5; i>=0; i--){
    setTimeout(function(){
        console.log(i===0? "go!":i);
    }, (5-i)*1000);
}
```


#### 변수로서의 함수
```javascript
```
