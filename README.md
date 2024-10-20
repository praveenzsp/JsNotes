# JsNotes

In Java script, everything happens inside an execution context

Inside execution context, there are two phases

*   Phase 1 is `memory creation`
    
*   Phase 2 is code execution, also known as thread of execution
    

# **Memory creation phase**

Even before running any Java script code, all the variables and functions in our source code will be allotted some memory inside global execution context and that global execution context will be pushed into the call stack.

For variables, a default value of undefined is assigned and for functions, the whole signature will be stored in memory.

The next step is code execution.

# **Code execution phase**

Since Javascript is synchronous and single threaded, code is executed line by line. 

For variables, the value which we are assigning in our code will replace the default value of undefined, which means undefined will be the value of the variable until we execute our code and assign some value to it. So if we try to print the variable value before we assign anything to it, it will print undefined.

For functions, they are already stored inside memory even before executing our code. So whenever we invoke the function and new global execution context for that specific function will be created inside the global context and that function specific education context will be pushed into the call stack and once that function, returns something. It will be popped off the stack and that specific function execution context will be garbage collected.

# **Rules of let, var and const:**

There are two things, one is resigning and the other is redeclaring.

Reassigning:
```
var x=10

x=100
```

Redeclaring:

```
var x=10

var x=100
```

*   Var -> resigning✅ redeclaring✅
    
*   Let -> resigning✅ redeclaring❌
    
*   Const -> resigning❌ redeclaring❌
    

# **Hoisting**

In simple terms, hoisting can be considered as allocating memory to the variables or functions. So all the variables like var, let and const will be hoisted during memory creation phase. But each of those variables act differently while code execution phase.

For var, whenever we create a var variable, it will be stored inside global execution context which means we can access it anywhere in our code. This is the case when var variables are created outside any block or function

But when we create a var variable inside a function, its scope will be limited to that function, so we cannot access that var variable outside the function. Because when we create a var variable inside function, it will be assigned some memory inside that function execution context, so if we try to access it outside the function, it will throw an error, saying that variable is not defined because that variable is not present inside global execution context.

```
function x(){

    var a=10

}

x()

console.log(a) // this will throw an error, saying a is not defined
```

```
var a=100;

function x(){

    var a=10

}

x()

console.log(a) // this will print hundred because we can find a inside global execution context with a value of hundred
```

```
console.log(a) // in this example, the variable a is hoisted, but the output will be undefined because we are accessing it even before assigning something to it, so the default value of undefined will be printed

var a=100;
```

For let and const, both these kind of variables also gets hoisted, but when you try to access them, they will act different. and as a matter of fact, whenever we create let and const variables, they will be stored inside as separate object known as script, this is unlike var variables, which will be stored inside global object, also known as window.

```

console.log(a) // this will give an error saying cannot access a before initialisation.

let/const a=100;

```

We are getting the above error because of something known as **temporal dead zone**. temporal dead zone can be defined as the time period between a variable having undefined and then the same variable gets assigned some value. This in between time period is called temporal dead zone. And if you try to access the variable in this time period, it will throw the above error.

For functions. Hoisting is possible, which means we can call a function even before defining it because during the memory creation phase, the whole function will be stored inside memory so we can access it just like usual.
```

a()

function a(){

    console.log("hello") // this will print, hello

}

```

In conclusion, we can say that let, var and const are hoisted, but when we try to access them, they act differently because of where they are getting stored in the memory. Var will be stored in global object and let and const will be stored outside global object. And we cannot access them during temporal dead zone

# **Block scope:**

A block can be defined as a group of statements that are inside {}.

Whenever we create a variable inside a block, instead of global object, it will be stored inside something known as block. So if we try to access those variables, we will get an error because they are present inside the block, but not in global object.

Let and const have block scope.
```
{
    let/const a=10
}

console.log(a) // this will cause an error because a is now block scoped and we cannot access it outside the block. but if a is var, we can access it even outside the block
```

The values which we pass during the function invocation are known as arguments

And the placeholder, which receive the values in the function, signature or known as parameters

The ability to pass a function as an argument inside another function or the ability to use functions, just like any values is known as first class function or first class citizen.

# **Lexical scope and scope chaining:**

