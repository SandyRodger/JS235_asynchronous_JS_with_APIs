# [JS235_asynchronous_JS_with_APIs](https://launchschool.com/courses/23e92be8)

## [1	Asynchronous Programming with Callbacks](https://launchschool.com/lessons/1a2b6504)

- laundry analogy. Wash up whle the clothes wash. yup

#### Why Do We Need Asynchronous Code?

- (5.6.25) I've read this page twice, there's nothing I need to remember on it.

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

- 2nd pass (5.6.25)
  1.
```javascript
function delayLog(n) {
  console.log(n);
  setTimeout(() => {
    if (n < 10) {
      delayLog(n+1)
    }
  }, 1000)
}

delayLog(1);
```

2. 1, 5, 9, 13, 2, 10, 6, 14 - correct
3. 
4. 
```javascript
function afterNseconds(callback, timeDuration) {
  setTimeout(()=> {
    callback()
  }, timeDuration)
}

afterNseconds(() => console.log('yes'), 3000)
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

  - 2nd pass (5.6.25):

1. yup
```javascript
function startCounting() {
  let n = 0;
  setInterval(() => {
    n += 1
    console.log(n)
  }, 1000)
}

startCounting();
```

2.
```javascript
function startCounting(n=0) {
let intId = setInterval(() => {
    n < 10 ? console.log(n+=1) : clearInterval(intId);
  }, 1000)
}

startCounting();
```
3.
```javascript
function weirdLog(counter = 0) {
  console.log('Starting...');
  let stop = setInterval(() => {
    if (counter < 6) {
      console.log("Hello!");
    } else {
      console.log("Goodbye!")
      clearInterval(stop);
    }
    counter += 1;
  }, 2000)
}

weirdLog();
```

I like LS solution:


console.log("Starting...");

const intervalId = setInterval(() => {
  console.log("Hello!");
}, 2000);

setTimeout(() => {
  clearInterval(intervalId);
  console.log("Goodbye!");
}, 10000);

### [5	What is the Event Loop?](https://launchschool.com/lessons/1a2b6504/assignments/2da1ba17)

- YT video:
  - what are you Javascript?
  - v8?
  - callstack and heap?
  - why callbacks? Why so different to other languages? How does this run-time work?
  heap: where memory allocation happen
  call-stack: where the stack frames are
  - grep - should I be better at grep?
  - single thread: it can do one thing at a time.
  - callstack keeps track of where we are.
  - main function is the file itself
  blocking = things that take a long time and block the stack.
  concurrency and the event loop (yes JS can only do one thing at a time, but… the browser is not the javascript runtime.)
  to shim? "I haven't shimmed XHR yet" he says.
  render,,,?
  scroll handlers… every 16 ms...
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

- How to manage tasks that take time without blocking Javascript's single-thread( ie not sitting in front of the laundry machine):
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

- "established patterns like error-first callbacks" whatever that means.
- but also "You don't need to master error handling with callbacks".

#### A complete example

### 10	Practice Problems: Error Handling with Callbacks

(2nd try 9.6.25)
1. OK, i did this successfully
2. 

```javascript
document.getElementById('userForm').addEventListener('submit', function (event) {
  event.preventDefault();

  const userInfoDiv = document.getElementById('userInfo');
  const errorMessageDiv = document.getElementById('errorMessage');

  userInfoDiv.textContent = '';
  errorMessageDiv.textContent = '';

  const username = document.getElementById('usernameInput').value.trim();
  const password = document.getElementById('passwordInput').value.trim();

  authenticate(username, password, (errorMessage, userId) => {
    if (userId) {
      userInfoDiv.textContent = 'Login Successful! Fetching your data...';

      fetchUserProfile(userId, (errorMessage, userData) => {
        if (errorMessage) {
          errorMessageDiv.textContent = `Error: ${errorMessage}`;
          errorMessageDiv.style.display = 'block';
        } else {
          userInfoDiv.textContent = `Hello, ${userData.name}!\nYou are ${userData.age} years old!`;
        }
      })

    } else {
      errorMessageDiv.textContent = errorMessage;
      errorMessageDiv.style.display = 'block';
    }
  });
});
```

key take-aways:
  - in the following line we can access what the user just typed in, even though we're not accessing the `event` or `currentTarget` or `target`. This is because the `value` property of an input field is updated in real time as the user types, not just when the form is submitted.
    - `const password = document.getElementById('passwordInput').value.trim();`
  - I found this code difficult to follow and write because it's hard to keep track of the callbacks and their return values. all in all it made me want to shoot myself in the eye. let's move on.

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

- 1st go (28.5.25) 4/6 (1, 2, 3, 4)
  - 5 & 6 wrong
- 2nd go (9.6.25) 4/6 (1, 2, 4, 6)
  - 3 & 5 wrong
1. B tick
2. C tick
3. A I will say yes because even though we have not completed the `washLaundry` function when we begin the `bakeCookies` function, we have completed the two console.logs that comprise the `washLaundry` activity.
B
Not C because we do not invoke the `afterCookies()` function.
Not D because I don't know what the `washLaundry` function does.
  - correct except for this. D does fulfill the conditions of the question because it is identical to A, except that we're using named functions. The mistake I made was not reading the question closely enough. The question states that the excerpts are updating the code above, so we do indeed know what the `bakecookies   and `washLaundry` functions do.
4. B - tick
5. A 
  - and B: I know this. This was a careless oversight.
6. D - tick


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
4. 2nd pass - got the right answer and I know why it works now.

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

```javascript
let myPromise = new Promise((resolve, reject) => {
  setTimeout(() => {
    reject(new Error("Something went wrong."));
  }, 1000)
});

myPromise.catch((errorMessage) => {
  console.log(`Error message: ${errorMessage}`);
})
```

- we can pass any value to `reject` but it's only supposed to take errors.

```javascript
function getStatus() {
  const rand = Math.random();

  return new Promise((resolve, reject) => {
    setTimeout(() => {
      if (rand > 0.4) {
        resolve(200);
      } else if (rand > 0.2) {
        reject(404);
      } else {
        reject(500);
      }
    }, 2000)
  })
}

const statusPromise = getStatus();

statusPromise
  .then(successCode => {
    console.log(`Success! Status code: ${successCode}`);
  })
  .catch(failureCode => {
    console.log(`Failed. Status code: ${failureCode}`);
  })
```

#### Error propagation in Promises

- `catch` should always be included because it catches everything, errors and rejects.

#### Recovering from Errors

- `.catch()` can, as well as loggin errors, be used to recover from errrors by returning a new value/promise. FOr example:

```javascript
doSomethingAsync()
  .then((result) => doSomethingElseAsync(result))
  .catch((error) => {
    console.error("An error occured in the first two steps:", error);
    return fallbackValue;
  })
  .then((newResult) => doThirdThingAsync(newResult));
```

- Just like with `then` any return value from `catch` will be wrapped in a promise.

#### `.finally()`

- this method is always called regardless of whether the promise was fulfilled or not.

```javascript
statusPromise
  .then(successCode => {
  console.log(`Success! Status code: ${successCode}`);
  })
  .catch(failureCode => {
    console.log(`Failed. Status code: ${failureCode}`);
  })
  .finally(() => {
    console.log(`All settled!`);
  })
```
#### A different way to use `then`

```javascript
const statusPromise = getStatus();

statusPromise
  .then(
    (successCode) => console.log(`Success! Status code: ${successCode}`),
    (failureCode) => console.log(`Failed. Status code: ${failureCode}`)
  )
```

- don't use this way. (why am I learning it? -> because you may see it written this way by others).

#### `Promise.resolve` and `Promise.reject` Static Methods

```javascript
let promiseA = Promise.resolve(42);

let promiseB = new Promise((resolve, reject) => {
  resolve(42);
})

console.log(promiseA); // Promise {<fulfilled>: 42}
console.log(promiseB); // Promise {<fulfilled>: 42}
```

```javascript
let promiseA = Promise.reject('Oh no!');

let promiseB = new Promise((resolve, reject) => {
  reject('Oh no!');
})

console.log(promiseA); // Promise {<rejected>: 'Oh no!'}
console.log(promiseB); // Promise {<rejected>: 'Oh no!'}
```
### [6	Practice Problems: Error Handling with Promises	](https://launchschool.com/lessons/701aaae0/assignments/05f3855c)

1. OK, my beef here is that the question stipulates: "Create a promise called flakyService that simulates a service that sometimes fails.", but the answer is a **function** called 'flakyService' which returns a promise. Not the same thing.

```javascript
function flakyService() {
  return new Promise((resolve, reject) => {
    if (Math.random() > 0.5) {
      resolve("Operation successful");
    } else {
      reject("Operation failed");
    }
  });
}

flakyService().then(console.log).catch(console.error);
```

2.

```javascript
function operation() {
  return new Promise((resolve) => {
    console.log("Operation started");
    setTimeout(() => {
      resolve("Operation complete");
    }, 1000);
  });
}

operation()
  .then(console.log)
  .finally(() => {
    console.log("Cleaning up resources...");
  });
```

3. weird
```javascript
Promise.resolve(7)
  .then((number) => number * 2)
  .then((number) => number + 5)
  .then((result) => console.log(result));
```

4. 
```javacsript
function fetchUserData() {
  return new Promise((resolve, reject) => {
    setTimeout(() => 
      reject({error:"User not found" }
    ), 500);
  });
}

fetchUserData()
  .catch((error) => console.error(error.error))
  .finally(() => console.log("Fetching complete"))
```

5. 
```javascript
function retryOperation(operationFunc) {
  let attempts = 0;
  function attempt() {
    return operationFunc().catch((err) => {
      if (attempts < 2) {
        attempts++;
        console.log(`Retry attempt #${attempts}`);
        return attempt();
      } else {
        throw err;
      }
    });
  }

  return attempt().catch(() => console.error("Operation failed"));
}


retryOperation(
  () =>
    new Promise((resolve, reject) =>
      Math.random() > 0.33
        ? resolve("Success!")
        : reject(new Error("Fail!"))
    )
);
```

7. 
```javascript
function loadData() {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      if (Math.random() > 0.5) {
        resolve("Data loaded");
      } else {
        reject("Network error");
      }
    }, 1000);
  }).catch(() => {
    console.error("An error occurred. Attempting to recover...");
    return Promise.resolve("Using cached data");
  });
}

loadData().then(console.log);
```
  SECOND PASS: I could still solve hardly any of this. (10.6.25)

### [7	Assignment: Updating User Information App with Promises	](https://launchschool.com/lessons/701aaae0/assignments/934f5046)

1.
```javascript
function authenticate(username, password) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      if (Math.random() < 0.1) {
        reject('Something went wrong. Please try again later.');
      }
  
      if (passwords[username] && passwords[username].password === password) {
        resolve(passwords[username].id);
      } else {
        reject('Invalid username or password');
      }
    }, 1000);
  })
}

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

authenticate('Susan', 'Green83')
  .then(console.log)
  .catch(console.error)
  .finally('and some cows');
```
- errors:
  - the argument to `reject` should be a `new Error()`
  - even though we are returning the main `Promise` of the function, we call return with the `reject` statement in order to escape the funtion early.
- Other than that I was pretty close!

2. Boy, I hope we don't have to write documentation in the assessment.
3. I can't find the `userId` I'm peekin' (also I wasn't sure how to disentangle `retryNTimes` from the code
    - aha - userId is the return value of a successsful `authenticate` call.
    - It was simpler than I was building it up to be. I just had to replace the `retryNTimes` with `authenticate`.
4. OK I peeked here as well. Key points:
     - the `retryNTimes` function is not only for `authenticate`, so it takes a `fn` argument and a `...args` argument.
5. Almost! I just missed that we are not only retrying `authenticate`, but also `fetschUserProfile`
### [8	Promise API	](https://launchschool.com/lessons/701aaae0/assignments/db7a742f)

- provides methods to use with promises.

#### `promise.all()`

- takes an iterable collection of promises and returns a single promise.
- fulfills if they all fulfill, rejects if even one rejects.
- returns an array of the resolve values in order.

#### `promise.race()`

- Takes an iterable of promises.
- takes the first promise to complete and dumps the rest. If this first promise resolves -> it resolves, if it rejects, it rejects.

- concise setTimeout: `setTimeout(resolve, 100, "two");` where the third argument is the arg passed to `resolve`

#### `promise.allSettled()`

- returns a promise that resolves when all inpout promises are fullfilled/rejected
- returns an array of objects descriing the outcomes.
- So it's like Promise.all, but witth a complete picture regardless of rejects.

```javascript
let p1 = Promise.resolve(3);
let p2 = new Promise((resolve, reject) => setTimeout(reject, 1000, "foo"));
let ps = [p1, p2];

