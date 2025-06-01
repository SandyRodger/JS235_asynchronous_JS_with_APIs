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

- this method is always called regardless of whether the primise was fulfilled or not.

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


### [7	Assignment: Updating User Information App with Promises	](https://launchschool.com/lessons/701aaae0/assignments/934f5046)

- skip for now

### [8	Promise API	](https://launchschool.com/lessons/701aaae0/assignments/db7a742f)

#### `promise.all()`
#### `promise.race()`
#### `promise.allSettled()`
#### `promise.any()`

### [9	Practice Problems: Promise API](https://launchschool.com/lessons/701aaae0/assignments/91f24c6e)

- skip for now

### [10	Async / Await](https://launchschool.com/lessons/701aaae0/assignments/3ce8eb42)

#### async Functions
#### await Keyword
#### Using await Without a Promise
#### Handling Multiple Asynchronous Tasks
### [11	Practice Problems: Async / Await](https://launchschool.com/lessons/701aaae0/assignments/aa5dfac0)

### [12	Error Handling with Async / Await](https://launchschool.com/lessons/701aaae0/assignments/49ba5b39)

#### Error Propagation
#### Catching Errors from Multiple Operations
#### Catching Specific Errors
#### Clean up using finally
### [13	Practice Problems: Error Handling with Async / Await](https://launchschool.com/lessons/701aaae0/assignments/f4edd69b)

### [14	Assignment: Updating User Information App with `async` / `await`](https://launchschool.com/lessons/701aaae0/assignments/d0fc9896)

### [15	Combining Async / Await with Promise Methods](https://launchschool.com/lessons/701aaae0/assignments/e02110a7)

### [16	Summary](https://launchschool.com/lessons/701aaae0/assignments/b8645009)
#### Conclusion
### 17	Quiz

## [3	Making HTTP Requests from JavaScript](https://launchschool.com/lessons/1b723bd0/assignments)


### [1	Introduction](https://launchschool.com/lessons/1b723bd0/assignments/ff3903a8)

- `fetch` browser APIfor making netweord requests

### [2	What to Focus On](https://launchschool.com/lessons/1b723bd0/assignments/d9f41631)	

- Think HTTP
- Be Familiar with Fetch
- Understand How To Serialize Data
- Decompose Functionality into API Requests
- Read API Documentation

### [3	HTTP Review](https://launchschool.com/lessons/1b723bd0/assignments/ad1bafe7)	
### [4	Book: Working with Web APIs](https://github.com/SandyRodger/launch_school_books/edit/main/working_with_web_APIs.md)