Lexical scope can be defined as the combination of local scope and its parent scope. For example, if you can't find a variable in local scope, we will go to its parent’s scope to access it. And if we can't find that variable in parent scope too we will then go to the parent’s parent scope. This process of going from local to parent and parent to its parent is known as scope chaining.

Lexical scope and scope chaining is mostly used in closures.

```

function x(){

    var a=10

    function y(){

        console.log(a) // prints 10

    }

    y()

}

x()
```

In the above example, we can see that the function Y is able to access the variable a with the help of lexical scope. And both functions X and Y together form a closure.

```
function x(){

    var a=10

    function y(){

        console.log(a) // prints 10

    }

    return y

}

const z=x()

z()

```

Example, we are returning the function y, so whenever the function y is returned. Its lexical Scope will also be returned only because of this, we are able to access the variable a even outside the function X.(when we invoke the function X, it is returning another function y, so after the function X returns the function y, X will be removed from memory, with this in mind variable a will also vanish, but when y is getting returned. It's lexical scope is also getting returned and we will have a reference to the variable a inside that returned lexical scope, that's how we are able to access the variable a)

So whenever a function, which is part of the closure is returned, its lexical scope will also be returned and we can access variables and functions that are inside that returned lexical scope.

# **Closures:**

A function which is bundled together along with its lexical scope, forms a closure.

Lexical scope == local scope + parent’s scope

Lexical scope and scope chaining is mostly used in closures.

```

function x(){

    var a=10

    function y(){

        console.log(a) // prints 10

    }

    y()

}

x()
```

In the above example, we can see that the function Y is able to access the variable a with the help of lexical scope. And both functions X and Y together form a closure.

```
function x(){

    var a=10

    function y(){

        console.log(a) // prints 10

    }

    return y

}

const z=x()

z()

```

Example, we are returning the function y, so whenever the function y is returned. Its lexical Scope will also be returned only because of this, we are able to access the variable a even outside the function X.(when we invoke the function X, it is returning another function y, so after the function X returns the function y, X will be removed from memory, with this in mind variable a will also vanish, but when y is getting returned. It's lexical scope is also getting returned and we will have a reference to the variable a inside that returned lexical scope, that's how we are able to access the variable a)

So whenever a function, which is part of the closure is returned, its lexical scope will also be returned and we can access variables and functions that are inside that returned lexical scope.

## **Notorious example off closure**

```
function demo(){

    for(var i=1;i<=5;i++){

        setTimeout(()=>{

            console.log(i)

        }, 1000)

    }

}

demo() // here we are expected that they will be 1 to 5 will be printed every second, instead 6 will be printed five times
```

Six is getting printed because for every copy of the timeout function, we are referencing the same variable i which is a global variable. And also Java script waits for nothing. So when the value of I is 1, the setTimeout function will be called again and even before it completes it's previous execution, the value of I becomes 2. This will continue to happen and 5 copies of setTimeout function is pushed into the stack. now the value of I is six inside global memory and once the timer runs out all the functions will print the value of I, but since all the functions are pointing the same global variable I, all those functions will print the value of six.

If we want to print the numbers. 1 to 5, we should change the type of the variable I to let. since let variables are block scoped, for every copy of the set time out function call, a new variable, I will be created in the memory. So now every copy of the function is having its own variable i,

# **Callbacks:**

A function which is passed as an argument to another function is known as a callback function.

```
function x(num,fn){  // the name of this parameter can be anything. In this case, it is fn

    console.log(num)

    fn();

}

x(10,function y(){

    console.log("y")

})
```

In the above, example, y can be called as a callback function because we are passing it as an argument to the function X. So we are calling function X and function X is calling function y.

With the help of call backs we can achieve asynchronicity in Java script. All these web APIs like setTimeout, setInterval help us achieve asynchronicity.

In Java script, everything executes through the call stack.
<img width="742" alt="Not paused" src="https://github.com/user-attachments/assets/2c8fee9c-8a98-4078-b24f-fb74d7b934b9">