Promise.allSettled(ps).then((result) => {
  result.forEach((r) => console.log(r));
});
```

#### `promise.any()`

- takes an iterable of promises and returns a single promise as soon as one of those promises is fulfilled
- If they all fail it returns an `aggregateError`, a kind of error that groups together individual errors.

### [9	Practice Problems: Promise API](https://launchschool.com/lessons/701aaae0/assignments/91f24c6e)

2. nailed it
```javascript
const firstResource = new Promise((resolve) =>
  setTimeout(() => resolve("First resource loaded"), 500)
);
const secondResource = new Promise((resolve) =>
  setTimeout(() => resolve("Second resource loaded"), 400)
);

Promise.race([firstResource, secondResource]).then((result) => {
  console.log(result);
})
```

3. nailed it

```javascript
const services = [flakyService(), flakyService(), flakyService()];
Promise.allSettled(services).then((result) => {
  result.forEach((r) => console.log(r));
})
```

4. nailed it.
```javascript
const services = [flakyService(), flakyService(), flakyService()];
Promise.any(services)
  .then((v) => console.log(v))
  .catch((v) => console.error(v));
```

5. kinda -> i guessed the race bit, but got muddled up with syntax.
```javascript

```

### [10	Async / Await](https://launchschool.com/lessons/701aaae0/assignments/3ce8eb42)

- this is mere syntactical sugar for promises.
  
#### async Functions

- yup

#### await Keyword

- omg
- ok (slightly rushed)

#### Using await Without a Promise

- JS will just wrap the value in a fulfilled promise.
- once the await promise is fulfilled it pushes the rest of the function onto a microtask queue and gets on with the rest of the code? Seems illogical to me.

#### Handling Multiple Asynchronous Tasks



### [11	Practice Problems: Async / Await](https://launchschool.com/lessons/701aaae0/assignments/aa5dfac0)

1. Yup (11.6.25)
2. Yup, solved. I used recursion, they used a for loop
```javascript

```
3. Interesting mistake with passing an unresolved resolve
```javascript
function brushTeeth() {
  return new Promise((resolve) => {
    console.log("brush teeth...");
    setTimeout(resolve, 1000);
  });
}

function eatBreakfast() {
  return new Promise((resolve) => {
    console.log("eating breakfast...");
    setTimeout(resolve, 1000);
  })
}

function shower() {
  return new Promise((resolve) => {
    console.log("showering...")
    setTimeout(resolve, 1000);
  })
}

async function getReady() {

  console.log("good morning!")

  await brushTeeth();
  await eatBreakfast();
  await shower();

  console.log("I'm ready!")
}

getReady();
```
4. Might need a little practice here...
```javascript
function brushTeeth() {
  return new Promise((resolve) => {
    console.log("brush teeth...");
    setTimeout(resolve, 1000);
  });
}

function eatBreakfast() {
  return new Promise((resolve) => {
    console.log("eating breakfast...");
    setTimeout(resolve, 1000);
  })
}

function shower() {
  return new Promise((resolve) => {
    console.log("showering...")
    setTimeout(resolve, 1000);
  })
}


async function makeCoffee() {
  console.log("putting on the coffee")
  await new Promise((resolve) => setTimeout(resolve, 6000));

  console.log('Coffee ready!');
}

async function getReady() {

  console.log("good morning!")

  await brushTeeth();
  await eatBreakfast();
  await shower();

  console.log("I'm ready!")
}

makeCoffee();
getReady();
```

### [12	Error Handling with Async / Await](https://launchschool.com/lessons/701aaae0/assignments/49ba5b39)
- ok
#### Error Propagation
- like synchronous functions uncaught errors in async functons propagate up the call chain. 
- frankly I'm not sure I could explain what that means.

#### Catching Errors from Multiple Operations

```javascript
async function multipleOperations() {
  try {
    const data1 = await operation1();
    const data2 = await operation2(data1);
    const data3 = await operation3(data2);
    return data3;
  } catch (error) {
    console.error("An error occured:", error.message);
  }
}
```

#### Catching Specific Errors

```javascript
async function handleRiskyOperation() {
  try {
    return await riskyOperation();
  } catch (error) {
    if (error instanceof DataFetchError) {
      console.error("Failed to fetch data:", error.message);
    } else if (error instanceof AuthorizationError) {
      console.error("Authorization failed:", error.message);

    } else {
      console.error("Unknown error:", error.message);
    }
  }
}
```

#### Clean up using finally

- So there are three identical finallys? Promise, then+catch and this native javascript finally?

```javascript
async function withCleanup() {
  let resource;
  try {
    resource = await getResource();
    return await useResource(resource)
  } catch (error) {
    console.error("Error using resource:", error.message);
  } finally {
    if (resource) {
      await releaseResource(resource);
    }
  }
}
```

### [13	Practice Problems: Error Handling with Async / Await](https://launchschool.com/lessons/701aaae0/assignments/f4edd69b)

1. yup, got it:

```javascript
function flakyService() {
  return new Promise((resolve, reject) => {
    if (Math.random() > 0.5) {
      resolve("Operation successful");
    } else {
      reject("Operation failed");
    }
  });
}

async function useFlakyService() {
  try {
    let data = await flakyService();
    console.log(data);
  } catch (error) {
    console.error(error);
  }
}

useFlakyService();
```

2. yup
```javascript
async function newOperation() {
  try {
    let result = await operation();
    console.log(result);
  } finally {
    console.log("Cleaning up resources...")
  }
}
```

3. OK, this one got away from me somewhat. I missed:
  - both functions needed to become async and there should have been 2 try/catch clasues.

```javascript
async function retryOperation(operationFunc) {
  let attempts = 0;

  async function attempt() {
    try {
      return operationFunc()
    } catch (error) {
      if (attempts < 2) {
        attempts++;
        console.log(`Retry attempt #${attempts}`);
        return attempt();
      } else {
        throw error;
      }
    }
  }

  try {
    console.log(await attempt());
  } catch {
    console.error("Operation failed");
  }
}

retryOperation(flakyService;)
```

4. OK this one got away from me...

```javascript
async function asyncLoadData() {
  try {
    const result = await new Promise((resolve, reject) => {
      setTimeout(() => {
        if (Math.random() > 0.5) {
          resolve("Data loaded");
        } else {
          reject("Network error");
        }
      }, 1000);
    });
    return result;
  } catch {
    console.error("An error occurred. Attempting to recover...");
    return "Using cached data;"
  }
}

async function processData() {
  console.log(await asyncLoadData());
}

processData();
```
### [14	Assignment: Updating User Information App with `async` / `await`](https://launchschool.com/lessons/701aaae0/assignments/d0fc9896)

- OK, didn't make it, but piano piano -> it's certainly more concise.

```javascript
async function retryNTimes(fn, n, ...args) {
  let attempts = 0;

  while (attempts <= n) {
    try {
      console.log(`Attempt number: ${attempts + 1}`);
      return await fn(...args); // Try calling the function
    } catch (error) {
      if (attempts >= n) {
        throw error; // Reject if max attempts reached
      }
      attempts++;
    }
  }
}
```
ok, doens't look beyond the ken of man. Mind you I didn't get the 2nd part.

### [15	Combining Async / Await with Promise Methods](https://launchschool.com/lessons/701aaae0/assignments/e02110a7)

```javascript
async function fetchAll() {
  const dataItems = ['foo', 42, 'Hello, world!'];
  const delays = [1500, 500, 2300];

  // Create an iterable of our promises without pausing execution
  const promises = dataItems.map((data, i) => fetchData(data, delays[i]));

  const results = await Promise.all(promises);  // `await` the promise returned by `Promise.all`
  console.log(`All data fetched: ${results}`);
}
```

### [16	Summary](https://launchschool.com/lessons/701aaae0/assignments/b8645009)

- all good

#### Conclusion

- be good at this
  
### 17	Quiz

- 1st go (50%)
- 2nd go (11th June 2025) 6/10 (60%)
1. B -> wrong, fulfilled is a valid state. Resolved is the wrong one.
2. B -> tick
3. A, B -> tick
4. B, C -> tick
5. C, D -> tick
6. B, C -> not c
7. A -> tick
8. a -> no, D because you can't know whch image will download first.
9. B, D -> tick
10. A -> no B, we don't do anything with the value returned by the promise, so it isn't output
## [3	Making HTTP Requests from JavaScript](https://launchschool.com/lessons/1b723bd0/assignments)


### [1	Introduction](https://launchschool.com/lessons/1b723bd0/assignments/ff3903a8)

- 2nd pass (12.6.25)
- `fetch` browser API for making network requests

### [2	What to Focus On](https://launchschool.com/lessons/1b723bd0/assignments/d9f41631)	

- Think HTTP
- Be Familiar with Fetch
- Understand How To Serialize Data
- Decompose Functionality into API Requests
- Read API Documentation

### [3	HTTP Review](https://launchschool.com/lessons/1b723bd0/assignments/ad1bafe7)	

- video: client/server request response cycle.
  -first step: create a request (usually prompted by user event)
  - user types in URL, this creates a request. Request:
    - domain name is not used in the request
    - request type = GET
    - path: `/tasks`
    - paramenters: `?due=today`
  - When the server receives the request it begins working with the data:
  - Then sends a response:
    - Status: 200 OK
    - Headers: content-type: text/html
    - Body: <html><body>...

- practice problems:
  - 1. What are the required components of an HTTP request? What are the additional optional components?
       - HTTP method
       - path
       - also HTTP version
       - the rest is optional (paramenters, headers, message body)
- 2. What are the required components of an HTTP response? What are the additional optional components?
     - Status line with status code
     - headers & body are optional
- 3. What determines whether a request should use GET or POST as its HTTP method?
     - Get for retrieving data, POST for sending data
### [4	Book: Working with Web APIs](https://github.com/SandyRodger/launch_school_books/edit/main/working_with_web_APIs.md)

# [Working with Web APIs](https://launchschool.com/books/working_with_apis)

## Introduction
### What this Book Covers
- Web 2.0 web applications focus on user participation and content-creation.
- APIs allow this by being software one can plug into one's web app.

- We're covering the basics here (and in the associated course? - JS235?):
  -  what is it
  -  How to interact with them
  -  How to build your own

## [Preparations](https://launchschool.com/books/working_with_apis/read/preparations)

- `brew install httpie`
- `http --version` -> 3.2.4
- https://www.postman.com/downloads/

#### HTTPie Option Reference

- `-p`
- `-a`
- `--help`
  
## [Using Postman](https://launchschool.com/books/working_with_apis/read/using_postman#makingarequest)

- It's like a specialised web-browser.
- `www.google.com/search`
- The API example uses DUckDuckGo, but I have blocked that website, so ChatGPT provided the following alternatives:
  - `https://en.wikipedia.org/w/api.php?action=opensearch&search=pistachios&limit=5&format=json`
  - `https://itunes.apple.com/search?term=pistachios&limit=5`
- APIs can be accesed using secure URLs like web-pages.

### Checking the Weather
- This brings up an important point about working with APIs: it is very important to read any documentation for the API in order to be able to interpret the values in a response.

### Summary

- postman makes it easy to make http requests from a web browser
- because it runs in a web-browser, Postman has few dependencies and is easy to install on almost any computer.

## Defining API
### [What is an API?](https://launchschool.com/books/working_with_apis/read/defining_api#whatisanapi)

- Application Programming Interface: provides a way for computer systems to interact with each other
- Every programming language has a built-in API that is used to write programs.
- Mobile devices provide APIs to provide access to location etc.
- Practically everything has an API. What they all do is provide functionality for use by another program.

### [Web APIs](https://launchschool.com/books/working_with_apis/read/defining_api#webapis)

- A type of API that are built with web-technologies and work similarly to the web itself.

### [Provider and Consumer](https://launchschool.com/books/working_with_apis/read/defining_api#provider_and_consumer)

- We must distinguish between the system the API belongs to and the external service that will use the API.
  - API PROVIDER: the system that provides the API for other parites to use. For instance HitHub is the provider of the GitHub API.
  - An API consumer is the system taht uses the API ti accomplish some work. When you look at the weather on your phone it is running a program which is consuming a weather API to retrieve forcast data.

- in this boook we will be the manual consumers of the web store API. 
### [What about Clients and Servers?](https://launchschool.com/books/working_with_apis/read/defining_api#clientsandservers)

- forget the image of the large server farm and the individual mobile phone as the client, we just mean the server as the API provider and the client as the consumer.

### [Summary](https://launchschool.com/books/working_with_apis/read/defining_api#summary)

- Web APIs allow one system to interact with another over HTTP
- The system offering the API for use by the others is the provider
- The system interacting with the API to accomplish a goal in the consumer
- It is best to use the terms provider and consumer over client and server.
  
