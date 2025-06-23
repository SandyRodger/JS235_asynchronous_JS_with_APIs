#### 1. Why do we use `await` twice here:

```javascript
async function fetchData() {
let response = await fetch('https://api.github.com/repos/rails/rails');
let data = await response.json(); // Parse JSON response
console.log(data.open_issues);
}

fetchData();
```
  A: Because you're dealing with 2 seperate asynchronous operations, each returning its own promise. 
  1) First is the network request (`let response = await fetch('https://api.github.com/repos/rails/rails')` which returns a promise, that resolves with a `Response` object once the HTTP request is completed. The `Response` object contains meta data about the request (status codes, headers etc) but not the actual body content.
  2) Second await is for reading the response body. The `.json()` method is also asynchronous. It returns another promise that resolves with a parsed javascript object after reading and parsing the response as JSON
  3) The reason we have 2 different requests is it gives you flexibility in how you handle the response body. The `Response` body has different methods for different data types (`.text()`, `.json()`, `.blob()`). Each operation takes time so we use async methods.
  4) One might think that once we have the `Response` object we shouldn't need to wait any more, but actually even then the browser doesn't have the complete response body in memory. For efficiency and security browsers use streaming and receive and process data in chunks as it arrives. After the network request completes it exists as a `ReadableStream` that must be consumed.
     When you call `response.json()` the browser must:
       - read the entire response body from the stream.
       - Parse the JSON string (lexical analysis, syntax validation, build Javascript objects.
       - Handle potential errors.
- it can take a very long time. Even `response.text()` involves work.

#### 2. What are the key properties of a Response object returned by fetch? Explain what Response.ok, Response.status, and Response.bodyUsed indicate.

A response object returned by `fetch` contains meta-data about the request. This information is contained in a javascript object with many properties. Some of the more useful ones are: `.status`, `.headers`, `.ok`, `.bodyUsed`, `.redirected`, '.type', `.url`, `.body`, `statusText`. `Response.ok` contains a boolean reflecting if the request was successful, ie if the response status has a code in the two hundreds. `Response.status` contains the HTTP status code of the response as a 3 digit number, ie 403 for "forbidden" (which is to be found in `statusText`. `Response.bodyUsed` also contains a boolean reflecting whether the body has been used, so if the `.json()` method had been called on it this property would point to `true` (the response body can only be consumed once).

#### 3. ​Intermediate​: Async Response Parsing* : What will the following code output and why? Identify any issues:
async function fetchData() {
  const response = await fetch('https://api.example.com/users');
  console.log(response.body);
  const data = response.json();
  console.log(data);
}

- Calling the `fetchData` function will return `undefined`. In the first line of the function we call `fetch` with a url and save the response as the constant `response`. We use the `await` keyword to ensure that the browser pauses to wait for the Promise to resolve to a  response before continuing. So `response` will be a a Response object. Because `await` causes the code within the function to pause, by pushing it to the microtask queue, the rest of the programme will continue. Once the callstack is empty Javascript will begin processing the microtask queue and if the response object has resolved (ie when the HTTP request has completed) it will continue to run the function. At this point `console.log(response.body);` will run and the value printed will be a `ReadableStream`. The next line contains an error as `response.json()` should have the `await` keyword preceding it. This is because it takes significant time to parse a Response object into a JSON object. As it is data will be saved as a pending Promise, which will be printed by the last line in the function as something line `Promise{<pending>}`. So this function is not a correct example of how to fetch and parse a request. The function will return undefined.

#### 4. ​Intermediate​: POST Request with JSON* Write a function that sends a POST request to create a new user with the following data:
{ name: "Alice Johnson", email: "alice@example.com", age: 28 }
Include proper headers and handle the response appropriately.

```javascript
function postRequest(url) {
  const data = { 
    name: "Alice Johnson",
    email: "alice@example.com", 
    age: 28 
  };
  const response = await fetch(url, {
    method: 'POST',
    headers: {
      'Content-type': 'application/json',
    },
    body: JSON.stringify(data)
  });
  return await response.json();
}
```
