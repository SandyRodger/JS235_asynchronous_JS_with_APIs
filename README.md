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

1.
2.
3.
4.
5.

### [9	Error Handling with Callbacks](https://launchschool.com/lessons/1a2b6504/assignments/7c17530f)

- error-first callbacks

#### A complete example

### 10	Practice Problems: Error Handling with Callbacks
### 11	The Limitations of Callbacks
### 12	Summary
### 13	Quiz

## 2	Promises and Async / Await
## 3	Making HTTP Requests from JavaScript
## 4	Putting it All Together