## [What APIs Can Do](https://launchschool.com/books/working_with_apis/read/what_apis_can_do)

### [Sharing Data](https://launchschool.com/books/working_with_apis/read/what_apis_can_do#sharingdate)

- share data between systems1

### [Enabling Automation](https://launchschool.com/books/working_with_apis/read/what_apis_can_do#enablingautomation)

- HatCo example
- "APIs allow users of a service to make use of it in new and useful ways."

### [Leverage Existing Services](https://launchschool.com/books/working_with_apis/read/what_apis_can_do#leverageexistingservices)

- "APIs enable application developers to build their applications on top of a variety of other specialized systems, allowing them to focus on their actual objectives and not worry about all the complexities of every part of the system."

### [Summary](https://launchschool.com/books/working_with_apis/read/what_apis_can_do#summary)

- APIs break the walls beween systems allowing them to share data.
- APIs provide an escape hatch; enabling service users to customise the software's beyaviour or integrate it into other sytems if required.
- Many modern web applications provide an API that allows developers to integrate their own code with these applications, taking advantage of the services' functionality in their own apps.

## [Accessibility](https://launchschool.com/books/working_with_apis/read/accessibility)

### [Public and Private](https://launchschool.com/books/working_with_apis/read/accessibility#publicandprivate)

##### Public APIs
- Insta, facebook, Twitter -> they all got em.
##### Private APIs
- GOogle search page. Don't try and call private APIs. It's sometimes doable, but generally a bad idea.
- Companies often gain an advantage in making their APIs public.

### [Terms and conditions](https://launchschool.com/books/working_with_apis/read/accessibility#termsandconditions)

- **Restrictions** You should understand waht restrictions does the API place on your use of its data. For instance data from the Amazon Product Advertising API is only available to Amazon Associates.
- **Is the API exposing any personal data?** Many social apops allow acces to a user;'s personal info, but by accessing it you are thaking the responsibility of keeping it secure.
-  **rate limits** Many APIs limit how many requests can be sent from a single user/app. Such restrictons will impact how you design your app.

### [Summary](https://launchschool.com/books/working_with_apis/read/accessibility#summary)


- APIs come in 2 flavours: public and private. You'll mainly be using pulics, private's usually when it's your own.
- You usually have to accept terms of the API provider

## [A Review of HTTP](https://launchschool.com/books/working_with_apis/read/a_review_of_http)

### [Request and Response](https://launchschool.com/books/working_with_apis/read/a_review_of_http#requestandresponse)


- Web APIs work wiht HyperTextTransfer Protocol -> just like websites/browsers/servers.
- request-response pattern. Remember this?
- IPAPI (PrettyBallsy)
  - API access key: 7c8917a2deabd924800fb9e0a6d51183
- `http http://api.ipapi.com/161.185.160.93?access_key=7c8917a2deabd924800fb9e0a6d51183 --json`

```
HTTP/1.1 200 OK
Access-Control-Allow-Headers: *
Access-Control-Allow-Methods: GET, POST, HEAD, OPTIONS
Access-Control-Allow-Origin: *
Cf-Cache-Status: DYNAMIC
Cf-Ray: 948112150ae0cc8d-MAN
Connection: keep-alive
Content-Type: application/json
Date: Fri, 30 May 2025 20:44:38 GMT
Nel: {"success_fraction":0,"report_to":"cf-nel","max_age":604800}
Report-To: {"endpoints":[{"url":"https:\/\/a.nel.cloudflare.com\/report\/v4?s=P7ergkYr%2BJnDuMkUMVCCCyNeL9OTUKK8ZSFVnEaQbId8InceSc61Y2eaOwjb4cvbodD6WyRrFAsv7PfY%2FX8rzAlfWJsr9OqyIx288VknBmb0FAeFfcDne4xtYE165CE%3D"}],"group":"cf-nel","max_age":604800}
Server: cloudflare
Server-Timing: cfL4;desc="?proto=TCP&rtt=15293&min_rtt=15293&rtt_var=7646&sent=1&recv=3&lost=0&retrans=0&sent_bytes=0&recv_bytes=215&delivery_rate=0&cwnd=225&unsent_bytes=0&cid=0000000000000000&ts=0&x=0"
Transfer-Encoding: chunked
X-Apilayer-Transaction-Id: 10da26df-46b4-4957-b77f-81fc2d67d7af
```

- Break-down:
  - status code: HTTP/1.1 200 OK
- 2xx is good
- 3xx is successful, but the response to the request is somewhere else
- 4xx means the client's made a mistake
- 5xx means the server's encountered an error, usually they screwed up, rather than the client.

### [Headers](https://launchschool.com/books/working_with_apis/read/a_review_of_http#headers)

- for example:

```
CF-Cache-Status: DYNAMIC
CF-RAY: 7694e9094d32ef9c-PDX
Connection: keep-alive
Content-Encoding: gzip
Content-Type: application/json
Date: Sun, 13 Nov 2022 04:54:35 GMT
NEL: {"success_fraction":0,"report_to":"cf-nel","max_age":604800}
Report-To: {"endpoints":[{"url":"https:\/\/a.nel.cloudflare.com\/report\/v3?s=VJQHe9u15PD8yXpSY0ZwfioL2qi8iHUOoiQLdEzMKnAnIrmmrtM6caxSquhaKkJuzMVBs3GHty42saK3qQsqqgjBSTuw0yeGlgAmXaJ3vItVuCcntg4vPOUfvQC%2FeyUOEmdN0C9DqoxDMqSA"}],"group":"cf-nel","max_age":604800}
Server: cloudflare
Transfer-Encoding: chunked
access-control-allow-headers: *
access-control-allow-methods: GET, POST, HEAD, OPTIONS
access-control-allow-origin: *
alt-svc: h3=":443"; ma=86400, h3-29=":443"; ma=86400
x-apilayer-transaction-id: 113aa789-ec14-4759-bb75-972b61f7b70f
x-increment-usage: 1
x-quota-limit: 1000
x-quota-remaining: 996
x-request-time: 0.031
```

- of which the content type is the most important:
  - `Content-Type: application/json`

### [Body](https://launchschool.com/books/working_with_apis/read/a_review_of_http#body)

- getting sleepy

### [Summary](https://launchschool.com/books/working_with_apis/read/a_review_of_http#summary)

- Web APIs are buolt on top of HTTP the technology that makes the web work
- HTTP responses have 3 main parts: status code, headers and body
- The conten-type header describes the format of the response body

## [A Review of URLs](https://launchschool.com/books/working_with_apis/read/a_review_of_urls)

### [URL or URI](https://launchschool.com/books/working_with_apis/read/a_review_of_urls#url_or_uri)

- URI: uniform resource Identifier: this identifies a resource. **These can be anywhere**
  - analogous to social security number
- URL: Uniform resource locator: a web resource with a specified locaton on a computer network.
  -  analogous to someone's street address
  -  ALso unique, so you could say a URL is a type of URI.
-  IF YOU ARE WORKING WITH RESOURCES ON THE INTERNET USE URL

### The parts of a URL

- Scheme: https
- `://`
- hostname
- optional colon and port: `:81` (uncommon with APIs)
- the path to the resource, like `/api/v1/pages/1`
- An optional query string: `?=query=term`

### [Identifiers in Paths](https://launchschool.com/books/working_with_apis/read/a_review_of_urls#identifiers_in_paths)

- `/products/:id` , where :id is a place-holder to be filled with a value.
- Nested paths are also a thing: `/products/:product_id/comments/:id`

### [Summary](https://launchschool.com/books/working_with_apis/read/a_review_of_urls#summary)

- Working with APIS, involves working with URLs
- URLs represent where a resource is and how it can be accessed.
- URLs typically contain a:
  - Scheme
  - hostname
  - path
  - sometimes a query string
- Paths can include placeholders when they are written generically

## [Media Types](https://launchschool.com/books/working_with_apis/read/media_types)

- The internet has shared markup languages and data formats to facilitate communication. HTML is an example of this. Almost all devices can read HTML and display it.
- HTML is one of many different **media types** (AKA content types sometimes AKA MIME types)
- `Content-Type: text/html` -> this tells the browser to interpret the content as HTML rather than render it graphically on the screen.
- `charset`
- `http --headers www.google.com`
- photos are returned as `image/jpeg`
  - example: `http --headers https://c2.staticflickr.com/4/3913/15095210318_930069f3d6_c.jpg`

### Data-serialization

- APIs allow systems to communicate by exchanging structured data. The structure of these is **data serialization format**
- Example with the black circle
- A "vector graphics program"
- save a representation of the circe with some SVG code(?)

```html
<svg viewBox="0 0 55 54">
  <circle cx="32.5" cy="22" r="21.3" fill="black"/>
</svg>
```

### XML

- Extensible Markup language
- stricter than HTML
- example:

```javascript
  <address>
    <street>1600 Pennsylvania Ave NW</street>
    <city>Washington</city>
    <state>DC</state>
    <zipcode>20500</zipcode>
    <country>Unites States</country>
</address>
```

### [JSON](https://launchschool.com/books/working_with_apis/read/media_types#json)

- a simple JSON document is used to represent key and value pairs:

```json
{
  "street": "1600 Pennsylvania Ave NW",
  "city": "Washington",
  "state": "DC",
  "zipcode": "20500",
  "country": "United States"
}
```

- this book we'll only use JSON

### [Summary](https://launchschool.com/books/working_with_apis/read/media_types#summary)

- media-types describe the format of a response's body
- media types are represented in an http response's `content-type` header and as a result are soemtimes referred to as content types.
- data-serialization provies a common way for systems to pass data to each other, with a guarantee that each system will be able to understand the data.
- JSON is the most popular media type for web APIs.

## [REST and CRUD](https://launchschool.com/books/working_with_apis/read/rest_and_crud)

### [What is REST?](https://launchschool.com/books/working_with_apis/read/rest_and_crud#rest)
- representational State Transfer
- representational => we send a copy
- State transfer: how HTTP is a stateless protocol, so the servers don't know anything at all about the clients and everything the server needs to process the request is included in the state itself.

- example of deleting a social media profile. The same actions could be performed by an API. Differences:
  - HTML forms must be loaded before they cab be submitted. Not so for APIs, they don't have forms. Therefore we can lose the initial GET request.
  - HTML forms only support 2 of the many HTTP methods, GET and POST. APIs are able to take advantage of all HTTP methods so they're clearer.
  - REST encompasses everything you would like to do with 'what' and 'how'.
    - what  resource is being brought into play?
    - how are we changing / interacting with it?

### [CRUD](https://launchschool.com/books/working_with_apis/read/rest_and_crud#crud)

- the 4 things you can do with a resource.
  - POST - create
  - GET - post
  - PUT - update
  - DELETE - delete
- if you know which action you want to take, you probably know which method to use.
-  By following REST conventions, API designers have fewer decisions to make about how to build an API and API consumers have fewer questions to answer before using one

### [A RESTful API Template](https://launchschool.com/books/working_with_apis/read/rest_and_crud#restfulapitemplate)

- template for any resource:

<img width="713" alt="Screenshot 2025-05-31 at 17 46 45" src="https://github.com/user-attachments/assets/39fc70d5-79fb-4b75-8add-e7040c877d57" />

- by following REST conventions most of the decisions a designer has to make turn into: What resources will be exposed?
- A lot of the perspective shift involves changing verb-based language into noun based ie:
  - (deposit $100 into this account) -> (create a new transaction with an amount of $100 for this account)
 
### [Resource-Oriented Thinking](https://launchschool.com/books/working_with_apis/read/rest_and_crud#resourceorientedthinking)

- examples:

<img width="701" alt="Screenshot 2025-05-31 at 17 50 54" src="https://github.com/user-attachments/assets/7b6ec1ff-a13c-4155-9ff9-472ff39f2647" />

- **singular resource or singleton resource**

### Conventions, not Rules
### Summary

- REST is a set of conventions about how to build APIs.
- RESTful APIs consist of CRUD actions on a resource
- By limiting actions to CRUD, REST requires thinking in a resource-oriented way.
- It is worth being as RESTful as possible, but there are times when it is not the best solution.

# WORKING WITH AN API
## [Fetching Resources](https://launchschool.com/books/working_with_apis/read/fetching_resources#serversetup)

