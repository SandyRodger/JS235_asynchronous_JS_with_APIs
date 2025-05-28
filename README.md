# [JS235_asynchronous_JS_with_APIs](https://launchschool.com/courses/23e92be8)

## [1	Asynchronous Programming with Callbacks](https://launchschool.com/lessons/1a2b6504)


### [1	Introduction](https://launchschool.com/lessons/1a2b6504/assignments/5844b58f)
### [2	What is Asynchronous Programming?](https://launchschool.com/lessons/1a2b6504/assignments/8946cd6c)
### [3	Asynchronous Execution with setTimeout](https://launchschool.com/lessons/1a2b6504/assignments/5a0b1ee6)  
#### problems:

1.
```javascript
function createLogger(n) {
  return function() {
    console.log(n);
  }
}

function delayLog() {
  for (let i = 1; i <= 10; i++) {
    let logger = createLogger(i);
    setTimeout(logger, i * 1000);
  }
}

delayLog();
```

2. correct

```javascript
setTimeout(() => { // 1
  console.log('Once'); // 5
}, 1000);

setTimeout(() => { // 2
  console.log('upon'); // 7
}, 3000);

setTimeout(() => { // 3
  console.log('a'); // 6
}, 2000);

setTimeout(() => { // 4
  console.log('time'); // 8
}, 4000);
```

3. my answer : f, g, d, z, n, s, q
     - correct except first two. Even though `f()` exectures immediately it will still wait until the next event cycle.

4. nailed it

```javascript
function afterNSeconds(callback, secs)  {
  setTimeout(callback, secs * 1000)
}

function c() {
  console.log('foo');
}

afterNSeconds(c, 10);
```


### [4	Repeating Execution with setInterval](https://launchschool.com/lessons/1a2b6504/assignments/d93e4c24)


#### Problems:

1. 
```javascript
function startCounting() {
  let n = 0;
  setInterval(() => console.log(n += 1), 1000)
}
```

2. 
```javascript
function startCounting() {
  let count = 0;
  return setInterval(() => {
    count += 1;
    console.log(count);
  }, 1000);
}

let id = startCounting()

function stopCounting(token) {
  clearInterval(token);
}

stopCounting(id)
```
  
### [5	What is the Event Loop?](https://launchschool.com/lessons/1a2b6504/assignments/2da1ba17)

- YT video I need to unblock
- [article](https://blog.bitsrc.io/understanding-asynchronous-javascript-the-event-loop-74cd408419ff)
  -  Javascript is a single-threaded programming language
    -  you needn't worry about concurrency issues
    -  therefore it has a single call stack.
  -  Asynchronous JS:
    -  callbacks
    -  promises
    -  async/wait

  - DOM events
  - Message queue
  - ES6 Job Queue/ Micro Task Queue

### [6	Managing Async Operations: An Overview](https://launchschool.com/lessons/1a2b6504/assignments/91d3619f)

- How to manage tasks that take time wihtout blocking Javascript's single-thread( ie not sitting in front of the laundry machine):
  - like fetching data from the server ("fetchez la vache")
- three main tools:
  - Callbacks
  - Promises
  - Async/Await

### [7	Callbacks](https://launchschool.com/lessons/1a2b6504/assignments/efeae132)

###### A simple example

- hmmmmm I know what callbacks are. why am i reading this?

#### What setTimeout Represents

### [8	Practice Problems: Callbacks](https://launchschool.com/lessons/1a2b6504/assignments/69007cc1)

1. My solution is the same as LS

```javascript
function basicCallback(callBack, num) {
  setTimeout(() => callBack(num), 2000);
}
```

2. yup, pretty easy.
```javascript
function downloadFile(cb) {
  console.log("Downloading file...");
  setTimeout(() => cb("Download complete!"), 1500);
}

downloadFile((message) => console.log(message));
```
3.
```javascript
function processData(arr, cb1) {
  setTimeout(() => console.log(arr.map((n) => cb1(n))), 1000);
}
```

4. easy
```javascript
function waterfallOverCallbacks(cbArr, n) {
  cbArr.forEach((cb) => n = cb(n))
  console.log(n);
}
```

5. This one I peeked.
```javascript
function startCounter(cb) {
  let counter = 0
  const intervalId = setInterval(() => {
    counter++;
    if (cb(counter)) {
      clearInterval(intervalId);
    }
  }, 1000);
}

startCounter((count) => {
  console.log(count);
  return count === 5;
});
```

- lesson: in order to clear an interval you must collect the return value of `setInterval` which is a unique identifier. This identifier is the argument you pass into `clearInterval`.

### [9	Error Handling with Callbacks](https://launchschool.com/lessons/1a2b6504/assignments/7c17530f)

- "established patterns like error-first callbacks" whatever that means
- but also "You don't need to master error handling with callbacks"

#### A complete example

### 10	Practice Problems: Error Handling with Callbacks

- urgh, this was a blow to my confidence.

### [11	The Limitations of Callbacks](https://launchschool.com/lessons/1a2b6504/assignments/1ce548f8)

##### Avoiding Callback Hell

##### Breaking Out of the Pyramid

- modularize
- use named functions

##### The Limits of Callbacks

- Readability: Deeply nested callbacks are hard to read.
- Error handling: It's easy to miss errors that are not properly managed at every level of the callback chain.
- Control flow: Managing the order of execution can become very challenging.

### [12	Summary](https://launchschool.com/lessons/1a2b6504/assignments/ac34c3eb)

- yup
- setTimeout() yup
- setInterval yup
- yup
- callbacks yup
- error handling yup
- callback hell yup


### 13	Quiz

- 1st go (28.5.25) 4/6

### [2	Promises and Async / Await](https://launchschool.com/lessons/701aaae0/assignments)

- three states:
  - pending
  - fulfilled
  - rejected

#### Creating a Promise

- a promise is an object that represents the eventual completion of an asynchronous task.

#### Using then()

#### Managing Asynchronous Operations with Promises



### [3	Practice Problems: Promise Basics](https://launchschool.com/lessons/701aaae0/assignments/d1860c94)
### 4	Chaining Promises	
### 5	Error Handling with Promises	
### 6	Practice Problems: Error Handling with Promises	
### 7	Assignment: Updating User Information App with Promises	
### 8	Promise API	
### 9	Practice Problems: Promise API	
### 10	Async / Await	
### 11	Practice Problems: Async / Await	
### 12	Error Handling with Async / Await	
### 13	Practice Problems: Error Handling with Async / Await	
### 14	Assignment: Updating User Information App with `async` / `await`	
### 15	Combining Async / Await with Promise Methods	
### 16	Summary	
### 17	Quiz

## 3	Making HTTP Requests from JavaScript
## 4	Putting it All Together