For practical execution visit this http://latentflip.com/loupe
Output will be X and Y and after sometime timer will be printed. When we start executing the above code, firstly, set timeout function will be stored somewhere to execute it later. And when the execution reaches line number 11 function X will be called and control goes to line 7 now function, X will execute and prints X and then it will call function Y. Now the control reaches back to line 11 and function Y will be executed and it will print y. During this execution, both function X and Y comes into call stack and once they are finished, both functions will be popped. So now when the five second timer runs out. The set timeout function comes into call, stack and start executing now. It will print timer and then that function will be popped off the stack.

Since JavaScript is single threaded, we have to execute things in order, but if there comes a heavy computing function, the rest of the code will be blocked from executing until that heavy computing function, complete its execution. This is what known as main thread blocking. in order to avoid this we use asynchronous operations.

# **Asynchronous and Eventloop in JS:**

Whenever a global execution context comes into the call stack it immediately executes that and it doesn't wait for anything. But while performing asynchronous operations we need to set some timers to our code, but we can’t do that with the help of call stack. so in order to achieve this web APIs comes into the picture. Examples of web APIs are set time out, fetch, local storage, location, console, DOM APIs. All these web API are provided by the browser. And all these web API reside inside window object, so they can be accessed with the help of window object like window.location etc
<img width="1512" alt="Screenshot 2024-09-25 at 3 41 14 PM" src="https://github.com/user-attachments/assets/6a8547ea-ad69-4f3a-84ab-de7db97ab876">


In the above, example, the output will be start end call back.

So when we start executing our court, initially, a global execution context will be created and it will be pushed into the call stack. Now, when the first line executes, it will print, start to the console with the help of Console web API, which resides inside window object. Now when the control goes to set timeout, so the callback function inside set, timeout function will be stored somewhere with the help of set timeout web API so that the callback function can be executed after the timer runs out. and the timer starts ticking. And now the control goes to last line and End will be printed to the console with the help of console web API.

And once the 5000 ms timer runs out, we need to execute the callback function. But in order to execute the callback function, we need to create an execution context and push it into the call stack. And we can directly push the execution context into call stack, and here comes the call back queue. Call back queue stores the call back function inside it, and with the help of event loop we are able to push the callback function execution context into the call stack so that it can be executed and prints. Call back to the console.

Here, event look act as a gatekeeper in putting the callback function inside call stack. It's only job is to check whether the call stack is empty or not if it is empty, it will take a call back function from call back queue and puts it into the call stack.
<img width="1512" alt="Screenshot 2024-09-25 at 4 19 02 PM" src="https://github.com/user-attachments/assets/799c7214-70b0-4f2e-bdfa-3ae670268ffb">


In this above example, output will be start end CB Netflix CB set timeout.

When we start executing a global execution context will be created, and it will be pushed into the call, stack. Now the control goes to 1st line and it will print start to the console with the help of Console API. Now the control goes to set time out, and then the callback inside set time out function will be stored inside web API environment, and the timer will start. Now the control goes to fetch function and since fetch returns a promise, we will execute a call back once fetch returns the data. So the callback function inside switch will also get stored in the web API environment. Now here comes micro task queue. Micro task queue job is similar to call back Q, but micro task Q stores call back functions related to promises. And micro task queue takes higher priority than callback Q.

Now the call back function will be pushed into micro task Q Once we get the data from the server. and while executing the code after the function for sometime, the 5000 timer will also expire and the set timeout callback function function will be pushed into the call back Q. So now, both set time out. Call back and fetch call back are waiting in their respective queues. And now the control goes to the last line and End will be printed to the console. After this, the global execution context inside call stack will be popped off.

Now the call is empty, so the event loop will push the fetch call back function inside micro task Q, since it takes higher priority than the set timeout callback function. So now the fetch call back function will be executed and it will print CB Netflix to the console. After this fetch callback function’s execution context will be popped of the call stack. So the call stack is empty again.

Now the event loop will push the set time out call back function into call stack and it will execute it and CB set timeout will be printed to the console and its execution context will be popped off the stack.

When there are lot of tasks inside the micro task queue, it will make the callback function inside. Call back Q to wait for a long time. This is known as starvation.

# **Java script runtime environment** 

It can be treated as a container that has all the things required to execute Java script code. It includes things like Java script, engine, event loop, web API, call back Q, micro task Q etc

# **Concurrency model:**