### Working with APIs book
## [Working with Web APIs](https://launchschool.com/books/working_with_apis)
### Introduction
#### What this Book Covers
### [Preparations](https://launchschool.com/books/working_with_apis/read/preparations)
##### HTTPie Option Reference
### [Using Postman](https://launchschool.com/books/working_with_apis/read/using_postman#makingarequest)
#### Checking the Weather
#### Summary
### Defining API
#### [What is an API?](https://launchschool.com/books/working_with_apis/read/defining_api#whatisanapi)
#### [Web APIs](https://launchschool.com/books/working_with_apis/read/defining_api#webapis)
#### [Provider and Consumer](https://launchschool.com/books/working_with_apis/read/defining_api#provider_and_consumer)
#### [What about Clients and Servers?](https://launchschool.com/books/working_with_apis/read/defining_api#clientsandservers)
#### [Summary](https://launchschool.com/books/working_with_apis/read/defining_api#summary)
### [What APIs Can Do](https://launchschool.com/books/working_with_apis/read/what_apis_can_do)
#### [Sharing Data](https://launchschool.com/books/working_with_apis/read/what_apis_can_do#sharingdate)
#### [Enabling Automation](https://launchschool.com/books/working_with_apis/read/what_apis_can_do#enablingautomation)
#### [Leverage Existing Services](https://launchschool.com/books/working_with_apis/read/what_apis_can_do#leverageexistingservices)
#### [Summary](https://launchschool.com/books/working_with_apis/read/what_apis_can_do#summary)
### [Accessibility](https://launchschool.com/books/working_with_apis/read/accessibility)
#### [Public and Private](https://launchschool.com/books/working_with_apis/read/accessibility#publicandprivate)
###### Public APIs
###### Private APIs
#### [Terms and conditions](https://launchschool.com/books/working_with_apis/read/accessibility#termsandconditions)
#### [Summary](https://launchschool.com/books/working_with_apis/read/accessibility#summary)
### [A Review of HTTP](https://launchschool.com/books/working_with_apis/read/a_review_of_http)
#### [Request and Response](https://launchschool.com/books/working_with_apis/read/a_review_of_http#requestandresponse)
#### [Headers](https://launchschool.com/books/working_with_apis/read/a_review_of_http#headers)
#### [Body](https://launchschool.com/books/working_with_apis/read/a_review_of_http#body)
#### [Summary](https://launchschool.com/books/working_with_apis/read/a_review_of_http#summary)
### [A Review of URLs](https://launchschool.com/books/working_with_apis/read/a_review_of_urls)
#### [URL or URI](https://launchschool.com/books/working_with_apis/read/a_review_of_urls#url_or_uri)
#### The parts of a URL
#### [Identifiers in Paths](https://launchschool.com/books/working_with_apis/read/a_review_of_urls#identifiers_in_paths)
#### [Summary](https://launchschool.com/books/working_with_apis/read/a_review_of_urls#summary)
### [Media Types](https://launchschool.com/books/working_with_apis/read/media_types)
#### Data-serialization
#### XML
#### [JSON](https://launchschool.com/books/working_with_apis/read/media_types#json)
#### [Summary](https://launchschool.com/books/working_with_apis/read/media_types#summary)
### [REST and CRUD](https://launchschool.com/books/working_with_apis/read/rest_and_crud)
#### [What is REST?](https://launchschool.com/books/working_with_apis/read/rest_and_crud#rest)
#### [CRUD](https://launchschool.com/books/working_with_apis/read/rest_and_crud#crud)
#### [A RESTful API Template](https://launchschool.com/books/working_with_apis/read/rest_and_crud#restfulapitemplate)
#### [Resource-Oriented Thinking](https://launchschool.com/books/working_with_apis/read/rest_and_crud#resourceorientedthinking)
#### Conventions, not Rules
#### Summary
## WORKING WITH AN API
### [Fetching Resources](https://launchschool.com/books/working_with_apis/read/fetching_resources#serversetup)
#### [Server setup](https://launchschool.com/books/working_with_apis/read/fetching_resources#serversetup)
#### [Fetching a Resource](https://launchschool.com/books/working_with_apis/read/fetching_resources#fetchingaresource)
#### [What is a Resource?](https://launchschool.com/books/working_with_apis/read/fetching_resources#what_is_a_resource)
#### [Fetching a collection](https://launchschool.com/books/working_with_apis/read/fetching_resources#fetching_a_collection)
#### [Elements and Collections](https://launchschool.com/books/working_with_apis/read/fetching_resources#elements_and_collection)
##### How to know if a URL is for a collection or an element?
#### [Summary](https://launchschool.com/books/working_with_apis/read/fetching_resources#summary)
### [Requests in Depth](https://launchschool.com/books/working_with_apis/read/requests_in_depth)
#### [GET and POST](https://launchschool.com/books/working_with_apis/read/requests_in_depth#get_and_post)
#### [Parts of a Request](https://launchschool.com/books/working_with_apis/read/requests_in_depth#parts_of_a_request)
#### [Summary](https://launchschool.com/books/working_with_apis/read/requests_in_depth#summary)
### [Creating Resources](https://launchschool.com/books/working_with_apis/read/creating_resources)
#### [HTTP Request Side Effects](https://launchschool.com/books/working_with_apis/read/creating_resources#http_request_side_effects)
#### [Creating a resource](https://launchschool.com/books/working_with_apis/read/creating_resources#creating_a_resource)
#### [Handling Errors](https://launchschool.com/books/working_with_apis/read/creating_resources#handling_errors)
###### Missing or Invalid Parameters
###### Missing resources
###### Authentication
###### Incorrect Media Type
###### Rate limiting
###### Server Errors
#### [Summary](https://launchschool.com/books/working_with_apis/read/creating_resources#summary)
### [More HTTP Methods](https://launchschool.com/books/working_with_apis/read/more_http_methods)
#### [Updating a resource](https://launchschool.com/books/working_with_apis/read/more_http_methods#updating_a_resource)
#### [Deleting a Resource](https://launchschool.com/books/working_with_apis/read/more_http_methods#deleting_a_resource)
#### [Summary](https://launchschool.com/books/working_with_apis/read/more_http_methods#summary)
## REAL WORLD APIS
### [TMDB API](https://launchschool.com/books/working_with_apis/read/tmdb_api)
#### Our Goal
#### [Reading Documentation](https://launchschool.com/books/working_with_apis/read/tmdb_api#readingdocumentation)
#### [First API Request](https://launchschool.com/books/working_with_apis/read/tmdb_api#firstrequest)
#### [Gaining Access](https://launchschool.com/books/working_with_apis/read/tmdb_api#gainingaccess)
#### [Fetching movie information](https://launchschool.com/books/working_with_apis/read/tmdb_api#fetchingmovieinfo)
#### [Rating A Movie](https://launchschool.com/books/working_with_apis/read/tmdb_api#ratingmovies)
#### [Deleting A Rating](https://launchschool.com/books/working_with_apis/read/tmdb_api#deletingrating)
#### [TMDB API and REST Principles Analysis](https://launchschool.com/books/working_with_apis/read/tmdb_api#apirestanalysis)
###### Resource Design
###### HTTP Methods
###### Error handling approach
###### Real-World Impact
###### What we can learn
#### Exercises:
### REFERENCE
### [HTTP Response Headers](https://launchschool.com/books/working_with_apis/read/http_response_headers)
#### [Access-Control-Allow-Origin](https://launchschool.com/books/working_with_apis/read/http_response_headers#accesscontrolalloworigin)
#### [Allow](https://launchschool.com/books/working_with_apis/read/http_response_headers#allow)
#### [Content-Length](https://launchschool.com/books/working_with_apis/read/http_response_headers#contentlength)
#### [Content-type](https://launchschool.com/books/working_with_apis/read/http_response_headers#contenttype)
#### [ETag](https://launchschool.com/books/working_with_apis/read/http_response_headers#etag)
#### [Last-Modified](https://launchschool.com/books/working_with_apis/read/http_response_headers#lastmodified)
#### [WWW-Authenticate](https://launchschool.com/books/working_with_apis/read/http_response_headers#wwwauthenticate)
#### [X-* Headers](https://launchschool.com/books/working_with_apis/read/http_response_headers#xheader)