### [Server setup](https://launchschool.com/books/working_with_apis/read/fetching_resources#serversetup)
- [apiculture](https://apiculture-n0y1.onrender.com/)
- apiculture-n0y1

### [Fetching a Resource](https://launchschool.com/books/working_with_apis/read/fetching_resources#fetchingaresource)

- `http GET https://apiculture-n0y1.onrender.com/v1/products/1`

```
HTTP/1.1 200 OK
CF-RAY: 9488393f3fe0b396-MAN
Connection: keep-alive
Content-Encoding: gzip
Content-Type: application/json
Date: Sat, 31 May 2025 17:34:44 GMT
Server: cloudflare
Transfer-Encoding: chunked
alt-svc: h3=":443"; ma=86400
cf-cache-status: DYNAMIC
rndr-id: af83d137-14e2-4bf4
status: 200 OK
vary: Origin, Accept-Encoding
x-render-origin-server: Render

{
    "id": 1,
    "name": "Red Pen",
    "price": 100,
    "sku": "redp100"
}
```
### [What is a Resource?](https://launchschool.com/books/working_with_apis/read/fetching_resources#what_is_a_resource)

-  the representation of some grouping of data.
-  anything an API has to deal with

### [Fetching a collection](https://launchschool.com/books/working_with_apis/read/fetching_resources#fetching_a_collection)

- `http GET https://apiculture-n0y1.onrender.com/v1/products`

```
[
    {
        "id": 1,
        "name": "Red Pen",
        "price": 100,
        "sku": "redp100"
    },
    {
        "id": 2,
        "name": "Blue Pen",
        "price": 100,
        "sku": "blup100"
    },
    {
        "id": 3,
        "name": "Black Pen",
        "price": 100,
        "sku": "blap100"
    }
]
```

### [Elements and Collections](https://launchschool.com/books/working_with_apis/read/fetching_resources#elements_and_collection)

- There are 2 elements involved in the use of RESTful APIs:
  -  elements (could be `/api/blog/posts/1`)
  -  collections (could be `/api/blog/posts`)

#### How to know if a URL is for a collection or an element?

-  it is best to reference the API's official documentation.
- often there is none, so:
  - Collection clues:
    - path ends in plural word => `example.com/products`
    - Response body contains multiple elements
  - single element clues:
    - path ends in plural word - slash - identifier
    - Response body contains single element

### [Summary](https://launchschool.com/books/working_with_apis/read/fetching_resources#summary)

- APIs provide access to single resources (elements) or groups of resources (collections).
- The path for an element is usually the path for its collection, plus an identifier for that resource.

## [Requests in Depth](https://launchschool.com/books/working_with_apis/read/requests_in_depth)

### [GET and POST](https://launchschool.com/books/working_with_apis/read/requests_in_depth#get_and_post)

- all requests made to web-servers start with an HTTP method (sometimes called a verb)
- For a long time web-browsers only supported GET and POST

### [Parts of a Request](https://launchschool.com/books/working_with_apis/read/requests_in_depth#parts_of_a_request)

- `http --print H GET https://apiculture-n0y1.onrender.com/v1/products/1`
- `Accept: */*` -> specifies waht media types the client will accept. This means any type. It's better to be more explicit.
- We want JSON, so we need to iinclude `application/json` =>

`http --print=H GET https://apiculture-n0y1.onrender.com/v1/products/1 Accept:application/json`

```
GET /v1/products/1 HTTP/1.1
Accept: application/json
Accept-Encoding: gzip, deflate
Connection: keep-alive
Host: apiculture-n0y1.onrender.com
User-Agent: HTTPie/3.2.4
```

### [Summary](https://launchschool.com/books/working_with_apis/read/requests_in_depth#summary)

- HTTP requests include a path, method, headers and body.
- The **Accept** header tells the provider what media types can be used to respond to the request

## [Creating Resources](https://launchschool.com/books/working_with_apis/read/creating_resources)

- There are also many applications that provide specialized reporting for some aspect of another system such as social networking sites

### [HTTP Request Side Effects](https://launchschool.com/books/working_with_apis/read/creating_resources#http_request_side_effects)

- GET requests should not alter resources, but there can be side-effects. For example a hit-counter. So consider this when creating resources.

### [Creating a resource](https://launchschool.com/books/working_with_apis/read/creating_resources#creating_a_resource)

- If you're making changes it's read and write, if not it's read-only
- Form submission is always POST
- We're going to try adding a resource to our web-page, but for this we'll need authentication.

`http POST https://apiculture-n0y1.onrender.com/v1/products name="Purple Pen" sku="purp100" price:=100` =>

```
HTTP/1.1 401 Unauthorized
CF-RAY: 948dcc2c7bf6eb1c-MAN
Connection: keep-alive
Content-Type: application/json
Date: Sun, 01 Jun 2025 09:48:50 GMT
Server: cloudflare
Transfer-Encoding: chunked
alt-svc: h3=":443"; ma=86400
cf-cache-status: DYNAMIC
rndr-id: dfe970ae-2f0a-4b3c
status: 401 Unauthorized
vary: Origin, Accept-Encoding
x-render-origin-server: Render

{
    "message": "Must pass credentials in Authentication header.",
    "status_code": 401
}
```

- Our web store uses token-based authentication, which is a common way to secure API endpoints

- `http POST https://apiculture-n0y1.onrender.com/v1/products Authorization:"token AUTH_TOKEN" name="Purple Pen" sku="purp100" price:=100`

```
HTTP/1.1 201 Created
CF-RAY: 948dd1821fb83c84-MAN
Connection: keep-alive
Content-Type: application/json
Date: Sun, 01 Jun 2025 09:52:29 GMT
Server: cloudflare
Transfer-Encoding: chunked
alt-svc: h3=":443"; ma=86400
cf-cache-status: DYNAMIC
rndr-id: 921c8735-db24-4361
status: 201 Created
vary: Origin, Accept-Encoding
x-render-origin-server: Render

{
    "id": 4,
    "name": "Purple Pen",
    "price": 100,
    "sku": "purp100"
}
```

- Many APIs use more sophisticated authentication means like OAuth or API keys

### [Handling Errors](https://launchschool.com/books/working_with_apis/read/creating_resources#handling_errors)

- Common errors and what you should do with them:

##### Missing or Invalid Parameters
  - example: `http POST https://apiculture-n0y1.onrender.com/v1/products Authorization:"token AUTH_TOKEN"`

```
HTTP/1.1 422 Unprocessable Entity
CF-RAY: 948ddce488172204-MAN
Connection: keep-alive
Content-Type: application/json
Date: Sun, 01 Jun 2025 10:00:15 GMT
Server: cloudflare
Transfer-Encoding: chunked
alt-svc: h3=":443"; ma=86400
cf-cache-status: DYNAMIC
rndr-id: 9d3a3f07-f7f4-487a
status: 422 Unprocessable Entity
vary: Origin, Accept-Encoding
x-render-origin-server: Render

{
    "message": "Name is missing, sku is missing, price is missing",
    "status_code": 422
}
```

- status 422 -> unprocessable entity

##### Missing resources

- http GET https://apiculture-n0y1.onrender.com/v1/products/42

```
HTTP/1.1 404 Not Found
CF-RAY: 948de0eaffa32dd8-MAN
Connection: keep-alive
Content-Encoding: gzip
Content-Type: application/json
Date: Sun, 01 Jun 2025 10:03:00 GMT
Server: cloudflare
Transfer-Encoding: chunked
alt-svc: h3=":443"; ma=86400
cf-cache-status: DYNAMIC
rndr-id: 93277252-686b-4ce7
status: 404 Not Found
vary: Origin, Accept-Encoding
x-render-origin-server: Render

{
    "message": "Couldn't find WebStore::Product with 'id'=42",
    "status_code": 404
}
```

##### Authentication

- 401, 403, 404

##### Incorrect Media Type

- HTTPie automatically converts media-types into JSON because that's the most common.
- The `HBhb` value in the following tells HTTPie to print out the heaeders and body for both request and response:

`http --print HBhb POST https://apiculture-n0y1.onrender.com/v1/products Authorization:"token AUTH_TOKEN"`

```
POST /v1/products HTTP/1.1
Accept: */*
Accept-Encoding: gzip, deflate
Authorization: token AUTH_TOKEN
Connection: keep-alive
Content-Length: 0
Host: apiculture-n0y1.onrender.com
User-Agent: HTTPie/3.2.4



HTTP/1.1 422 Unprocessable Entity
CF-RAY: 948de7582935d85e-MAN
Connection: keep-alive
Content-Type: application/json
Date: Sun, 01 Jun 2025 10:07:23 GMT
Server: cloudflare
Transfer-Encoding: chunked
alt-svc: h3=":443"; ma=86400
cf-cache-status: DYNAMIC
rndr-id: 67a3f66e-439d-44b7
status: 422 Unprocessable Entity
vary: Origin, Accept-Encoding
x-render-origin-server: Render

{
    "message": "Name is missing, sku is missing, price is missing",
    "status_code": 422
}
```

- note: content type is application/JSON and body is JSON

##### Rate limiting

- It's possible an API will receive too many requests in a short period of time.
- Most APIs address this problem by enforcing "rate limiting".
- These usually return 403 forbidden

##### Server Errors

- The above are all client errors. They have the code 5xx.
- possible causes:
  -  a bug in server implementation
  -  Hardward problem
  -  Unforseen error (same as first point?)
  - Sometimes a 5xx error is returned when a client error would be more fitting.

### [Summary](https://launchschool.com/books/working_with_apis/read/creating_resources#summary)

- create resources with POST
- requests need to include all required parameters and use the proper media type
- responses to failed requests will often contain information about the cause of failure

## [More HTTP Methods](https://launchschool.com/books/working_with_apis/read/more_http_methods)

### [Updating a resource](https://launchschool.com/books/working_with_apis/read/more_http_methods#updating_a_resource)

- fetch all products to see what's there:
  - http GET https://apiculture-n0y1.onrender.com/v1/products

```
HTTP/1.1 200 OK
CF-RAY: 948dfae50fed9b32-MAN
Connection: keep-alive
Content-Encoding: gzip
Content-Type: application/json
Date: Sun, 01 Jun 2025 10:20:44 GMT
Server: cloudflare
Transfer-Encoding: chunked
alt-svc: h3=":443"; ma=86400
cf-cache-status: DYNAMIC
rndr-id: 052be07c-cc0a-45a0
status: 200 OK
vary: Origin, Accept-Encoding
x-render-origin-server: Render

[
    {
        "id": 1,
        "name": "Red Pen",
        "price": 100,
        "sku": "redp100"
    },
    {
        "id": 2,
        "name": "Blue Pen",
        "price": 100,
        "sku": "blup100"
    },
    {
        "id": 3,
        "name": "Black Pen",
        "price": 100,
        "sku": "blap100"
    },
    {
        "id": 4,
        "name": "Purple Pen",
        "price": 100,
        "sku": "purp100"
    }
]
```

- updating this product will look a lot like creating a new one. Differences are we use PUT instead of POST and the product's path instead of the product collection path. (so /products/1 instead of /products)

- http PUT https://apiculture-n0y1.onrender.com/v1/products/4 Authorization:"token AUTH_TOKEN" price=150
- http PUT https://apiculture-n0y1.onrender.com/v1/products/4 Authorization:"token AUTH_TOKEN" name="New and Improved Purple Pen" sku="newp100"
```
HTTP/1.1 200 OK
CF-RAY: 948e05bafe68eb1d-MAN
Connection: keep-alive
Content-Encoding: gzip
Content-Type: application/json
Date: Sun, 01 Jun 2025 10:28:08 GMT
Server: cloudflare
Transfer-Encoding: chunked
alt-svc: h3=":443"; ma=86400
cf-cache-status: DYNAMIC
rndr-id: 993ab4fc-de79-4d51
status: 200 OK
vary: Origin, Accept-Encoding
x-render-origin-server: Render

{
    "id": 4,
    "name": "Purple Pen",
    "price": 150,
    "sku": "purp100"
}
```

### [Deleting a Resource](https://launchschool.com/books/working_with_apis/read/more_http_methods#deleting_a_resource)

- like fetching a resource, except you use DELETE.
- http DELETE https://apiculture-n0y1.onrender.com/v1/products/4 Authorization:"token AUTH_TOKEN"

```
HTTP/1.1 204 No Content
CF-RAY: 948e0b086ce70757-MAN
Connection: keep-alive
Date: Sun, 01 Jun 2025 10:31:45 GMT
Server: cloudflare
alt-svc: h3=":443"; ma=86400
cf-cache-status: DYNAMIC
rndr-id: 2b6f5725-c07b-4c1a
status: 204 No Content
vary: Origin
x-render-origin-server: Render
```

### [Summary](https://launchschool.com/books/working_with_apis/read/more_http_methods#summary)

- Use HTTP method PUT to update resources.
- Use HTTP method DELETE to delete resources.

# REAL WORLD APIS
## [TMDB API](https://launchschool.com/books/working_with_apis/read/tmdb_api)
### Our Goal

- learn how to interact with this movie-database. How to fetch movie-data and rate the movie

### [Reading Documentation](https://launchschool.com/books/working_with_apis/read/tmdb_api#readingdocumentation)

- First steps fo working with an API:
  - What's the URL:
    - protocol
    - host
    - path
  - what params do I need
  - What auth is required
- we'll look here: https://developer.themoviedb.org/reference/intro/getting-started
- Movies -> details : https://developer.themoviedb.org/reference/movie-details

### [First API Request](https://launchschool.com/books/working_with_apis/read/tmdb_api#firstrequest)

- https://api.themoviedb.org/3/movie/27205
- Inception

```
HTTP/1.1 401 Unauthorized
Alt-Svc: h3=":443"; ma=86400
Cache-Control: public, max-age=300
Connection: keep-alive
Content-Type: application/json; charset=utf-8
Date: Sun, 01 Jun 2025 10:42:38 GMT
Server: openresty
Transfer-Encoding: chunked
Vary: Origin
Via: 1.1 da75aba073a4674b4acba0f7682b0446.cloudfront.net (CloudFront)
X-Amz-Cf-Id: tAF5Q9XlGcfHIkjmRSdLpWpbf6aQV4OXxSmCuikbLlpmaD9cDj_SrQ==
X-Amz-Cf-Pop: LHR50-P2
X-Cache: Error from cloudfront

{
    "status_code": 7,
    "status_message": "Invalid API key: You must be granted a valid key.",
    "success": false
}
```
- need auth.

### [Gaining Access](https://launchschool.com/books/working_with_apis/read/tmdb_api#gainingaccess)

- we won't be using a URL, so use `http://localhost:3000`
- API read-access token:
`eyJhbGciOiJIUzI1NiJ9.eyJhdWQiOiIzMDY5ZjIyNjc4YmJkODliZDRhNmQyY2IwYzI0OWUzNCIsIm5iZiI6MTc0ODc3NDkzMC40NjQsInN1YiI6IjY4M2MzMDEyZTg2NTYzYjgxMWQyMTIxZSIsInNjb3BlcyI6WyJhcGlfcmVhZCJdLCJ2ZXJzaW9uIjoxfQ.5GIHNExqKPBkcsqbcE414OqC3NpEFgMIgB-4QBAC-Tk`
- API key -> `3069f22678bbd89bd4a6d2cb0c249e34`

### [Fetching movie information](https://launchschool.com/books/working_with_apis/read/tmdb_api#fetchingmovieinfo)

- http GET https://api.themoviedb.org/3/movie/27205 \
    "Authorization: Bearer eyJhbGciOiJIUzI1NiJ9.eyJhdWQiOiIzMDY5ZjIyNjc4YmJkODliZDRhNmQyY2IwYzI0OWUzNCIsIm5iZiI6MTc0ODc3NDkzMC40NjQsInN1YiI6IjY4M2MzMDEyZTg2NTYzYjgxMWQyMTIxZSIsInNjb3BlcyI6WyJhcGlfcmVhZCJdLCJ2ZXJzaW9uIjoxfQ.5GIHNExqKPBkcsqbcE414OqC3NpEFgMIgB-4QBAC-Tk"

```
http GET https://api.themoviedb.org/3/movie/27205 \
>     "Authorization: Bearer eyJhbGciOiJIUzI1NiJ9.eyJhdWQiOiIzMDY5ZjIyNjc4YmJkODliZDRhNmQyY2IwYzI0OWUzNCIsIm5iZiI6MTc0ODc3NDkzMC40NjQsInN1YiI6IjY4M2MzMDEyZTg2NTYzYjgxMWQyMTIxZSIsInNjb3BlcyI6WyJhcGlfcmVhZCJdLCJ2ZXJzaW9uIjoxfQ.5GIHNExqKPBkcsqbcE414OqC3NpEFgMIgB-4QBAC-Tk"
HTTP/1.1 200 OK
Alt-Svc: h3=":443"; ma=86400
Cache-Control: public, max-age=6991
Connection: keep-alive
Content-Encoding: gzip
Content-Type: application/json;charset=utf-8
Date: Sun, 01 Jun 2025 10:55:20 GMT
ETag: W/"5d0b668cbd65633dab0f600de60311c1"
Server: openresty
Transfer-Encoding: chunked
Vary: accept-encoding, Origin
Via: 1.1 2a3070b1680e47b1f595c5f66410764a.cloudfront.net (CloudFront)
X-Amz-Cf-Id: hQ-2SmkO927sZyJJrihUil2Ru7BjSdmhHk9j3PoNHyZOdlYFu-MZSA==
X-Amz-Cf-Pop: LHR50-P2
X-Cache: Miss from cloudfront
x-az: us-east-1a
x-memc: HIT
x-memc-age: 18539
x-memc-expires: 6991
x-memc-key: ba0bb4b9033cab23506074980327c2f8
x-task-id: 5ae9ef7c9edb4082a88f27237d27cbc2

{
    "adult": false,
    "backdrop_path": "/8ZTVqvKDQ8emSGUEMjsS4yHAwrp.jpg",
    "belongs_to_collection": null,
    "budget": 160000000,
    "genres": [
        {
            "id": 28,
            "name": "Action"
        },
        {
            "id": 878,
            "name": "Science Fiction"
        },
        {
            "id": 12,
            "name": "Adventure"
        }
    ],
    "homepage": "https://www.warnerbros.com/movies/inception",
    "id": 27205,
    "imdb_id": "tt1375666",
    "origin_country": [
        "US",
        "GB"
    ],
    "original_language": "en",
    "original_title": "Inception",
    "overview": "Cobb, a skilled thief who commits corporate espionage by infiltrating the subconscious of his targets is offered a chance to regain his old life as payment for a task considered to be impossible: \"inception\", the implantation of another person's idea into a target's subconscious.",
    "popularity": 26.3253,
    "poster_path": "/oYuLEt3zVCKq57qu2F8dT7NIa6f.jpg",
    "production_companies": [
        {
            "id": 923,
            "logo_path": "/5UQsZrfbfG2dYJbx8DxfoTr2Bvu.png",
            "name": "Legendary Pictures",
            "origin_country": "US"
        },
        {
            "id": 9996,
            "logo_path": "/3tvBqYsBhxWeHlu62SIJ1el93O7.png",
            "name": "Syncopy",
            "origin_country": "GB"
        },
        {
            "id": 174,
            "logo_path": "/zhD3hhtKB5qyv7ZeL4uLpNxgMVU.png",
            "name": "Warner Bros. Pictures",
            "origin_country": "US"
        }
    ],
    "production_countries": [
        {
            "iso_3166_1": "GB",
            "name": "United Kingdom"
        },
        {
            "iso_3166_1": "US",
            "name": "United States of America"
        }
    ],
    "release_date": "2010-07-15",
    "revenue": 839030630,
    "runtime": 148,
    "spoken_languages": [
        {
            "english_name": "English",
            "iso_639_1": "en",
            "name": "English"
        },
        {
            "english_name": "French",
            "iso_639_1": "fr",
            "name": "Français"
        },
        {
            "english_name": "Japanese",
            "iso_639_1": "ja",
            "name": "日本語"
        },
        {
            "english_name": "Swahili",
            "iso_639_1": "sw",
            "name": "Kiswahili"
        }
    ],
    "status": "Released",
    "tagline": "Your mind is the scene of the crime.",
    "title": "Inception",
    "video": false,
    "vote_average": 8.369,
    "vote_count": 37509
}
```

- non-existant movie test:
  - http GET https://api.themoviedb.org/3/movie/999999999 \
    "Authorization: Bearer eyJhbGciOiJIUzI1NiJ9.eyJhdWQiOiIzMDY5ZjIyNjc4YmJkODliZDRhNmQyY2IwYzI0OWUzNCIsIm5iZiI6MTc0ODc3NDkzMC40NjQsInN1YiI6IjY4M2MzMDEyZTg2NTYzYjgxMWQyMTIxZSIsInNjb3BlcyI6WyJhcGlfcmVhZCJdLCJ2ZXJzaW9uIjoxfQ.5GIHNExqKPBkcsqbcE414OqC3NpEFgMIgB-4QBAC-Tk"

```
HTTP/1.1 404 Not Found
Alt-Svc: h3=":443"; ma=86400
Connection: keep-alive
Content-Encoding: gzip
Content-Type: application/json;charset=utf-8
Date: Sun, 01 Jun 2025 10:57:59 GMT
Server: openresty
Transfer-Encoding: chunked
Vary: accept-encoding, Origin
Via: 1.1 71cbe01df9e5102d886edc4f5a32c1ea.cloudfront.net (CloudFront)
X-Amz-Cf-Id: BBwudbiDTDnPQOgsUnLUw0nL3mLXQFzcLx2t8v09kc5ioA7sr2aEww==
X-Amz-Cf-Pop: LHR50-P2
X-Cache: Error from cloudfront
x-az: us-east-1b
x-task-id: 0091e8d404bb4c579424a9eabac49dd4

{
    "status_code": 34,
    "status_message": "The resource you requested could not be found.",
    "success": false
}
```

### [Rating A Movie](https://launchschool.com/books/working_with_apis/read/tmdb_api#ratingmovies)

- the documentation says we need:
  - a URL like this: `https://api.themoviedb.org/3/movie/{movie_id}/rating`
  - a value between 0.5 and 10
  - a POST method
  - our access token
- http POST https://api.themoviedb.org/3/movie/27205/rating \
    "Authorization: Bearer eyJhbGciOiJIUzI1NiJ9.eyJhdWQiOiIzMDY5ZjIyNjc4YmJkODliZDRhNmQyY2IwYzI0OWUzNCIsIm5iZiI6MTc0ODc3NDkzMC40NjQsInN1YiI6IjY4M2MzMDEyZTg2NTYzYjgxMWQyMTIxZSIsInNjb3BlcyI6WyJhcGlfcmVhZCJdLCJ2ZXJzaW9uIjoxfQ.5GIHNExqKPBkcsqbcE414OqC3NpEFgMIgB-4QBAC-Tk" \
    value:=9

```
HTTP/1.1 201 Created
Alt-Svc: h3=":443"; ma=86400
Connection: keep-alive
Content-Type: application/json;charset=utf-8
Date: Sun, 01 Jun 2025 11:00:40 GMT
Server: openresty
Transfer-Encoding: chunked
Via: 1.1 ce8f85a4dd9437febbc40094aa7d575a.cloudfront.net (CloudFront)
X-Amz-Cf-Id: zYb9Rh8on0FXajrCMfFzAOh8_4ZCeQeFqxb5vIl8_0gig0CQqr5JGw==
X-Amz-Cf-Pop: LHR50-P2
X-Cache: Miss from cloudfront
cache-control: public, max-age=0
content-encoding: gzip
etag: W/"4543905c5703940a323f39bb4fdcba82"
vary: accept-encoding, Origin
x-az: us-east-1c
x-task-id: 19248da2e9fe44b5bbd72d15bd3bc48f

{
    "status_code": 1,
    "status_message": "Success.",
    "success": true
}
```

- behind the scenes TMDB:
  - checks the movei exists
  - checks we haven't already rated this movie
  - recalculates the movie's average rating
  - records this rating in our account history

### [Deleting A Rating](https://launchschool.com/books/working_with_apis/read/tmdb_api#deletingrating)

- http DELETE https://api.themoviedb.org/3/movie/27205/rating \
    "Authorization: Bearer eyJhbGciOiJIUzI1NiJ9.eyJhdWQiOiIzMDY5ZjIyNjc4YmJkODliZDRhNmQyY2IwYzI0OWUzNCIsIm5iZiI6MTc0ODc3NDkzMC40NjQsInN1YiI6IjY4M2MzMDEyZTg2NTYzYjgxMWQyMTIxZSIsInNjb3BlcyI6WyJhcGlfcmVhZCJdLCJ2ZXJzaW9uIjoxfQ.5GIHNExqKPBkcsqbcE414OqC3NpEFgMIgB-4QBAC-Tk"

```
HTTP/1.1 200 OK
Alt-Svc: h3=":443"; ma=86400
Connection: keep-alive
Content-Type: application/json;charset=utf-8
Date: Sun, 01 Jun 2025 11:04:29 GMT
Server: openresty
Transfer-Encoding: chunked
Via: 1.1 0ed0b3a1a3e8908d48a47272b433d54e.cloudfront.net (CloudFront)
X-Amz-Cf-Id: YarYnZNVa2bKo6wERaBK0UwHtacyEbj7NXXXOB11qBkqVOAO7TwFgQ==
X-Amz-Cf-Pop: LHR50-P2
X-Cache: Miss from cloudfront
cache-control: public, max-age=0
content-encoding: gzip
etag: W/"0566f98871ddfd7f6ade28aaefb5167d"
vary: accept-encoding, Origin
x-az: us-east-1b
x-task-id: 8b4e2f6569074cd88a533175c64bc4e0

{
    "status_code": 13,
    "status_message": "The item/record was deleted successfully.",
    "success": true
}
```

### [TMDB API and REST Principles Analysis](https://launchschool.com/books/working_with_apis/read/tmdb_api#apirestanalysis)

##### Resource Design

- good, noun-based, so not 'getMovie' or 'createRadting', but `GET` and `POST`.

##### HTTP Methods

- Ideally we'd use `PUT` or `PATCH` for updates, rather than `POST` as they do. It's a trade-off: less RESTful, but devs don't have to check whether a rating exists before deciding whether to use post or put.

##### Error handling approach

- unconventional, but deliberate.

```
{
    "status_code": 7,
    "status_message": "Invalid API key: You must be granted a valid key.",
    "success": false
}
```
- they have their own custom status code system.
- Problems:
  - this custom code is not self-documenting, you have to search for what the code means.
  - The success field doesn't tell us anything new, the success is displayed in the HTTP status code.

##### Real-World Impact

- They made these choices for a reason.
- If they want to update the API now it has to be done very carefully, probably releasing a new version of the API, so users can opt-in or not to the changes.
- One must balance the potential disruption with the advantages of the update.

##### What we can learn

- even widely used APIs are imperfect
- Always read documentation carefully, don't assume this API will be like others.
- Check example responses in documentation.
- Even imperfect APIs can be useful

### Exercises:

This works:

http GET https://api.themoviedb.org/3/movie/27205 \
    "Authorization: Bearer eyJhbGciOiJIUzI1NiJ9.eyJhdWQiOiIzMDY5ZjIyNjc4YmJkODliZDRhNmQyY2IwYzI0OWUzNCIsIm5iZiI6MTc0ODc3NDkzMC40NjQsInN1YiI6IjY4M2MzMDEyZTg2NTYzYjgxMWQyMTIxZSIsInNjb3BlcyI6WyJhcGlfcmVhZCJdLCJ2ZXJzaW9uIjoxfQ.5GIHNExqKPBkcsqbcE414OqC3NpEFgMIgB-4QBAC-Tk"


1.

http GET "https://api.themoviedb.org/3/search/movie?query=Lilo%20and%20Stitch" \
  "Authorization: Bearer eyJhbGciOiJIUzI1NiJ9.eyJhdWQiOiIzMDY5ZjIyNjc4YmJkODliZDRhNmQyY2IwYzI0OWUzNCIsIm5iZiI6MTc0ODc3NDkzMC40NjQsInN1YiI6IjY4M2MzMDEyZTg2NTYzYjgxMWQyMTIxZSIsInNjb3BlcyI6WyJhcGlfcmVhZCJdLCJ2ZXJzaW9uIjoxfQ.5GIHNExqKPBkcsqbcE414OqC3NpEFgMIgB-4QBAC-Tk"

http GET "https://api.themoviedb.org/3/search/movie?query=Sikandar" \
  "Authorization: Bearer eyJhbGciOiJIUzI1NiJ9.eyJhdWQiOiIzMDY5ZjIyNjc4YmJkODliZDRhNmQyY2IwYzI0OWUzNCIsIm5iZiI6MTc0ODc3NDkzMC40NjQsInN1YiI6IjY4M2MzMDEyZTg2NTYzYjgxMWQyMTIxZSIsInNjb3BlcyI6WyJhcGlfcmVhZCJdLCJ2ZXJzaW9uIjoxfQ.5GIHNExqKPBkcsqbcE414OqC3NpEFgMIgB-4QBAC-Tk"

http GET "https://api.themoviedb.org/3/search/movie?query=Fountain%20of%20Youth" \
  "Authorization: Bearer eyJhbGciOiJIUzI1NiJ9.eyJhdWQiOiIzMDY5ZjIyNjc4YmJkODliZDRhNmQyY2IwYzI0OWUzNCIsIm5iZiI6MTc0ODc3NDkzMC40NjQsInN1YiI6IjY4M2MzMDEyZTg2NTYzYjgxMWQyMTIxZSIsInNjb3BlcyI6WyJhcGlfcmVhZCJdLCJ2ZXJzaW9uIjoxfQ.5GIHNExqKPBkcsqbcE414OqC3NpEFgMIgB-4QBAC-Tk"

2.

Movie 1: Lilo & Stitch:

http POST "https://api.themoviedb.org/3/account/22048510/watchlist \
     --header 'Authorization: Bearer eyJhbGciOiJIUzI1NiJ9.eyJhdWQiOiIzMDY5ZjIyNjc4YmJkODliZDRhNmQyY2IwYzI0OWUzNCIsIm5iZiI6MTc0ODc3NDkzMC40NjQsInN1YiI6IjY4M2MzMDEyZTg2NTYzYjgxMWQyMTIxZSIsInNjb3BlcyI6WyJhcGlfcmVhZCJdLCJ2ZXJzaW9uIjoxfQ.5GIHNExqKPBkcsqbcE414OqC3NpEFgMIgB-4QBAC-Tk' \
     --header 'accept: application/json' \
     --header 'content-type: application/json'

GET https://api.themoviedb.org/3/account?api_key=eyJhbGciOiJIUzI1NiJ9.eyJhdWQiOiIzMDY5ZjIyNjc4YmJkODliZDRhNmQyY2IwYzI0OWUzNCIsIm5iZiI6MTc0ODc3NDkzMC40NjQsInN1YiI6IjY4M2MzMDEyZTg2NTYzYjgxMWQyMTIxZSIsInNjb3BlcyI6WyJhcGlfcmVhZCJdLCJ2ZXJzaW9uIjoxfQ.5GIHNExqKPBkcsqbcE414OqC3NpEFgMIgB-4QBAC-Tk&session_id=YOUR_SESSION_ID

curl -X GET "https://api.themoviedb.org/3/authentication/token/new?api_key=3069f22678bbd89bd4a6d2cb0c249e34"

https://www.themoviedb.org/authenticate/fdd4452cf6eec5d78e81a9bc286fc2e9e144df59

curl -X POST "https://api.themoviedb.org/3/authentication/session/new?api_key=3069f22678bbd89bd4a6d2cb0c249e34" \
  -H "Content-Type: application/json" \
  -d '{"request_token":"fdd4452cf6eec5d78e81a9bc286fc2e9e144df59"}'
  - returns `{"success":true,"session_id":"e7998e4d72f207a042376f958ff12ae43b47f6c0"}`

GET https://api.themoviedb.org/3/account?api_key=3069f22678bbd89bd4a6d2cb0c249e34&session_id=e7998e4d72f207a042376f958ff12ae43b47f6c0

```
hello there: HTTP/1.1 401 Unauthorized
Alt-Svc: h3=":443"; ma=86400
Connection: keep-alive
Content-Encoding: gzip
Content-Type: application/json;charset=utf-8
Date: Sun, 01 Jun 2025 11:50:13 GMT
Server: openresty
Transfer-Encoding: chunked
Vary: accept-encoding, Origin
Via: 1.1 0ed0b3a1a3e8908d48a47272b433d54e.cloudfront.net (CloudFront)
X-Amz-Cf-Id: L_lMtaKpYYOOkXfuj3ILpdZQmprbdr98Uggv0QafCgP_XjWwp3fR1w==
X-Amz-Cf-Pop: LHR50-P2
X-Cache: Error from cloudfront
x-az: us-east-1b
x-task-id: c48e0a81c0964062b5f95e61bfbac73e

{
    "status_code": 3,
    "status_message": "Authentication failed: You do not have permissions to access the service.",
    "success": false
}
```
- fuck

- get the movie ID:
http GET "https://api.themoviedb.org/3/search/movie?query=Lilo%20and%20Stitch" \
  "Authorization: Bearer eyJhbGciOiJIUzI1NiJ9.eyJhdWQiOiIzMDY5ZjIyNjc4YmJkODliZDRhNmQyY2IwYzI0OWUzNCIsIm5iZiI6MTc0ODc3NDkzMC40NjQsInN1YiI6IjY4M2MzMDEyZTg2NTYzYjgxMWQyMTIxZSIsInNjb3BlcyI6WyJhcGlfcmVhZCJdLCJ2ZXJzaW9uIjoxfQ.5GIHNExqKPBkcsqbcE414OqC3NpEFgMIgB-4QBAC-Tk"
  > 552524
- Get your account Id:
  - curl "https://api.themoviedb.org/3/account?api_key=3069f22678bbd89bd4a6d2cb0c249e34&session_id=e7998e4d72f207a042376f958ff12ae43b47f6c0"
    - {"avatar":{"gravatar":{"hash":"b0c996a676932046ee8f310136e1c878"},"tmdb":{"avatar_path":null}},"id":22048510,"iso_639_1":"en","iso_3166_1":"GB","name":"","include_adult":false,"username":"sandyPunandi"}
    - "id":22048510
- send a post:
  - curl -X POST "https://api.themoviedb.org/3/account/22048510/watchlist?api_key=3069f22678bbd89bd4a6d2cb0c249e34&session_id=e7998e4d72f207a042376f958ff12ae43b47f6c0" \
  -H "Content-Type: application/json" \
  -d '{"media_type": "movie", "media_id": 11544, "watchlist": true}'
    - response: `{"success":true,"status_code":1,"status_message":"Success."}`

Movie 2: Sikandar

- get the movie ID:
http GET "https://api.themoviedb.org/3/search/movie?query=Sikandar" \
  "Authorization: Bearer eyJhbGciOiJIUzI1NiJ9.eyJhdWQiOiIzMDY5ZjIyNjc4YmJkODliZDRhNmQyY2IwYzI0OWUzNCIsIm5iZiI6MTc0ODc3NDkzMC40NjQsInN1YiI6IjY4M2MzMDEyZTg2NTYzYjgxMWQyMTIxZSIsInNjb3BlcyI6WyJhcGlfcmVhZCJdLCJ2ZXJzaW9uIjoxfQ.5GIHNExqKPBkcsqbcE414OqC3NpEFgMIgB-4QBAC-Tk"
  > 1257960
- POST with HTTPie this time:

http POST "https://api.themoviedb.org/3/account/22048510/watchlist" \
  api_key==3069f22678bbd89bd4a6d2cb0c249e34 \
  session_id==e7998e4d72f207a042376f958ff12ae43b47f6c0 \
  Content-Type:application/json \
  media_type=movie media_id:=1257960 watchlist:=true

Movie 3: Fountain of Youth

id = 1098006

http POST "https://api.themoviedb.org/3/account/22048510/watchlist" \
  api_key==3069f22678bbd89bd4a6d2cb0c249e34 \
  session_id==e7998e4d72f207a042376f958ff12ae43b47f6c0 \
  Content-Type:application/json \
  media_type=movie media_id:=1098006 watchlist:=true
  
## REFERENCE
## [HTTP Response Headers](https://launchschool.com/books/working_with_apis/read/http_response_headers)
 
 - This page is a list of the most common HTTP response headers:

### [Access-Control-Allow-Origin](https://launchschool.com/books/working_with_apis/read/http_response_headers#accesscontrolalloworigin)

- gives a list of what domains can access this resource using CORS.
- THis would allow all sites: `Access-Control-Allow-Origin: *`

### [Allow](https://launchschool.com/books/working_with_apis/read/http_response_headers#allow)

- lists what methods are allowed

### [Content-Length](https://launchschool.com/books/working_with_apis/read/http_response_headers#contentlength)

- length of the response body in bytes

### [Content-type](https://launchschool.com/books/working_with_apis/read/http_response_headers#contenttype)

describes the media-type or format of the body

### [ETag](https://launchschool.com/books/working_with_apis/read/http_response_headers#etag)

- used to specify a version of a resource

### [Last-Modified](https://launchschool.com/books/working_with_apis/read/http_response_headers#lastmodified)

The last time the request was modified.

### [WWW-Authenticate](https://launchschool.com/books/working_with_apis/read/http_response_headers#wwwauthenticate)

- what type of authentication is required to access a resource

### [X-* Headers](https://launchschool.com/books/working_with_apis/read/http_response_headers#xheader)

-lists headers beginning with x. If they begin with x it means they're not standardized headers, often API specific.

### [5	Network Programming in JavaScript](https://launchschool.com/lessons/1b723bd0/assignments/9d538701)

- ok
- AJAX is good for hovering over a word and a popup telling us about that word
- Or for when a loarge page has a form. If yiou submit it with an error, you won't want to reload the whole page

#### What is the advantage of using AJAX over letting the browser automatically visit links and submit forms? (LSBot refined)

- it only effects the relevant part of the page and does not reload the whole page.
- unlike requests from the browser it is free to ignore responses
- there are more HTTP methods available to AJAX (such as PUT, DELETE, and PATCH.

#### Single Page Applications

- These days making extra HTML requests outside the main HTML page load is super common, and indeed some modern apps fetch data in a serialized format stitching the DOM together just from those. These are "Single-page applications" because they often run entirely within one HTML page. So this model does each interaction by passing data to and from the server, usually encoded as JSON

### [6	Making a Request with Fetch	](https://launchschool.com/lessons/1b723bd0/assignments/9beef168)

- used to do this with `XMLHttpRequest` object. That used callbacks, but this is...
- promise-based.

#### Imagine we need to access some JSON data that's provided by an API endpoint. What are the three basic steps required to do so using fetch? You may assume we do so using a GET request. (LSbot approveded)

1. Send a fetch request with a url and an optional configuration object to provide method, headers and body
2. Capture the promise returned that resolves to a Response object
3. Use asynchronous methods such as `.json` and `.text` to handle the response data (also provide a catch statement to handle any errors).

#### Making a Simple Request

- the browser provides a `fetch` method (on `Window`)
- It takes a resource as an argument
- it initiates a GET request adn retuns a promise that is fulfilled once the response is available.

#### Fetch's Response Object

- table of response properties:
  - .body
  - .bodyUsed
  - .headers
  - .ok
  - .redirected
  - .status
  - .statusText
  - .type
  - .url

#### Reading Response Data

- all these useful methods ARE ASYNCHRONOUS!
- the response body is in plain text. Use `response.text()`.
- Also:
  - `.text()`
  - `.json()`
  - `.blob()`
- You can only consume the body of a response once. Hence teh `bodyUsed` property. If you need to use it more than once, use `response.clone()`

#### Configuring Fetch Requests

- fetch can take a 2nd parameter: `options`
- `options` is an object.
- The most common kv pairs in `options` are:
  - `method` => a string of the HTTP method, defaults to GET
  - `body` -> the request body, data-type is flexible, could be a string or other data types
  - `headers` -> an obejct literal, keys are headers, values are header values

```javascript
fetch('https://my_blog.com/api/entries', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json'
  },
  body: JSON.stringify({ date: '2025-04-25', entry: 'Perfect day! Not too hot, not too cold'})
})
```

### [7	Data Serialization](https://launchschool.com/lessons/1b723bd0/assignments/8da76880)

- review data-serialization in API book. tick

#### Request Serialization Formats

yup

##### Query String / URL Encoding

```
GET /path?title=Do%20Androids%20Dream%20of%20Electric%20Sheep%3F&year=1968 HTTP/1.1
Host: example.test
Accept: */*
```

#### Multipart Forms

- a boundry delimiter seperates the parts of the request:
- `Content-Type: multipart/form-data; boundary=----WebKitFormBoundarywDbHM6i57QWyAWro`
- looks like this:
```javascript
POST /path HTTP/1.1
Host: example.test
Content-Length: 267
Content-Type: multipart/form-data; boundary=----WebKitFormBoundarywDbHM6i57QWyAWro
Accept: */*

------WebKitFormBoundarywDbHM6i57QWyAWro
Content-Disposition: form-data; name="title"

Do Androids Dream of Electric Sheep?
------WebKitFormBoundarywDbHM6i57QWyAWro
Content-Disposition: form-data; name="year"

1968
------WebKitFormBoundarywDbHM6i57QWyAWro--
```

#### JSON Serialization

```
POST /path HTTP/1.1
Host: example.test
Content-Length: 62
Content-Type: application/json; charset=utf-8
Accept: */*

{"title":"Do Androids Dream of Electric Sheep?","year":"1968"}
```

###### content-type and charset

- charset is optional, but best-practice

### [8	Example: Loading HTML via Fetch](https://launchschool.com/lessons/1b723bd0/assignments/b781aaa3)

```javascipt
document.addEventListener('DOMContentLoaded', async () => {
  let store = document.getElementById('store');
  const BASE_URL = 'https://ls-230-web-store-demo.herokuapp.com';
  
  async function loadContent(url) {
    let response = await fetch(url);
    store.innerHTML = await response.text();
  }

  await loadContent(`${BASE_URL}/products`);

  store.addEventListener('click', async event => {
    let target = event.target;
    if (target.tagName !== "A") {
      return;
    }

    event.preventDefault();
    let url = BASE_URL + target.getAttribute('href');
    await loadContent(url)
  })
});
```

#### Summary

- we can use the `fetch` API to fetch content and insert it into an existing web-page without a full page reload
- We can attach event listeners to dynamically loaded content and use `fetch` to handle navigation and interactions without reloading the entire page.

#### Problem

1. LS answer: "The form submits a POST request to http://s.codepen.io/products/<product id> and receives a 404 response"

### [9	Example: Submitting a Form via Fetch](https://launchschool.com/lessons/1b723bd0/assignments/85a3754f)	

3 steps:
1. serialize the data
2. send the request with `fetch`
3. handle the response

- `Accept` defaults to `*/*`

#### URL-encoding POST Parameters

```javascipr
async function postData() {
  let data = 'title=Effective%20JavaScript&author=David%20Herman';

  let response = await fetch('https://lsjs230-book-catalog.herokuapp.com/books', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/x-www-form-urlencoded',
    },
    body: data,
  });

  if (response.status === 201) {
    console.log(`This book was added to the catalog: ${await response.text()}`);
  }
}

postData();
```

#### Submitting a Form

- `HTMLFormElement.elements`
- what the fuck is `encodeURIComponent(element.name)` ?

```javascript
let form = document.getElementById('form');
let messageDiv = document.getElementById('message');

form.addEventListener('submit', event => {

  event.preventDefault();
  messageDiv.style.display = 'none';

  let keysAndValues = [];

  for (let index = 0; index < form.nextElementSibling.clientHeight; index += 1) {
    let element = form.elements[index];
    let key;
    let value;

    if (element.type !== 'submit') {
      key = encodeURIComponent(element.name);
      value = encodeURIComponenet(element.value);
      keysAndValues.push(`${key}=${value}`);
    }
  }

  let data = keysAndValues.join('&');

  async function submitFormData(form, data) {
    let response = await fetch(`https://lsjs230-book-catalog.herokuapp.com/${form.getAttribute('action')}`, {
      method: form.method,
      headers: {
        'Content-Type': 'application/x-www-form-urlencoded',
      },
      body: data,
    });

    if (response.status === 201) {
      messageDiv.textContent = `This book was added to the catalog: ${await response.text()}`;
      messageDiv.style.display = 'block';
    }
  }
  submitFormData(form, data);
});
```

#### Submitting a Form with FormData

- the process above is manual and error prone - so i gues I don't need to pay it toomuch attention.
- `FormData` only uses input fields with a `name` attribute.
- makes serializing data easy.
- don't need headers as the browser automatically fills it in.

#### Problems

1. I got like 2 lines, but mainly am feeling rather lost:

```javascript
document.addEventListener('DOMContentLoaded', async () => {
  let store = document.getElementById('store');
  const BASE_URL = 'https://ls-230-web-store-demo.herokuapp.com';
  
  async function loadContent(url) {
    let response = await fetch(url);
    store.innerHTML = await response.text();
  }

  await loadContent(`${BASE_URL}/products`);

  store.addEventListener('click', async event => {
    event.preventDefault();

    let form = event.target;
    let data = new FormData(form);
    let url = `https://ls-230-web-store-demo.herokuapp.com${form.getAttribute('action')}`;

    let response = await fetch(url, {
      method: 'POST', 
      body: data,
    });

    store.innerHTML = await response.text();
  });
});
```
- ok I mistakenly thought I was meant to replace the click event, but I actually ahd to add another submit event.

2.
```javascript
    let response = await fetch(url, {
      method: 'POST',
      body: data,
      headers: {
        Authorization: 'token AUTH_TOKEN',
      },
    });