```

console.log("start")

setTimeout(function cb(){

    console.log("callback")

},5000)

console.log("end")

// some piece of code which takes lot of time(say 10 sec) to execute
```

The output will be start end call back after ~10 seconds
For practical execution visit this link http://latentflip.com/loupe

Here we are expected to see call back printed on the control after five seconds, but we are seeing call back printed on the console after 10 seconds.

This is because while executing the global execution context, when we reach the last line, it takes around 10 seconds to complete its execution. So after 10 seconds, the global execution context will be popped off the call stack. but by the time, the global execution context complete its execution the timer of 5000 ms have already passed. But we can't execute the callback function right after 5000 ms because the call stack is not yet empty. It is still executing that heavy computing code so after completing the heavy computing code, only then the callback function will be pushed into the call stack and then it will be executed. So instead of five seconds, call back will be printed after 10 seconds, this is known as concur model. 

# **Higher order functions:**

The function which takes another function as an argument or a function which returns another function is known as a high order function.

```
function processArray(arr, square) {

  return arr.map(square); // this will apply square function on every element of the array

}

function square(num) {

  return num \* num;

}

const numbers = \[1, 2, 3, 4\];

const squaredNumbers = processArray(numbers, square);  // \[1, 4, 9, 16\] here Square is a callback function and processArray is a higher order function
```

Here, processArray is a higher-order function because it accepts a function (callback) as an argument. The square function is passed in as the callback to apply to each element of the array.Here, processArray is a higher-order function because it accepts a function (callback) as an argument. The square function is passed in as the callback to apply to each element of the array.

# **Callback hell:**

**Callback Hell** refers to the situation where multiple nested callbacks make the code hard to read and maintain. This usually happens in asynchronous JavaScript when you have to perform sequential tasks, and each task depends on the result of the previous one, leading to deeply nested callbacks.

**Key Points:**

*   It's a result of multiple nested callbacks.
    
*   Makes code hard to read and maintain (often called "pyramid of doom").
    
*   Can be avoided using Promises or async/await.
    

## **Example of Callback Hell:**

```
function getUser(userId, callback) {

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

```

In the above example, we are calling get user function, get user function Will call get orders function, and get orders function will call process payment function. So this is the problem with nested call back also known as call back hell.

## **Problems:**

*   **Hard to Read**: Code becomes more indented and harder to follow.
    
*   **Difficult to Debug**: Tracing errors can be complicated due to deep nesting.
    

## **Solutions:**

1.  **Using Promises**: Promises provide a cleaner way to handle asynchronous code.
    
2.  **async/await**: Allows writing asynchronous code that looks more like synchronous code.
    

# **Promises:**

A promise is an object which will store the result of an asynchronous operation. its value might be undefined for now, but it might change in the future.

And we will attach call back functions to this promise object instead of passing the callback function as an argument to another function. And this promise object will make sure that the promise will be resolved or rejected only once and also provide some other trust benefits.

Let's say there are two functions. One is add to cart and the other is proceeded to payment. Usually the flow would be like add to cart and then proceed to payment. So we cannot execute proceed to payment even before adding to the cart, so add to cart must be executed first, and then proceed to payment follows.

So in order to implement this, we use callbacks. now we will pass. Proceed to payment function as a call back to add to cart function, and we will add to cart function and that will call proceed to payment function, so this is how we restored the execution order with the help of callbacks. Like this

```
function addToCart( cb){

cb();

}

addToCart(function (){

proceedToPayment();

})

```

But whenever our application grows and the number of functions increase, this will lead to the problem of callback hell.

So in order to make things work without callback hell, we use promises.

```

const promise = addToCart();

promise.then(proceedToPayment())

```

In the above example, add to cart function will return a promise, and until that promise has some data in it, a default value of undefined will be stored. And once at returns, the data undefined will be replaced by that data. Now once the data is available, proceed to payment function will be called automatically by the promise object. In this way, restored the order of execution and proceeded to payment won't be called until add to cart returns data to the promise object.

```
const URL= "some url here"

function printResult(data){

    console.log(data);

}

const result=fetch(URL); // since fetch returns a promise, result will be a promise object.

result.then((data)=>printResult(data))
```