### [5	Network Programming in JavaScript](https://launchschool.com/lessons/1b723bd0/assignments/9d538701)
#### Single Page Applications
### [6	Making a Request with Fetch	](https://launchschool.com/lessons/1b723bd0/assignments/9beef168)
#### Making a Simple Request
#### Fetch's Response Object
#### Reading Response Data
#### Configuring Fetch Requests
### [7	Data Serialization](https://launchschool.com/lessons/1b723bd0/assignments/8da76880)
#### Request Serialization Formats
##### Query String / URL Encoding
#### Multipart Forms
#### JSON Serialization
### [8	Example: Loading HTML via Fetch](https://launchschool.com/lessons/1b723bd0/assignments/b781aaa3)
#### Summary
#### Problems
### [9	Example: Submitting a Form via Fetch](https://launchschool.com/lessons/1b723bd0/assignments/85a3754f)	
#### URL-encoding POST Parameters
#### Submitting a Form
#### Submitting a Form with FormData
#### Problems
### [10	Example: Loading JSON via Fetch](https://launchschool.com/lessons/1b723bd0/assignments/cf01ccbf)
#### Problems
### [11	Example: Sending JSON via Fetch](https://launchschool.com/lessons/1b723bd0/assignments/ae60c1cd)
#### Setting the Content-Type Header
#### Handling the Response
#### Problems
### [12	Cross-Domain Requests with Fetch and CORS](https://launchschool.com/lessons/1b723bd0/assignments/25da79fd)
#### Cross-Origin Requests
#### Cross-Origin requests with Fetch
#### CORS
#### CORS in action
#### Conclusion
### [13	Legacy JavaScript - jQuery and XMLHttpRequest](https://launchschool.com/lessons/1b723bd0/assignments/84d25145)
#### jQuery
#### XMLHttpRequest (XHR)
##### A Basic GET Request
### [14	Project: Search Autocomplete, Part 1 - Introduction and Setup	](https://launchschool.com/lessons/1b723bd0/assignments/b9691285)
#### Introduction
#### Server Setup
### [15	Project: Search Autocomplete, Part 2 - Setting up the UI](https://launchschool.com/lessons/1b723bd0/assignments/e784e7dd)
#### Setting up the UI 
### [16	Project: Search Autocomplete, Part 3 - Talking to the server](https://launchschool.com/lessons/1b723bd0/assignments/0be1e6a9)
#### Binding Events
#### Communicating With the Server
#### Rendering List of Countries
#### Resetting the UI
#### Rendering Overlay Content
#### Improving the Overlay Content
### [17	Project: Search Autocomplete, Part 4 - Improving user experience](https://launchschool.com/lessons/1b723bd0/assignments/d9af6b23)
#### Going Up and Down the List
#### Tab Completion
#### Hide the List and Revert to User Input
#### Selecting a Country by Clicking
### [18	Project: Search Autocomplete, Part 5 - Throttling Fetch requests](https://launchschool.com/lessons/1b723bd0/assignments/72dd3b59)
#### The Problem
#### The Solution
###### Throttling
#### Implementing debounce
##### Making the Code Reusable
### [19	Summary](https://launchschool.com/lessons/1b723bd0/assignments/7729a45a)	
### 20	Quiz	

## [4	Putting it All Together](https://launchschool.com/lessons/32b572ea/assignments)
### [1	Introduction](https://launchschool.com/lessons/32b572ea/assignments/bcd772c9)	
### [2	Project: Photo Gallery, Part 1 - Introduction and Server Setup](https://launchschool.com/lessons/32b572ea/assignments/96518ba5)
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
### [6	Project: Booking App, Part 1 - Introduction and Server Setup](https://launchschool.com/lessons/32b572ea/assignments/40b0794b)
##### App Description
##### App Specifications
##### Getting to Know the Booking App
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
    -  teh DOM
    -  the Event Model
    -  Asynchronous code
    -  AJAX with `fetch`
  -  3 and a half hours to complete
-  YOU CAN SKIP THIS
### 5	Part 2: Tips for the JS239 Interview
### 6	Part 2: Schedule a JS239 Interview
### 7	JS239 Interview Feedback
### 8	Part 3: Tips for the Take Home Project
### 9	 Part 3: Practice Project
### 10	Part 3: The Take Home Project
### 11	Project Feedback
