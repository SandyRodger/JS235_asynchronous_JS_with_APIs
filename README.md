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

```javascript
let myPromise = new Promise((resolve) => {
  setTimeout(() => {
    resolve("hello");
  }, 1000)
});

myPromise.then((result) => {
  console.log(`Got a result: ${result}`);
});

console.log('Finished setting up!');
```

#### Managing Asynchronous Operations with Promises

```javascript
const users = {
  1234: { name: 'Aisha Patel', age: 29 },
  5678: { name: 'John Smith', age: 35 },
  9101: { name: 'Susan Green', age: 42 },
};

const passwords = {                             // WARNING: THIS IS NOT HOW
  Aisha: { password: 'password123', id: 1234 }, // PASSWORDS ARE STORED
  John: { password: 'secret', id: 5678 },
  Susan: { password: 'Green83', id: 9101 },
};

function authenticate(username, password, callback) {
  setTimeout(() => {
    // 10% chance of an unknown server error
    if (Math.random() < 0.1) {
      return callback('Something went wrong. Please try again later.', null);
    }

    if (passwords[username] && passwords[username].password === password) {
      callback(null, passwords[username].id);
    } else {
      callback('Invalid username or password', null);
    }
  }, 1000);
}

function fetchUserProfile(id, callback) {
  setTimeout(() => {
    // 10% chance of an unknown server error
    if (Math.random() < 0.1) {
      return callback('Something went wrong. Please try again later.', null);
    }

    // Normal behavior: check if user exists
    let userData = users[id];
    if (userData) {
      callback(null, userData);
    } else {
      callback('User not found', null);
    }
  }, 2000);
}

function retryNTimes(fn, n, callback, ...args) {
  let attempts = 0;

  function attempt() {
    console.log(`Attempt number: ${attempts + 1}`);
    fn(...args, (error, data) => {
      if (!error || attempts >= n) {
        callback(error, data);
      } else {
        attempts++;
        attempt();
      }
    });
  }

  attempt();
}
```

```javascript
washLaundry().then(() => {
  console.log("Folding Laundry.");
  console.log("Putting away Laundry.");
})
```

### [3	Practice Problems: Promise Basics](https://launchschool.com/lessons/701aaae0/assignments/d1860c94)

1.
```javascript
function bakeCookies() {
  console.log("Mixing ingredients.");
  console.log("Scooping cookie dough.");
  console.log("Putting cookies in oven.");
  
  console.log("Baking...");
  return new Promise((resolve) => {
    setTimeout(() => {
      console.log("Beep!");
      resolve();
    }, 3000)
  });
}

let bakingPromise = bakeCookies();

bakingPromise.then(() => {
  console.log('Take out cookies');
  console.log('Leave cookies to cool');
});
```

2.  Ok I didn't quite get this one. In my defence, the instructions are unclear. But also I haven't really internalised this.

3. The return value of the function (as LS write it) is `Promise <pending>` and I'm not sure waht to do with that. Especially as "Download complete!" is not printed.

```javascript
function downloadFilePromise() {
  return new Promise((resolve) => {
    console.log("Downloading file...");
    setTimeout(() => {
      resolve("Download complete!");
    }, 1500);
  });
}

downloadFilePromise(); // Promise { <pending> }
```
4. I got the exact right answer, but don't know what it means or how it works. Not very Launch-school

```javascript
function processDataPromise(numbers, callback) {
  return new Promise((resolve) => {
    setTimeout(() => {
      const processed = numbers.map(callback);
      resolve(processed);
    }, 1000);
  });
}
```

### [4	Chaining Promises](https://launchschool.com/lessons/701aaae0/assignments/cbc0f690)

- So far it's been promise resolved by `then` once the promise resolves. Now we're going to make these an asynchronous sequence.

```javascriipt
let myPromise = new Promise((resolve) => {
  setTimeout(() => {
    resolve("Hello");
  }, 1000)
});

let thenReturnValue = myPromise.then((result) => {
  return `The result is: ${result}`;
});

console.log(thenReturnValue);
```

#### a promise chain

```javascript
new Promise(function(resolve) {
  setTimeout(() => resolve(1), 2000);
})
  .then(function (result) {
    console.log(result);
    return result * 2;
  })
  .then(function(result) {
    console.log(result);
    return result * 2;
  })
  .then(function(result) {
    console.log(result);
    return result * 2;
  });
```

```javascript
let email = 'example@email.com';

checkfEmailExists(email)
  .then(storeUserDetails)
  .then(sendVerificationEmail)
  .then(() => {
    console.log("user signup process completed!");
  })
```

#### A common gotcha

- be careful with what exactly your promise is returning

### [5	Error Handling with Promises](https://launchschool.com/lessons/701aaae0/assignments/e79150ab)

#### reject and .catch()



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