Now when the code starts executing, we will call the fetch function with some URL since fetch returns a promise, result will be a promise object, which will have various fields like promise state, promise response etc. Until the promise is resolved and some data is assigned to the promise response. The state will be pending once the data is assigned state will be updated to fulfilled and if something goes wrong and the data is not returned, then the state will be rejected.

Let's say, in our example, promise gets resolved, and the data is assigned properly to the promise response. So now we can access the data with the help of then function and whatever data we got response can be accessible inside then(). Since the promise is resolved, the promise object will now automatically call the printeResult call back function, which is inside then() along with the data we got from the API.
<img width="1512" alt="Screenshot 2024-09-26 at 9 05 43 PM" src="https://github.com/user-attachments/assets/9ae5ca12-8d61-422c-8c65-5ea8383bfca1">


In the above example, line number 3-9, includes a solution that uses callbacks. And the highlighted piece of code implements the same with the help of promises.
<img width="1512" alt="Screenshot 2024-09-27 at 5 18 17 PM" src="https://github.com/user-attachments/assets/06ea3a3c-02cb-4b6b-aacb-07c879238d4f">

In the above example, we are creating a function called create order, which will return as a promise. Now let's dive deep into how we create. Create order function, which returns a promise, we can create a new promise with the help of Promise class. So line number 13 indicates creation of a new promise. And this Promise is a constructor. And it takes a function as a call back and this callback function includes two things resolve and reject both. These are also functions. These are provided by Java script itself. Let's consider that our promise is resolved. So whatever data we want to pass to whoever called create order function we will pass that data using resolve method in line number 24. And if something goes wrong, our promise will be rejected and we can pass whatever the error with the help of rejected method in line number 19.

So when we call create order function in line number three, it will return as a promise object. And if the promise resolved the promise, object will now have the requested data, and if the promise is rejected, the promise, object will store whatever error. Let's consider the promises resolved and we got the order ID. Now the promise object will call the call back function inside then and the call back function will call. Proceed to payment function with order ID as argument. And if the promise is rejected, then control will go to the catch block.

So if your promise is resolved, control will go into the then function, and if the promise is rejected, control will go to catch function and whatever is present inside then function or catch function is then executed

# **Promise chaining:**

<img width="1512" alt="Screenshot 2024-09-27 at 6 21 10 PM" src="https://github.com/user-attachments/assets/a05ed7de-00d7-47c7-a271-cb6ffd58ca40">


The above example, create order function will return a promise. Now when the promise is resolved, then in line number four will be executed and ID will be printed to the and then we are passing order ID down the promise chain by returning it. So now line number eight can have access to order ID and we are calling proceed to payment function with order ID as argument. And proceed to payment function will also return a promise, and if that promise is resolved, control will go to line number 11, and we will print payment info to the console so that's how promise chaining works. We should pass the data around the chain if you want to access the data further in the chain.

<img width="479" alt="Screenshot 2024-09-27 at 6 28 00 PM" src="https://github.com/user-attachments/assets/b84dd0bf-07e1-4cc4-834c-8995c8d52ea9">


Catch method will only look after the then functions above it. So in this example the catch function will only look after first then function. The second and third then functions need to be looked after by another catch function at the end of them. Even if the catch executes because of some error above it, the second and third then functions will be executed. And if there is some error in second and third then functions, it will throw an error because we are not handling the error in second and third then functions.

### Promise APIs:

``` const p1=new Promise(function(resolve, reject){
    setTimeout(()=>resolve("P1 resolved"))
},1000)

const p2=new Promise(function(resolve, reject){
    setTimeout(()=>resolve("P2 resolved"))
},2000)

const p3=new Promise(function(resolve, reject){
    setTimeout(()=>resolve("P3 resolved"))
},5000)

Promise.all([p1,p2,p3]) // if all promises are resolved, it will return an array of outputs like [ 'P1 resolved', 'P2 resolved', 'P3 resolved' ]. if any one of the promise is rejected it will throw an error
.then(res=>console.log(res))

Promise.allSettled([p1,p2,p3]) // basically it returns array of output after all promises are settled(resolved or rejected) or not.
    // if all promises are resolved, it will return an array of outputs like [
    //{ status: 'fulfilled', value: 'P1 resolved' },
    // { status: 'fulfilled', value: 'P2 resolved' },
    // { status: 'fulfilled', value: 'P3 resolved' }]. if any one of the promise is rejected this is the output [
    //   { status: 'fulfilled', value: 'P1 resolved' },
    //   { status: 'fulfilled', value: 'P2 resolved' },
    //   { status: 'rejected', reason: 'P3 rejected' }
    // ]
.then(res=>console.log(res))


Promise.race([p1,p2,p3]) // whether success or failure, it will return the output of first settled(completed) promise.
.then(res=>console.log(res))

Promise.any([p1,p2,p3]) // it will always look for first success.if all promises fail, it will throw an  combined error
.then(res=>console.log(res))
```


