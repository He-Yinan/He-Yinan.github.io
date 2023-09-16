---
layout: post
title:  "Asynchronous JavaScript"
date:   2023-02-21 14:35:24 +0800
categories: jekyll update
tags: javascript programming-language
---
Asynchronous programming allow operations to be non-blocking and can be executed in a non-sequential fashion. Asynchronous programming can prevent time consuming operation to block further execution until the operation is completed. 

Considering the fact that JavaScript is a single-threaded language, asynchronous programming is very useful when there exist time-consuming functions. However, asynchronous functions may be a pain in the neck in many cases. For example, the program may become unstable and error prone when asynchronous functions are not handled properly and multiple functions must execute sequentially. To manage asynchronous functions, we need to understand quite a few concepts:

1.  Callback
2.  Promise
3.  Async
4.  Await

## Callback

A callback function is a function passed as an argument to another funtion.

Example:

```javascript
function sumLogger(ans) {
    console.log('Sum: ' + ans);
}

function adder(num1, num2) {
    return num1 + num2
}

function calculator(num1, num2, operator, displayer) {
    let ans = operator(num1, num2);
    displayer(ans);
}

calculator(3, 5, adder, sumLogger);
```

`adder` is first called to compute the sum, then `answerLogger` is called to display the sum. 

To calculate subtraction, we do not need to change the code inside `calculator`. All we need to do is to write other functions like `diffLogger` and `subtractor` and change the way we call `calculator` to `calculator(num1, num2, subtractor, diffLogger)`. We do not need to change any existing code when we add a new feature. This is crucial in writing flexible code. 

The above example is a synchronous callback as it is executed sequentially. Callbacks can also be used to continue code execution after an asynchronous operation has completed (asynchronous callbacks).

## Promise

A `promise` is an object that represents the eventual completion or failure of an asynchronous operation. 

A JavaScript Promise object can be in one of three states:
1.  Pending: initial state. (result is undefined)
2.  Fulfilled: operation was completed successfully. (result is a valid value)
3.  Reject: operation failed. (result is an error object)

### Creation

Syntax to create a promise object:

```javascript
let promise = new Promise((resolve, reject) => {
    setTimeout(() => {
        let x = 1 + 1;
        if(x === 2) {
            resolve("Done!");  // when successful
        } else {
            reject("Failed");  // when error occur
        }
    },
    3000);
});

promise.then((result) => {
    // fulfillment
    console.log(result);
}, (error) => {
    // rejection
    console.log(error);
});
```

The `resolve()` and `reject` are two predefined functions that we do not need to define. `resolve()` is called when `setTimeout()` is executed according to expectation. `reject()` is called when error occurs.

The two parameters in `promise.then()` are two anonymous callback functions. The first callback function is executed when `resolve()` function is called (`onFulfilled`). The parameter passed into the `resolve()` function corresponds to the `result`. The second callback function is executed when `reject()` function is called (`onRejected`).

## Async

The keyword `async` can be placed before a function like this:

```javascript
async function foo() {
    return "Hello World";
}
```

Functions with `async` will always return a promise. For example, the `foo()` function is exactly the same as the function below:

```javascript
async function foo() {
    return Promise.resolve("Hello World");
}

foo().then((message) => {
    console.log(message);
})
```

Note that the returned message can be accessed using the `then()` function.

The combination of `async` keyword and `then()` function can already solve the problem asynchronous functions. Code that depends on asynchronous code can be put inside the `then()` function to ensure that the asynchronous code has already finished executing before the subsequent code are executed. However, it is quite troublesome to use the `then()` method. In many cases, the `await` keyword can help to make our code much more elegant.

## Await

The keyword `await` can **only be used in async functions**. It forces the code to wait until the promise is in a fulfilled state. Let's take a look at an example:

```javascript
async function timeout() {
    let promise = new Promise((resolve) => {
        setTimeout(() => resolve("Done!"), 10000)  // timeout for 10 seconds
    });

    console.log("Starting to block");
    let result = await promise;  // !
    console.log("Result out");
    console.log(result);
}
timeout();
```

The result printed in the console is as shown below:

```shell
Starting to block
Result out  # after 10 seconds
Done!  # after 10 seconds
```

The function execution pauses at `!` line and resumes after 10 seconds timeout. The `await` keyword makes sure `promise` is fulfilled and `result` is assigned `"Done!"` value before the code continues to execute. 

What happens without the `await` keyword? The console will print the following instead:

```bash
Starting to block
Result out  # printed immediately
Promise { <pending> }  # printed immediately
```

Note that the code did not pause at `!` line. This cause `result` to be a pending promise when `console.log(result)` is called.