```

- tricky

### [10	Example: Loading JSON via Fetch](https://launchschool.com/lessons/1b723bd0/assignments/cf01ccbf)

- Well, apparently so far we have learnt how to retrieve HTML fragments from a server and insert them into a page....
- sometimes you just need to load data in a primitive structure and render it with the client-side code.

###### Server-side rendering v Client side rendering
- SSR: building a complete web-page on the server side and sending that page to the client (web-browser)
- CSR: server sends the bare-bones HTML document and some JS to the client. The client then executes the JS code which requests the data it needs and dynamically fills out the rest of the page..

- various asynchronous response handling methods a `response` object provides:
  - `.text()`
  - `.json()`
  - `.blob()`

#### Problems

1. I FUCKING NAILED IT BROOOOOO

```javascript
(async function fetchFromGH() {
  let response = await fetch('https://api.github.com/repos/rails/rails');
  let json = await response.json();
  console.log(`Status: ${response.status}`);
  console.log(`open issues: ${json.open_issues}`);
})();
```

2. 
nailed it again
```javascript
(async function fetchFromGH() {
  try {
    // let response = await fetch('https://api.github.com/repos/rails/rails');
    let response = await fetch('hts://api.github.com/repos/rails/rails');
    let json = await response.json();
    console.log(`Status: ${response.status}`);
    console.log(`open issues: ${json.open_issues}`);
  } catch {
    console.error('The request could not be completed!')
  }
})();
```

### [11	Example: Sending JSON via Fetch](https://launchschool.com/lessons/1b723bd0/assignments/ae60c1cd)

- sending JSON data to a server is like submitting a form -> you use the same 3 steps with a few tweaks.
  - serialise the data into valid JSON
  - Send the request using `fetch` with a header of:
    - `Content-Type: application/json; charset=utf-8`
  - Handle the response.

###### Step 1: serializing Data to JSON:
  - this is a basic POST from javascript:

```javascript
fetch('https://lsjs230-book-catalog.herokuapp.com/books', {
  method: 'POST',
  body: 'title=Eloquent%20JavaScript&author=Marijn%20Haverbeke',
});
```

- this is how it should be sent as JSON format:

```javascript
let data = { title: 'Eloquent JavaScript', author: 'Marijn Haverbeke' };
let json = JSON.stringify(data);

