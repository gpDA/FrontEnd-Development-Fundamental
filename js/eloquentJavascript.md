# eloquentJavascript

#### HTTP status

EXAMPLE - 
`HTTP/1.1 200 OK`

STATUS CODE
2 --> success
4 --> something wrong (404 NOT FOUND)
5 --> error happens on the server

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

#### HTTP sandboxing
-
SECURITY
Browsers disallow scripts to make HTTP requests to other domains (themafia.org / mybank.com)

To build systems that want to access several domains for legitimaste reasons
`Access-Control-Allow-Orign: *`

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

