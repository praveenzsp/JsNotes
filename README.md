# JsNotes

In Java script, everything happens inside an execution context

Inside execution context, there are two phases

*   Phase 1 is memory creation
    
*   Phase 2 is code execution, also known as thread of execution
    

**Memory creation phase**

Even before running any Java script code, all the variables and functions in our source code will be allotted some memory inside global execution context and that global execution context will be pushed into the call stack.

For variables, a default value of undefined is assigned and for functions, the whole signature will be stored in memory.

The next step is code execution.

**Code execution phase**

Since Javascript is synchronous and single threaded, code is executed line by line. 

For variables, the value which we are assigning in our code will replace the default value of undefined, which means undefined will be the value of the variable until we execute our code and assign some value to it. So if we try to print the variable value before we assign anything to it, it will print undefined.

For functions, they are already stored inside memory even before executing our code. So whenever we invoke the function and new global execution context for that specific function will be created inside the global context and that function specific education context will be pushed into the call stack and once that function, returns something. It will be popped off the stack and that specific function execution context will be garbage collected.

**Rules of let, var and const:**

There are two things, one is resigning and the other is redeclaring.

Reassigning:
\`\`\`
var x=10

x=100
\`\`\`

Redeclaring:

\`\`\`
var x=10

var x=100
\`\`\`

*   Var -> resigning✅ redeclaring✅
    
*   Let -> resigning✅ redeclaring❌
    
*   Const -> resigning❌ redeclaring❌
    

**Hoisting**

In simple terms, hoisting can be considered as allocating memory to the variables or functions. So all the variables like var, let and const will be hoisted during memory creation phase. But each of those variables act differently while code execution phase.

For var, whenever we create a var variable, it will be stored inside global execution context which means we can access it anywhere in our code. This is the case when var variables are created outside any block or function

But when we create a var variable inside a function, its scope will be limited to that function, so we cannot access that var variable outside the function. Because when we create a var variable inside function, it will be assigned some memory inside that function execution context, so if we try to access it outside the function, it will throw an error, saying that variable is not defined because that variable is not present inside global execution context.

\`\`\`jsfunction x(){

    var a=10

}

x()

console.log(a) // this will throw an error, saying a is not defined\`\`\`

\`\`\`jsvar a=100;

function x(){

    var a=10

}

x()

console.log(a) // this will print hundred because we can find a inside global execution context with a value of hundred\`\`\`

\`\`\`jsconsole.log(a) // in this example, the variable a is hoisted, but the output will be undefined because we are accessing it even before assigning something to it, so the default value of undefined will be printed

var a=100;\`\`\`

For let and const, both these kind of variables also gets hoisted, but when you try to access them, they will act different. and as a matter of fact, whenever we create let and const variables, they will be stored inside as separate object known as script, this is unlike var variables, which will be stored inside global object, also known as window.

\`\`\`js

console.log(a) // this will give an error saying cannot access a before initialisation.

let/const a=100;

\`\`\`

We are getting the above error because of something known as **temporal dead zone**. temporal dead zone can be defined as the time period between a variable having undefined and then the same variable gets assigned some value. This in between time period is called temporal dead zone. And if you try to access the variable in this time period, it will throw the above error.

For functions. Hoisting is possible, which means we can call a function even before defining it because during the memory creation phase, the whole function will be stored inside memory so we can access it just like usual.\`\`\`js

a()

function a(){

    console.log("hello") // this will print, hello

}

\`\`\`

In conclusion, we can say that let, var and const are hoisted, but when we try to access them, they act differently because of where they are getting stored in the memory. Var will be stored in global object and let and const will be stored outside global object. And we cannot access them during temporal dead zone

**Lexical scope and scope chaining:**

Lexical scope can be defined as the combination of local scope and its parent scope. For example, if you can't find a variable in local scope, we will go to its parent’s scope to access it. And if we can't find that variable in parent scope too we will then go to the parent’s parent scope. This process of going from local to parent and parent to its parent is known as scope chaining.

Lexical scope and scope chaining is mostly used closures.

\`\`\`js

function x(){

    var a=10

    function y(){

        console.log(a) // prints 10

    }

    y()

}

x()\`\`\`

In the above example, we can say that the function Y is able to access the variable a with the help of lexical scope. And both functions X and Y together form a closure.

\`\`\`jsfunction x(){

    var a=10

    function y(){

        console.log(a) // prints 10

    }

    return y

}

const z=x()

z()

\`\`\`

Example, we are returning the function y, so whenever the function y is returned. Its lexical Scope will also be returned only because of this, we are able to access the variable a even outside the function X.(when we invoke the function X, it is returning another function y, so after the function X returns the function y, X will be removed from memory, with this in mind variable a will also vanish, but when y is getting returned. It's lexical scope is also getting returned and we will have a reference to the variable a inside that returned lexical scope, that's how we are able to access the variable a)

So whenever a function, which is part of the closure is returned, its lexical scope will also be returned and we can access variables and functions that are inside that returned lexical scope.

**Closures:**

A function which is bundled together along with its lexical scope, forms a closure.

Lexical scope == local scope + parent’s scope

**Notorious example off closure**

\`\`\`jsfunction demo(){

    for(var i=1;i<=5;i++){

        setTimeout(()=>{

            console.log(i)

        }, 1000)

    }

}

demo() // here we are expected that they will be 1 to 5 will be printed every second, instead 6 will be printed five times\`\`\`

Six is getting printed because for every copy of the timeout function, we are referencing the same variable i which is a global variable. And also Java script waits for nothing. So when the value of I is 1, the setTimeout function will be called again and even before it completes it's previous execution, the value of I becomes 2. This will continue to happen and 5 copies of setTimeout function is pushed into the stack. now the value of I is six inside global memory and once the timer runs out all the functions will print the value of I, but since all the functions are pointing the same global variable I, all those functions will print the value of six.

If we want to print the numbers. 1 to 5, we should change the type of the variable I to let. since let variables are block scoped, for every copy of the set time out function call, a new variable, I will be created in the memory. So now every copy of the function is having its own variable i,

**Block scope:**

A block can be defined as a group of statements that are inside {}.

Whenever we create a variable inside a block, instead of global object, it will be stored inside something known as block. So if we try to access those variables, we will get an error because they are present inside the block, but not in global object.

Let and const have block scope.

The values which we pass during the function invocation are known as arguments

And the placeholder, which receive the values in the function, signature or known as parameters

The ability to pass a function as an argument inside another function or the ability to use functions, just like any values is known as first class function or first class citizen

**Callbacks:**

A function which is passed as an argument to another function is known as a callback function.

\`\`\`jsfunction x(num,fn){  // the name of this parameter can be anything. In this case, it is fn

    console.log(num)

    fn();

}

x(10,function y(){

    console.log("y")

})\`\`\`

In the above, example, y can be called as a callback function because we are passing it as an argument to the function X. So we are calling function X and function X is calling function y.

With the help of call backs we can achieve asynchronicity in Java script. All these web APIs like setTimeout, setInterval help us achieve asynchronicity.

In Java script, everything executes through the call stack.

Output will be X and Y and after sometime timer will be printed. When we start executing the above code, firstly, set timeout function will be stored somewhere to execute it later. And when the execution reaches line number 11 function X will be called and control goes to line 7 now function, X will execute and prints X and then it will call function Y. Now the control reaches back to line 11 and function Y will be executed and it will print y. During this execution, both function X and Y comes into call stack and once they are finished, both functions will be popped. So now when the five second timer runs out. The set timeout function comes into call, stack and start executing now. It will print timer and then that function will be popped off the stack.

Since JavaScript is single threaded, we have to execute things in order, but if there comes a heavy computing function, the rest of the code will be blocked from executing until that heavy computing function, complete its execution. This is what known as main thread blocking. in order to avoid this we use asynchronous operations.

**Asynchronous and Eventloop in JS:**

Whenever a global execution context comes into the call stack it immediately executes that and it doesn't wait for anything. But while performing asynchronous operations we need to set some timers to our code, but we can’t do that with the help of call stack. so in order to achieve this web APIs comes into the picture. Examples of web APIs are set time out, fetch, local storage, location, console, DOM APIs. All these web API are provided by the browser. And all these web API reside inside window object, so they can be accessed with the help of window object like window.location etc

In the above, example, the output will be start end call back.

So when we start executing our court, initially, a global execution context will be created and it will be pushed into the call stack. Now, when the first line executes, it will print, start to the console with the help of Console web API, which resides inside window object. Now when the control goes to set timeout, so the callback function inside set, timeout function will be stored somewhere with the help of set timeout web API so that the callback function can be executed after the timer runs out. and the timer starts ticking. And now the control goes to last line and End will be printed to the console with the help of console web API.

And once the 5000 ms timer runs out, we need to execute the callback function. But in order to execute the callback function, we need to create an execution context and push it into the call stack. And we can directly push the execution context into call stack, and here comes the call back queue. Call back queue stores the call back function inside it, and with the help of event loop we are able to push the callback function execution context into the call stack so that it can be executed and prints. Call back to the console.

Here, event look act as a gatekeeper in putting the callback function inside call stack. It's only job is to check whether the call stack is empty or not if it is empty, it will take a call back function from call back queue and puts it into the call stack.

In this above example, output will be start end CB Netflix CB set timeout.

When we start executing a global execution context will be created, and it will be pushed into the call, stack. Now the control goes to 1st line and it will print start to the console with the help of Console API. Now the control goes to set time out, and then the callback inside set time out function will be stored inside web API environment, and the timer will start. Now the control goes to fetch function and since fetch returns a promise, we will execute a call back once fetch returns the data. So the callback function inside switch will also get stored in the web API environment. Now here comes micro task queue. Micro task queue job is similar to call back Q, but micro task Q stores call back functions related to promises. And micro task queue takes higher priority than callback Q.

Now the call back function will be pushed into micro task Q Once we get the data from the server. and while executing the code after the function for sometime, the 5000 timer will also expire and the set timeout callback function function will be pushed into the call back Q. So now, both set time out. Call back and fetch call back are waiting in their respective queues. And now the control goes to the last line and End will be printed to the console. After this, the global execution context inside call stack will be popped off.

Now the call is empty, so the event loop will push the fetch call back function inside micro task Q, since it takes higher priority than the set timeout callback function. So now the fetch call back function will be executed and it will print CB Netflix to the console. After this fetch callback function’s execution context will be popped of the call stack. So the call stack is empty again.

Now the event loop will push the set time out call back function into call stack and it will execute it and CB set timeout will be printed to the console and its execution context will be popped off the stack.

When there are lot of tasks inside the micro task queue, it will make the callback function inside. Call back Q to wait for a long time. This is known as starvation.

**Java script runtime environment** 

It can be treated as a container that has all the things required to execute Java script code. It includes things like Java script, engine, event loop, web API, call back Q, micro task Q etc

**Concurrency model:**

‘’’Js

console.log("start")

setTimeout(function cb(){

    console.log("callback")

},5000)

console.log("end")

// some piece of code which takes lot of time(say 10 sec) to execute\`\`\`

The output will be start end call back after ~10 seconds

Here we are expected to see call back printed on the control after five seconds, but we are seeing call back printed on the console after 10 seconds.

This is because while executing the global execution context, when we reach the last line, it takes around 10 seconds to complete its execution. So after 10 seconds, the global execution context will be popped off the call stack. but by the time, the global execution context complete its execution the timer of 5000 ms have already passed. But we can't execute the callback function right after 5000 ms because the call stack is not yet empty. It is still executing that heavy computing code so after completing the heavy computing code, only then the callback function will be pushed into the call stack and then it will be executed. So instead of five seconds, call back will be printed after 10 seconds, this is known as concur model. 

**Higher order functions:**

The function which takes another function as an argument or a function which returns another function is known as a high order function.

\`\`\`jsfunction processArray(arr, square) {

  return arr.map(square); // this will apply square function on every element of the array

}

function square(num) {

  return num \* num;

}

const numbers = \[1, 2, 3, 4\];

const squaredNumbers = processArray(numbers, square);  // \[1, 4, 9, 16\] here Square is a callback function and processArray is a higher order function

\`\`\`Here, processArray is a higher-order function because it accepts a function (callback) as an argument. The square function is passed in as the callback to apply to each element of the array.Here, processArray is a higher-order function because it accepts a function (callback) as an argument. The square function is passed in as the callback to apply to each element of the array.

**Callback hell:**

**Callback Hell** refers to the situation where multiple nested callbacks make the code hard to read and maintain. This usually happens in asynchronous JavaScript when you have to perform sequential tasks, and each task depends on the result of the previous one, leading to deeply nested callbacks.

**Key Points:**

*   It's a result of multiple nested callbacks.
    
*   Makes code hard to read and maintain (often called "pyramid of doom").
    
*   Can be avoided using Promises or async/await.
    

**Example of Callback Hell:**

\`\`\`jsfunction getUser(userId, callback) {

  setTimeout(() => {

    console.log("Got user from DB");

    callback({ userId, name: "John" });

  }, 1000);

}

function getOrders(user, callback) {

  setTimeout(() => {

    console.log(\`Got orders for ${user.name}\`);

    callback(\[{ orderId: 1, amount: 100 }, { orderId: 2, amount: 50 }\]);

  }, 1000);

}

function processPayment(orders, callback) {

  setTimeout(() => {

    console.log(\`Processing payment for orders\`, orders);

    callback("Payment successful");

  }, 1000);

}

// Callback Hell Example:

getUser(1, (user) => {

  getOrders(user, (orders) => {

    processPayment(orders, (paymentStatus) => {

      console.log(paymentStatus);

    });

  });

});

\`\`\`

In the above example, we are calling get user function, get user function. Will call get orders function, and get orders function will call process payment function. So this is the problem with nested call back also known as call back hell.

**Problems:**

*   **Hard to Read**: Code becomes more indented and harder to follow.
    
*   **Difficult to Debug**: Tracing errors can be complicated due to deep nesting.
    

**Solutions:**

1.  **Using Promises**: Promises provide a cleaner way to handle asynchronous code.
    
2.  **async/await**: Allows writing asynchronous code that looks more like synchronous code.
    

**Promises:**

A promise is an object which will store the result of an asynchronous operation. its value might be undefined for now, but it might change in the future.

And we will attach call back functions to this promise object instead of passing the callback function as an argument to another function. And this promise object will make sure that the promise will be resolved or rejected only once and also provide some other trust benefits.

Let's say there are two functions. One is add to cart and the other is proceeded to payment. Usually the flow would be like add to cart and then proceed to payment. So we cannot execute proceed to payment even before adding to the cart, so add to cart must be executed first, and then proceed to payment follows.

So in order to implement this, we use callbacks. now we will pass. Proceed to payment function as a call back to add to cart function, and we will add to cart function and that will call proceed to payment function, so this is how we restored the execution order with the help of callbacks. Like this

\`\`\`jsfunction addToCart( cb){

cb();

}

addToCart(function (){

proceedToPayment();

})

\`\`\`

But whenever our application grows and the number of functions increase, this will lead to the problem of callback hell.

So in order to make things work without callback hell, we use promises.

\`\`\`js

Const promise = addToCart();

promise.then(proceedToPayment())

\`\`\`

In the above example, add to cart function will return a promise, and until that promise has some data in it, a default value of undefined will be stored. And once at returns, the data undefined will be replaced by that data. Now once the data is available, proceed to payment function will be called automatically by the promise object. In this way, restored the order of execution and proceeded to payment won't be called until add to cart returns data to the promise object.

\`\`\`jsconst URL= "some url here"

function printResult(data){

    console.log(data);

}

const result=fetch(URL); // since fetch returns a promise, result will be a promise object.

result.then((data)=>printResult(data))\`\`\`

Now when the code starts executing, we will call the fetch function with some URL since fetch returns a promise, result will be a promise object, which will have various fields like promise state, promise response etc. Until the promise is resolved and some data is assigned to the promise response. The state will be pending once the data is assigned state will be updated to fulfilled and if something goes wrong and the data is not returned, then the state will be rejected.

Let's say, in our example, promise gets resolved, and the data is assigned properly to the promise response. So now we can access the data with the help of then function and whatever data we got response can be accessible inside then(). Since the promise is resolved, the promise object will now automatically call the printeResult call back function, which is inside then() along with the data we got from the API.

In the above example, line number 3-9, includes a solution that uses callbacks. And the highlighted piece of code implements the same with the help of promises.