fetch('https://lsjs230-book-catalog.herokuapp.com/books', {
  method: 'POST',
  body: json,
});
```

- the serialized JSON looks like this: `{"title": "Eloquent JavaScript", "author": "Marijn Haverbeke"}`

#### Step 2: Setting the Content-Type Header:

- might be not neccesary, do it anyway to be safe.

```javascript
let data = { title: 'Eloquent JavaScript', author: 'Marijn Haverbeke' };
let json = JSON.stringify(data);

fetch('https://lsjs230-book-catalog.herokuapp.com/books', {
  method: 'POST',
  headers: {
    'Content-type': 'application/json; charset=utf-8',
  },
  body: json,
});
```

#### Step 3: Handling the Response

```javascript
  let response = await fetch('https://lsjs230-book-catalog.herokuapp.com/books', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json; charset=utf-8',
    },
    body: json,
  });

  if (response.status === 201) {
    console.log(`This book was added to the catalog: ${await response.text()}`);
  }
```

#### Problems

1.
2. Mistakes:
  - I included `'Authorization': 'token AUTH_TOKEN',` with Authorisation not a string, but a variable
  - I didn't convert the response into JSON with `const data = await response.json();`
  - I didn't use `JSON.stringify()` to convert the response into readbale format:
    - `  console.log(`This product was added: ${JSON.stringify(data)}`)`

### [12	Cross-Domain Requests with Fetch and CORS](https://launchschool.com/lessons/1b723bd0/assignments/25da79fd)

#### Cross-Origin requests with Fetch

- cross origin request means the page is trying to access a resource from a different origin.
-example: we are on this page: `http://mysite.com/doc1`, it's a cross origin request if:
  - the scheme/protocol is different: `https://mysite.com/doc1`
  - the port is different: `http://mysite.com:4000/doc1`
  - The host is different: `http://anothersite.com/doc1` (different host)
