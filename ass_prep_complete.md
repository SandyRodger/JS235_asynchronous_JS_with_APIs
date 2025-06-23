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
async function postRequest(url) {
  try {
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

    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }
    return await response.json();
  } catch (error) {
    console.error('Request failed:', error);
    throw error;
  }
}
```

#### 5. ​Advanced​: Response Body Consumption* Explain why the following code will cause an error and how to fix it:

```
async function processResponse() {
  const response = await fetch('https://api.example.com/data');
  const text = await response.text();
  const json = await response.json(); // This will fail
  return { text, json };
}
```

- This code will cause an error because it is consuming the `Response` twice, when it is only possible to do so once. To fix this while preserving the intention of the code one can either clone the response like this:

```
async function processResponse() {
  const response = await fetch('https://api.example.com/data');
  const responseClone = response.clone();
  const text = await response.text();
  const json = await responseClone.json();
  return { text, json };
}
```

or create the Json object from the text, like this:


```
async function processResponse() {
  const response = await fetch('https://api.example.com/data');
  const text = await response.text();
  const json = JSON.parse(text);
  return { text, json };
}
```
#### 6. ​Advanced​: Error Handling and Response Validation* Complete this function to properly handle different response scenarios:

```
async function robustFetch(url) {
  try {
    const response = await fetch(url);
    if (!response.ok) {
      throw new Error(`HTTP ${response.status}:${response.statusText}`);
    }
    const result = await response.json();
    if (!result) {
      throw new Error(result);
    }
    return result;
  } catch (error) {
    console.error('Problem retrieving resource', error)
  }
}
```

- There are 3 problems: 
  1: in line 4 we test the validity of the response by using a `!` to see if it is not truthy, but valid JSON can be `null`, `false` `0` or `""`.
  2: `throw new Error(result);` this line passes the response object as an error message which isn't helpful
  3: It would be clearer if there was a space in the middle of this string: `HTTP ${response.status}:${response.statusText}`

Also I would return the result of parsing the json response and defer error handling to the event handler that called `robustFetch`.

I would correct the code to look like this:

```
async function robustFetch(url) {
  try {
    const response = await fetch(url);
    if (!response.ok) {
      throw new Error(`HTTP ${response.status}: ${response.statusText}`);
    }
    const result = await response.json();
    return result;
  } catch (error) {
    console.error('Problem retrieving resource: ', error);
    throw error;
  }
}
```

### 7 Debugging Exercise​: Fix the Fetch Implementation*

// This code is supposed to send form data and display the result, but it has several issues. Identify and fix them:

function submitForm(formData) { // this needs to be preceded by the `await` keyword
  fetch('https://api.example.com/submit', { //this needs to be preceded by `await` and also saved to a variable
    method: 'POST',
    // this is missing a headers key with an appropriate value
    body: formData // this is formData and needs to be parsed into a string with `JSON.stringify(formData)
  })
  .then(response => {
    return response.json;
  }) // there is no catch statement for error handling
  .then(data => {
    console.log('Success:', data);
  });
}

// I would correct the code to use try/catch syntax for more robust error handling

async function submitForm(formData) {
  try {
    const response = await fetch('https://api.example.com/submit', {
      method: 'POST',
      headers: {
        'Content-type': 'application/json',
      },
      body: JSON.stringify(formData),
    }) 

    if (!response.ok) {
      throw new Error(`Problem retrieving data: ${response.statusText}`);
    }

    const result = await response.json();
    return result;
  } catch (error) {
    console.error(error);
    throw error;
  }
}

### 8 Explain FormData

Based on the Launch School curriculum, FormData is a built-in browser API that helps serialize form data for HTTP requests. Let me clarify when to use it and address your question about JSON conversion.
When FormData is Used
FormData is primarily used when:
1.  ​Submitting HTML forms via JavaScript​ - Instead of manually extracting and encoding form field values
2.  ​Uploading files​ - FormData handles file uploads naturally
3.  ​Working with multipart/form-data​ - When you need this specific encoding format
FormData Does NOT Need JSON Conversion
This is a key misconception to clear up. FormData should ​not​ be converted to JSON for network requests. Here's why:
Direct Usage with Fetch
let form = document.getElementById('form');
let data = new FormData(form);

// Use FormData directly - no conversion needed
fetch('/api/books', {
  method: 'POST',
  body: data  // FormData goes directly in body
  // No Content-Type header needed - browser sets it automatically
});
When you use FormData directly:
•   Browser automatically sets Content-Type: multipart/form-data; boundary=...
•   Data is sent in multipart format, not JSON
•   Perfect for forms with file uploads
When You DO Convert FormData
You only convert FormData to a plain object when you specifically want to send ​JSON​ instead of multipart data:
// Convert FormData to plain object for JSON sending
function formDataToObject(formData) {
  return Object.fromEntries(formData.entries());
}

let form = document.getElementById('form');
let formData = new FormData(form);
let jsonData = formDataToObject(formData);

fetch('/api/books', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json'
  },
  body: JSON.stringify(jsonData)  // Now sending JSON
});
Three Common Approaches
According to the JS235 curriculum, you have three main serialization options:
1.  ​URL-encoded​ (application/x-www-form-urlencoded)
2.  ​Multipart​ (multipart/form-data) - what FormData uses
3.  ​JSON​ (application/json)
Choose FormData when you want multipart encoding, especially for file uploads. Convert to JSON only when the API specifically requires JSON format.

#### 9 FormData Conversion
// Write a function that takes a FormData object and converts it to a regular JavaScript object that can be JSON stringified. Then demonstrate how to use this in a fetch request.

// Convert FormData to plain object for JSON sending
function formDataToObject(formData) {
  return Object.fromEntries(formData.entries());
}

let form = document.getElementById('form');
let formData = new FormData(form);
let jsonData = formDataToObject(formData);

fetch('/api/books', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json'
  },
  body: JSON.stringify(jsonData)  // Now sending JSON
});