### Async & await:

Async is a keyword that is used before a function, and that function will be treated as an async function. And async function must return a promise, and if we try to return some value, it will be wrapped inside a promise, and that promise will be returned

```
// always returns a promise
async function getData() {
return "Namaste"; // even though we are returning a value, it will be wrapped inside a promise, and that promise will be returned
｝
const dataPromise = getData();
dataPromise.then((res) → console. log(res)); // will print Namaste
```

Await(simply means wait at that line until the promise gets resolved)keyword is used before a promise or a promise object. And await keyboard can be used only inside an async function.

```
const p=new Promise(function(resolve,reject){
    resolve("promise resolved")
})

async function handleData(){
    const val1=await p;
    const val2=await new Promise(function(resolve,reject){
        resolve("another promise resolved")
    })
    console.log(val1, val2)
}

handleData() //this will print promise resolved, and another promise resolved. Here we are using a keyboard before promise object P, and we can also use await keyword before a promise as well
```

### Difference between handling promises with then() and async & await:

## Using then():

```
const p=new Promise(function(resolve,reject){
    setTimeout(()=>{
        resolve("promise resolved")
    },5000)
})

function handleData(){
    p.then(res=>console.log(res)) // we will not wait here until p resolves
    console.log("hello world")
}

handleData()
```

In the above example, hello world will be printed first and after five seconds, promise resolved will be printed. Inside handle data function, we are waiting for the promise P to be resolved, but it will take five seconds for the promise P to be resolved. Since Java script waits for none, it will move on to the next line and print, hello world. And after five seconds, the promise will be resolved and then promise resolved will be printed. This is how we used to handle promises using then().

Using async & await:

```
const p=new Promise(function(resolve,reject){
    setTimeout(()=>{
        resolve("promise resolved")
    },5000)
})

async function handleData(){
// line x    const val1=await p; // we will wait here until p resolves
    console.log("hello world")
    console.log(val1)
}

handleData()
```

In the above example, hello world and promise resolved will be printed after five seconds. When we call handle data function control will reach over. So now we will wait at line x until the promise gets resolved, and then only we will move onto the next line. So we will wait for five seconds at line x and then we will move onto next line. So after five seconds, hello world will be printed and promise resolved will also be printed

The key difference between then() and asynchronous await is that while using then() we will not wait for the promise to be resolved, and immediately we will move to the next line. And while using async await, we will wait until the promise gets resolved, and only then we will move the next line.

Some more examples:
```
const p=new Promise(function(resolve,reject){
    setTimeout(()=>{
        resolve("promise resolved")
    },5000)
})

async function handleData(){
    const res=await p; // this handleData will be suspended from call stack until p resolves(takes 5 sec), once handleData is removed fom call stack we can execute other lines of code in our program like console.log("after main")
    console.log(res)
    console.log("hello world")
}

handleData()
console.log("after main")
```
 in the above example, first we'll call handleData(), now when we see await, handleData() will be suspended from call stack and it will once p is resolved, which means handleData() will again come into call stack after 5 sec( because p takes 5 sec to resolve) and resumes its execution, since JS waits for none, during this 5 sec wait time, JS engine will execute the lines after handleData() function call. So the output will be after main, promise rsolved and bhello world will be printed after 5 sec.