- we're going to be focusing on cross-domain requests so a request from one domain host name to another domain.
- These present security risks that attackers can exploit. It might be by fooling the user into clicking on a link that sends a request to an application with the user's credentials. (they are XSS and CSRF attacks, but we won't cover them here)

#### Cross-Origin Requests
- by default `fetch` can't retrieve cross-origin resources.
- All browsers enforce 'same-origin policy'
- We get around it with CORS

#### CORS

- It's a W3C sepcification that defines how browsers and servers must communicate when accessing resources across origins.
- origin header.
- Let the 2 systems know enough about each other to determine whether the response should be allowed to happen.
- Every `fetch` request must have an `Origin` header that contains the origin of the requesting page.
- The browser automatically adds the `Origin` header as part of a `fetch` request
- So if we send a request from `http://localhost:8080`, the browser adds this header `Origin: http://localhost:8080`
- If the request is to be granted the response looks like this:
  ```Access-Control-Allow-Origin`
- If the server wants to make a resource avaialable to everyone it can send the header with a value of `*`:
  - `Access-Control-Allow-Origin: *`
  - Even if the response is totally kosher, but it does not have an appropriate `Access-Control-Allow-Origin` header and value, it will raise an error and not let the script access the response.

#### CORS in action

- This isn't working for me. In theory it seems quite straight forward though.

