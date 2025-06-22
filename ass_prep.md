# possible questions -> Demonstrate & explain::

## JS230 - DOM manipulation and browser events
- (quiz Q1) describe the structure and composition of the DOM
- (quiz Q2) What are the two main types of nodes that we have worked with in the course? How are they different?
- (quiz Q3) Using built in DOM methods write JS code that logs the value of the `href` attribute on every `a` element in the web-page.
- (quiz 5) Describe document.querySelector and document.querySelectorAll. What arguments do they take? What values do they return? Do they have any side effects?
- Capturing and bubbling
- Adding nodes to a DOM in a particular way (quiz Q7)
- finding all nodes that meet certain criteria (quiz Q4)
- Walk the tree in a particular order (quiz Q6)
## JS235 Async Javascript & APIs
- `setTimeout()` (quiz Q8)
- Event listeners
- The 3 phases of propagation of a DOM (Quiz Q10)
- `.stopPropagation` v `.preventDefault` (quiz Q11)
- what is `addEventListener`
- Explain the `type`, `target` and `currentTarget` properties of an Event object.
- Demonstrate how you prevent a click event's default behaviour (Q14)

## AJAX with fetch

- What is the advantage of using AJAX over letting the browser automatically visit links and submit forms?(Quiz 15)
- Imagine we need to access some JSON data that's provided by an API endpoint. What are the three basic steps required to do so using fetch? You may assume we do so using a GET request.(Quiz 16)
- Quiz 17:
Write JavaScript code that uses fetch to send the following HTTP request:

PUT /todos/15 HTTP/1.1
Content-Length: 37
Host: my-todo-app.com
Content-Type: application/json

{"title":"Buy Milk","completed":true}
Use the following object in your code:

{title: 'Buy Milk', completed: true}
You should assume that my-todo-app.com is not the host where this code is running.

- Quiz 18: CORS:What is CORS? Explain why it is needed? Also, name the HTTP headers that are required for CORS to work.
- Quiz 19: A promise can have multiiple states, describr them (Quiz 19)
- Quiz 20: rewrit the following code to use `async` and   `await` keywords.