```
const p=new Promise(function(resolve,reject){
    setTimeout(()=>{
        resolve("promise resolved")
    },5000)
})

 function handleData(){
    p.then(res=>console.log(res))
    console.log("hello world")
}

handleData()
console.log("after main")
```
 in the above example, when we call handleData(), if p is resolved we wil print its value, but p takes 5 sec to resolve, so instead we'll move to next line and prints hello world and there's nothing after that, so control reaches last line and prints after main and after 5 sec p is resolved, then promise resolved is printed.


 ### we cannot execute the lines after await promiseObject line until promiseObject resolves, meanwhile we can execute code which is present in global execution context.
### we can execute the lines after then() even if promise is not resolved at that moment,meanwhile we can execute the code which is present in global execution context.
## Example for above statements:
```
const p=new Promise(function(resolve,reject){
    setTimeout(()=>{
        resolve("promise resolved")
    },1000)
})

  async function handleData(){
    const res=await p;
    console.log(res)
    console.log("hello world")
}

handleData()
console.log("after main")
let a=0
for(let i=0;i<10000000000;i++){
    a+=1
}
console.log("last")
```
output will be: after main, last (after for loop completes), promise resolved, hello world. when we call handleData() control goes to await line. since p is not resolved yet we can't run the next 2 console logs.so until p resolves we will execute our remaining code in GEC. so after main will be printed. next for loop goes on and lets say it takes more time than time taken by p to resolve.so p is now resolved while our for loop is running, but we can't just print res and hello world yet, because our call stack is still filled with GEC which is currently running for loop.after loop completes, we will print last.Now our GEC will be popped oof the call stack. finally we can now place our handleData() inside call stack and resume it execution, now it will print promise resolved and hello world.
Even though our promise is resolved in 1 sec, we can't print res and hello world. because call stack is occuoied by GEC, only after call stack is empty again we can push handleData into it and execute the rest of the code.

Anothe example is if we try to remove async await and use then() the the output will be: hello world, after main, last, promise resolved.
hello world is printed first because then() allows the next lines to run even if promise is not yet resolved.and for remaining output read above reasoning(above paragraph)

```
const p=new Promise(function(resolve,reject){
    setTimeout(()=>{
        resolve("promise resolved")
    },5000)
})

async function handleData(){
    console.log("Hey")  // Hey Will be printed immediately.
    const val1=await p; // will wait for 5 sec
    console.log("hello world") // after 5 sec, hello world and promise resolved are printed
    console.log(val1)
}

handleData() 
```

```
const p=new Promise(function(resolve,reject){
    setTimeout(()=>{
        resolve("promise resolved")
    },5000)
})

async function handleData(){
    console.log("Hey") // Hey Will be printed immediately
    const val1=await p; // wait for five seconds, for the promise to be resolved
    console.log("hello world 1") // after five seconds, hello world 1 and promise resolved will be printed
    console.log(val1)
    
    const val2=await p; // we will not wait here for five seconds because promise P is already resolved above. So we will move to next line immediately.
    console.log("hello world 2") // prints hello, world 2 and promise resolved without any wait
    console.log(val1)
}

handleData() // hey will be printed and after five seconds, there will be four console logs, namely hello world1, promise resolved, hello world 2, promise resolved.
```

```
const p1=new Promise(function(resolve,reject){
    setTimeout(()=>{
        resolve("promise resolved")
    },10000)
})

const p2=new Promise(function(resolve,reject){
    setTimeout(()=>{
        resolve("promise resolved")
    },5000)
})

async function handleData(){
    console.log("Hey") // prints Hey immediately
    const val1=await p1;  // waits for 10 sec
    console.log("hello world 1") // after 10 sec, hello world 1 and promise resolved is printed
    console.log(val1)
    
    const val2=await p2; // we don’t need to wait here, because p2 is already resolved by the time p1 gets resolved. Because p1 takes 10 secs and p2 takes <10 sec
    console.log("hello world 2") // even though p2 is resolved in 5 sec, hello world 2 and promise resolved will be printed after 10 seconds.
    console.log(val1)
}

handleData() // it will print hey immediately, and after 10 seconds, there will be four console logs, namely, hello world 1, promise resolved, hello world 2, promise,resolved
```

Now let's change the timers, P1 takes five seconds and P2 takes 10 seconds