### CORS Explanation

What constitutes a cross-origin request:

A cross-origin request occurs when a web page attempts to access a resource from a different origin than the one serving the page. An origin is defined by the combination of scheme (protocol), host (domain), and port. If any of these three components differ between the requesting page and the target resource, it's considered a cross-origin request.
For example:
•   A page at https://myapp.com requesting data from https://weather-api.com (different host)
•   A page at http://localhost:8080 requesting from http://localhost:3000 (different port)
•   A page at http://example.com requesting from https://example.com (different scheme)

The security concern that CORS addresses:

CORS addresses critical security vulnerabilities, particularly Cross-Site Request Forgery (CSRF) and certain Cross-Site Scripting (XSS) attacks. Without proper controls, malicious websites could exploit a user's authenticated sessions with other sites to make unauthorized requests on their behalf. For instance, a malicious site could attempt to access your banking information or make transactions using your existing login credentials from another tab.

The role of the same-origin policy:

The same-origin policy is a fundamental security concept implemented by web browsers that restricts how documents or scripts from one origin can interact with resources from another origin. By default, this policy blocks cross-origin requests made by JavaScript (including fetch and XMLHttpRequest). This serves as the first line of defense, preventing potentially malicious cross-origin access attempts.
CORS provides a controlled mechanism to selectively relax the same-origin policy when cross-origin access is legitimately needed, allowing servers to explicitly specify which origins are permitted to access their resources through specific HTTP headers.

#### Conclusion

- gives us a way to complete legitimate cross-origin requests withou the security problems associated with cross-origin requests.

### [13	Legacy JavaScript - jQuery and XMLHttpRequest](https://launchschool.com/lessons/1b723bd0/assignments/84d25145)

- There's lots of tools we don't use now, but you will come across. they won't be hard to learn when you need to.

#### jQuery

- "vanilla solution" or "jQuery" solution.
- Recognise it by the `$` symbols sprinkled around.

```javascript
// Vanilla JS
document.querySelector("button").addEventListener("click", function() {
  alert("Button clicked!");
});

// jQuery
$("button").click(function() {
  alert("Button clicked!");
});
```

#### XMLHttpRequest (XHR)

##### A Basic GET Request

```javascript
// XMLHttpRequest
document.addEventListener("DOMContentLoaded", () => {
  let request = new XMLHttpRequest();
  request.open("GET", "https://ls-230-web-store-demo.herokuapp.com/products");

  request.addEventListener("load", () => {
    let store = document.getElementById("store");
    store.innerHTML = request.response;
  });

  request.send();
});

// Equivelant request using Fetch
document.addEventListener('DOMContentLoaded', async () => {
  let store = document.getElementById('store');
  let response = await fetch('https://ls-230-web-store-demo.herokuapp.com/products');
  store.innerHTML = await response.text();
});
```

- you don't need to commit any of this to memory, but it;'s good to know.

### [14	Project: Search Autocomplete, Part 1 - Introduction and Setup	](https://launchschool.com/lessons/1b723bd0/assignments/b9691285)

- We'll build a small Javascript app that communicates with the server in response to user interactions.

#### Introduction

- auto-complete
- Fosuc on how to use the API to power the front-end component you're building.
- Three key take-aways:
  - as a front-end dev you'll be working with APIs
  - You won't always be working from scratch, so imagine you're on a team and you have to develop the front-end compoent of the web-app
  - Notice the back-end uses `node` and `express`. This isn't a requirement, the code we write here should work with any server as long as it has the same API.

#### Server Setup

`http://localhost:3000/countries?matching=ca`

### [15	Project: Search Autocomplete, Part 2 - Setting up the UI](https://launchschool.com/lessons/1b723bd0/assignments/e784e7dd)


#### Setting up the UI 

- done

### [16	Project: Search Autocomplete, Part 3 - Talking to the server](https://launchschool.com/lessons/1b723bd0/assignments/0be1e6a9)

#### Binding Events

- yup

#### Communicating With the Server

```javascript
  fetchMatches: async function(query) {
    let response = await fetch(`${this.url}${encodeURIComponent(query)}`);
    let data = await response.json();
    return data;
  },
```
#### Rendering List of Countries

```javascript
  draw: function() {
    while (this.listUI.lastChild) {
      this.listUI.removeChild(this.listUI.lastChild);
    }

    if (!this.visible) {
      this.overlay.textContent = '';
      return;
    }

    this.matches.forEach(match => {
      let li = document.createElement('li');
      li.classList.add('autocomplete-ui-choice');

      li.textContent = match.name;
      this.listUI.appendChild(li);
    });
  },
```

#### Resetting the UI

```javascript
const Autocomplete = {
  // ...

  reset: function() {
    this.visible = false;
    this.matches = [];

    this.draw();
  },

  // ...
};
```

#### Rendering Overlay Content



#### Improving the Overlay Content

- yup

### [17	Project: Search Autocomplete, Part 4 - Improving user experience](https://launchschool.com/lessons/1b723bd0/assignments/d9af6b23)

#### Going Up and Down the List

- highlight a country by pressing up or down.

#### Tab Completion


- press tab, it completes with the highlighted option

#### Hide the List and Revert to User Input

- keeping track of what the user typed before
- I'm not sure this is working as it's meant to be...

#### Selecting a Country by Clicking

- got parts of it, but not all.
  - wins:
    - i identified event.target as the thing I needed
    - I knew to add a line to our `bindEvents()` method:
```javascript
bindEvents: function() {
    this.input.addEventListener('input', this.valueChanged.bind(this));
    this.input.addEventListener('keydown', this.handleKeydown.bind(this));
    this.listUI.addEventListener('mousedown', this.handleMousedown.bind(this));
  },
```
- Learnt
  - we used a 'mousedown event' rather than a click event:
    - `this.listUI.addEventListener('mousedown', this.handleMousedown.bind(this));`
  -
### [18	Project: Search Autocomplete, Part 5 - Throttling Fetch requests](https://launchschool.com/lessons/1b723bd0/assignments/72dd3b59)

#### The Problem

- we're sending too many requests
  
#### The Solution: Throttling

 wait a bit before sending the request

#### Implementing debounce

- a function that takes care of this. Takes a callback and a delay in ms.
- Cool!

##### Making the Code Reusable

- 

### [19	Summary](https://launchschool.com/lessons/1b723bd0/assignments/7729a45a)	

-ok

### [20 Quiz](https://launchschool.com/quizzes/eb268892)

- 1st try 2/10
- 2nd try: 6/10
1.  A -> tick
2.  B -> tick
3.  C, D -> tick
4.  A, B, C, D -> not B or D
5.  B, C, -> actually A and B
6.  A -> nope, C and D
- `get data returns a prommise
- JSON.parse is unecessarty and leads to an error.
7.  C, D -> tick
8.  A, B, C, D -> not C I'm afraid. we don;t know that the response will contain JSON
9.  A -> tick
10.  D -> tick

## [4	Putting it All Together](https://launchschool.com/lessons/32b572ea/assignments)

### [1	Introduction](https://launchschool.com/lessons/32b572ea/assignments/bcd772c9)	

- yup

### [2	Project: Photo Gallery, Part 1 - Introduction and Server Setup](https://launchschool.com/lessons/32b572ea/assignments/96518ba5)

- handle events
- make Ajax requests
- update the DOM
- create a photo gallery with data received from our API server
- like/favourite/comment on photos
- render comments below slide-show
- send new comments ack vie AJAX
- When the slideshow advances (?) the details of the photo and the comments are re-rendered to match the visible image (?)

#### Server Set up
#### HTML & CSS
### [3	Project: Photo Gallery, Part 2 - Fetch Data and Render on Page Load	](https://launchschool.com/lessons/32b572ea/assignments/584a0789)
##### Templates
#### Step 1: Fetch Photos Data and Render Photos on Page Load
#### Step 2: Render Comments for the First Photo
### [4	Project: Photo Gallery, Part 3 - Slide Show](https://launchschool.com/lessons/32b572ea/assignments/67707770)
##### Step 3: Create the Slide Show Functionality
### [5	 Project: Photo Gallery, Part 4 - Like, Favorite, and Comment](https://launchschool.com/lessons/32b572ea/assignments/3c346f4c)
#### Step 4: Like and Favorite a Photo
#### Step 5: Add a New Comment for a Photo

- I did this project, looking at the LS solution, not really feeling like I had enough time to devote more than 15 minutes to each solution. Have I internalised enough of it?

### [6	Project: Booking App, Part 1 - Introduction and Server Setup](https://launchschool.com/lessons/32b572ea/assignments/40b0794b)
- 15.6.25
##### App Description
##### App Specifications

- 4 resources/ specifications:
  - Staff members:
    - ID
    - name
    - email
  - Schedules
    - ID
    - Staff ID
    - Date
    - Time
    - Student Email
  - Students
    - Id
    - Name
    - Email
  - Booking sequences
    - ID
    - Student Email
    - Sequence

- staff members can provide time-slots
- Schedules can be booked by students
- A schedule is booked if there is a value for the student email
- Students can bok a schedule
- A student who wants to register must have a booking sequence
- Students get a bookig sequence when they first try to book a schedule and are not yet in the database.

##### Getting to Know the Booking App

1. How many staff are there?
  - 5
2. How many students are there?
  - 5
3. How many schedules exist?
  -  7
    -  incorrect, 9. I misread what I though was the final entry
4. How many schedules have bookings?
  - 3
5. Do all staff have schedules?
  - no (there is no path for this but it is inferable from the info above)
6. Did all students book a schedule?
  - no, same reason as above

### [7	Project: Booking App, Part 2 - Getting Schedules	](https://launchschool.com/lessons/32b572ea/assignments/de4a7522)

### [8	Project: Booking App, Part 3 - Adding Staff](https://launchschool.com/lessons/32b572ea/assignments/94801ccf)	
### [9	Project: Booking App, Part 4 - Adding Schedules](https://launchschool.com/lessons/32b572ea/assignments/2518c13d)
### [10	Project: Booking App, Part 5 - Booking Time Slots](https://launchschool.com/lessons/32b572ea/assignments/c5488715)	
### [11	Project: Booking App, Part 6 - Viewing Bookings](https://launchschool.com/lessons/32b572ea/assignments/f31a1f08)	
### [12	 Project: Booking App, Part 7 - Cancellations](https://launchschool.com/lessons/32b572ea/assignments/d5166c30)	
### 13	JS235 Course Feedback

## Assessment
### [1	Assessment Format](https://launchschool.com/lessons/ee8a7b70/assignments/7eedfff9)
#### Part 1 - The Written Quiz
#### Part 2 - The Interview
#### Part 3 - The Take Home Project
  - 3 days to complete.
### [2	Study Guide](https://launchschool.com/lessons/ee8a7b70/assignments/ebb356f3)
#### Study Topics
- DOM Traversal and Manipulation
- Events and Event Handling
- Asynchronous JavaScript
  - Callback-based Asynchronous JavaScript
  - The Promise API
  - async/await
  - setTimeout, setInterval
  - The fetch API
  - Error Handing with Asynchronous Code
- Communicating with the server through fetch and rendering the response to the page
### [3	Part 1: Tips for the JS239 Quiz](https://launchschool.com/lessons/ee8a7b70/assignments/2182dc64)
  - A mixture of theory and practical questions.
#### Online resources
#### Additional tips

### [4	Part 1: Start the JS239 Quiz](https://launchschool.com/lessons/ee8a7b70/assignments/9e8f3b11)
  - revise:
    -  the DOM
    -  the Event Model
    -  Asynchronous code
    -  AJAX with `fetch`
  -  3 and a half hours to complete
-  YOU CAN SKIP THIS (but i won't)
### 5	Part 2: Tips for the JS239 Interview
### 6	Part 2: Schedule a JS239 Interview
### 7	JS239 Interview Feedback
### 8	Part 3: Tips for the Take Home Project
### 9	 Part 3: Practice Project
### 10	Part 3: The Take Home Project
### 11	Project Feedback
