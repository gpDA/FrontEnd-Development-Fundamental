### Single Thread vs. Multi Threads
Single Thread
  - single sequence of execution
  
  EXAMPLE
    JavaScript
    - JavaScript code is single-threaded but stuff done by browser (AJAX request, rendering, event triggers) is not
    
    Execution of JS program
    1) Inital code execution during page load
    -  
    2) Event loop
    - After initial loading, all event handlers are attached
        (all AJAX requests are send / our observables are observing)
    - *We Do not Want to Change the DOM in Parallel*
        - DOM tree in not thread-safe (mess if we try to do it)
        - Thus, JS code should be executed in single thread (while at       the same time ensuring that it handles all the EVENTS           and does not lose any CALLBACK)
    - DEFINITIONS
        - CALL STACK : contains all called functions
        - EVENT HANDLERS QUEUE : a queue of event handlers that are         waiting to be executed
        - EVENT LOOP : check whether event handlers queue is not            empty / if not empty, calls top event handler and             removes it from queue
        - JS WEB APIs - Provided by the browser / responsible for filling the Event handlers queue

Multi Thread
  - implemented as 1) user-level threads 2) kernel-level threads

  ADVANTAGE
  - All threads of a process share its resources ( memory / data/ files )
    - economical
  - Program is able to run even if part of it is blocked
  - increases concurrency of the System

  DISADVANTAGE
  - Multithreaded processes are quite complicated to implement
  - difficult to handle concurrency in multithreaded processes
  - 


### Async in JS
    - All async code in JS is based on Web APIs provided by the browser

    TWO GROUPS
    1) APIs that fires a callback after some work is done
        - AJAX request / onload handle within the DOM / web worker response.
        - The callback is fired as when something changes, and the *the purpose of this callback is to react on this changes*
        - When a callback must be executed, it is added to the Event handlers queue
    2) setTimeout(callback, time) AND setInterval(callback, time) functions
        - SetTimeout : runs a callback once after specified amount of time
        - SetInterval : runs a callback at a specified time interval
    
        *Not reacting to changes, but just running our code*
        *Main purpose is to provided us the `means of breaking synchronous execution into async parts`
        NO GUARANTEE that callback will be executed after the amount of time that you specifiy
        DO GUARANTEE that callback will be added to the Event handlers queue, SO "time" argument should be treated as "NOT EARLIER THAN, BUT AFTER THE SPECIFIED TIME"

### Parallelism in JS
    - JS is single-threaded in same execution context. We have 2 options to achieve parallelism in JS
    1) setTimeout function
        - main idea is to unblock user interaction with the interface, but at the same time, execute logic that is needed to be executed
    2) WebWorkers
        - WebWorkers represent the concept of parallel threads in JS but with limitations



### OOP characteristics
    1) Inheritance
        - inehriting or deriving properties of an existing class to get new class. (common features or characteristics)
    2) polymorphism and overloading
        - polymorphism : a state having many shapes.
        - A language's ability to process objects of various types and classes through a single, uniform interface
        EXAMPLE
        - a parent class refers to a child class object (IS-A)
        Animal / Cat
        - `Cat` be a subclass of Animal (ANY cat IS animal)
    3) Data Hiding
        - Data is hidden inside the class by declaring it as private inside the class
    4) Encapsulation
        - bundling of data and methods operating on this data into one unit
        - restirct direct access to some of the object's components ( SECURITY / FLEXIBILITY / MAINTAINABILITY )
            - get / set
    5) Reusability
        - Inheritance

    *Objects && Class*
    - Objects 
        : is an instance of a class
    - Classes
        : contain data and functions bundled together under a unit
        : class is a collection of similar objects

### Singlton Class
    - In object-oriented programming, a singleton class is a class that can have only one object ( an instance of the class ) at a time.
        - After first time, if we try to instantiate the Singleton class, the new variable also points to the first instance created
        - So, whatever modifications we do to any variable inside the class through any instance, 
            - it affects the variable of single instance created
            - is visible if we access that variable through any variable of that class type defined
    TO DESIGN A SINGLETON Class
    - Make construcotr as private
    - Write a static method that has return type object of this singleton class

    *Normal Class vs Singleton Class*
    - For normal class, we use constructor
    - For singleton class, we use getInstance() method

    BENEFIT
    - Singleton prevents other objects from instantiating their own copies of the Singleton object, ensuring that all objects access the single instance



### What is Synchronized
    - Java is Multi-threaded
        Multi-threaded programs may often come to a situation where multiple threads try to access the same resources and finally produce `error` and `unforseen results`
    - So, synchronization method ensures that only one thread can access the resource at a given point of time



### Microservice architecture vs. Monolothic architecture
    BENEFIT of Monolithic architecture
    - Fast to develop
    - Easy to test (using Selenium (Django))
    - Simple to scale horizontally by running multiple copies behind a load balancer
    - Great for early stages of the project
    
    DRAWBACK of monolithic architecture
    - limitation in size and complexity
    - must redeploy the entire application on each update
    - Continuous deployment is difficult

    Microservice Architecture
    - split application into a set of smaller, interconnected services instead of building a single monolithic application
    - GOOD relationship between the application and the database
        ( Instead of sharing a single database schema with other services, each service has its own database schema)
    - Having a database schema per service is seential if you want to benefit from microservices ( LLOSE COUPLING)
        Each of the services has its own database
    
    WEB + MOBILE --> API --> SERVICES --> DATABASE

    BENEFIT of Microservice architecture
    - reduces barrier of adopting new technologies
    - scaled independently
    - deployed indepdently

    DRAWBACK of mmicroservice architecture
    - distributed system / need to implement an iter-process communication mechanism based on either messaging / RPC
    - DEPLOYMENT + TESTING is tricky
    - Each instance need to be configured, deployed, scaled, and monitored


### Symmetric cryptography vs Asymmetric cryptography (public key)
    Symmetric Encryption
    - uses the same key to decrpt `cipher text`
    - IMPORTANCE how to store key
    "SHARING"

     Asymmetric Encryption (RSA Algorithm)
     - Use different key (public key (encryption) / secret key (decryption))
     - Exchange PUBLIC KEY
     - Encrypts using PUBLIC KEY
     - Decrypts using PRIVATE KEY
     "KEEP itself no need to SHARING" 


### Design ATM machine


### How to get minimum value in SQL
    1) MAX on column `select MAX(prices)`
    2) self join USING ALIAS
    (create all the possible pairs for which each value in one column is greater than the other column)
    `SELECT DISTINCT Numbers FROM compare
    WHEE Numbers NOT IN (SELECT Smaller.Numbers FROM Compare AS Larger JOIN Compare AS Smaller ON smllaer.Numbers< lARGER.Numbers)

    "DUPLICATION"
    Order by .. descending order
    LIMIT 1



### What is Recursion
- functions calls itself directly or indirectly 
    (TOH / Inorder / Preorder Traversals)




    


2. Asynchrous code in JS
3. Parallelism in JS
4. OOP characteristics
5. What is singleton in object
6. What is synchronized
7. Microservice architecture vs Monolithic architecture
8. Symmetric cryptography vs Asymmetric cryptography (public key)