```
const p1=new Promise(function(resolve,reject){
    setTimeout(()=>{
        resolve("promise resolved")
    },5000)
})

const p2=new Promise(function(resolve,reject){
    setTimeout(()=>{
        resolve("promise resolved")
    },10000)
})

async function handleData(){
    console.log("Hey") // Hey will be printed immediately
    const val1=await p1; // will wait for five seconds
    console.log("hello world 1") // after five seconds, hello world 1 and promise resolved will be printed
    console.log(val1)
    
    const val2=await p2; // by the time, P1 resolves, P2 also spent five seconds waiting, now P2 is left with another five seconds for it to be resolved
    console.log("hello world 2") // after waiting another five seconds, hello world 2 and promise resolved will be printed
    console.log(val1)
}

handleData() // this will print Hey immediately and after five seconds, hello world 1 and promise resolved will be printed. and after another five seconds, hello world 2 and promise resolved will be printed
```
    
We are telling that we are waiting at a certain line until the promise gets resolved. But we know that Java script doesn't wait for anything, so how is this waiting possible then?.

When we say that we are waiting at a certain line, we doesn't actually mean it. So Java script engine doesn't wait until a promise is resolved. If we wait until a promise is resolved, we are blocking the main thread which we shouldn't do in Java script. So instead, Java script does something which resembles that we are waiting at a certain line.
So whenever our function handle data goes into call stack, it starts its execution. But when it encounters await, the whole function will now be suspended, which means it will be taken out of the call stack.And once handleData is popped we can execute the rest of the code in our GEC.and once GEC is completed and popped off the stack,we'll check if our promise is resolved or not, if it is resolved then we'll put handleData() in call stack and execute it. even if promise is resolved before completely executing GEC, we can't run the promise. Because by the time promise resolved, call stack is still busy running the code of GEC. SO only after popping GEC we can push our handleData() into call stack and resume the execution

### E-commerce example using async & await:

```
const createOrder= async (product)=>{
    return new Promise((resolve, reject)=>{
        setTimeout(()=>{
            resolve(product+" order created")
        },2000)
    })
}

const proceedToPayment= async (orderData)=>{
    return new Promise((resolve, reject)=>{
        setTimeout(()=>{
            orderData.includes("Iphone")?resolve("payment success"): reject("payment failed")
        },3000)
    })
}

const orderSummary= async (product, paymentInfo)=>{
    return new Promise((resolve, reject)=>{
        setTimeout(()=>{
            paymentInfo.includes("success")?resolve("Here's the order summary of "+product): reject("Something went wrong")
        },3000)
    })
}
const buyProduct=async (product)=>{
    const orderData= await createOrder(product)
    const paymentInfo= await proceedToPayment(orderData)
    const orderSummaryData= await orderSummary(product, paymentInfo)
}

const product="Iphone"
buyProduct(product)
```
* createOrder: Logs "Iphone order created" after 2 seconds.
* proceedToPayment: Logs "payment success" after an additional 3 seconds.
* orderSummary: Logs "Here's the order summary of Iphone" after an additional 3 seconds.


### Debouncing
Whenever I usually typing something, many API calls will happen. For example, if a user is typing pizza and API call will happen with P. Then another API call will happen with PI and then another API call will happen with PIZ and this goes on. So this isn't a good practice making so many API calls. This is where the bouncing comes into picture, which will reduce the number of API calls.

The way de bouncing works is, we will call the API only after the user has stopped typing. We won't call the API if the user is still typing. This will make sure that a single API call is happening.

## Execution:
```
let timeout
function debouncedFun(){
    clearTimeout(timeout)
    setTimeout(()=>{
        actualFun()
    }, 1000)
}

function actualFun(){
    // making the api call here
}

<input onInput/onChange=debouncedFun()/>
```
First, when the user is typing, we will call the debounced function, and that debounced function will call the actual function after one second, which will make the API call. But when the user is typing debounced function will be called multiple times, but until the user stops typing, we should not call the actual function. So we will create a global timer variable called timeout. Whenever debounced function is called multiple times, The previous timer will be cleared and a new timer will be set. This way. We can call the actual function after one second, once the user stopped typing.
So now, if the user stop typing and once again have passed, then the actual function will be called. This way, we can reduce the total number of API calls.
