# Node.js

Node.js is a powerful, open-source, and cross-platform JavaScript runtime environment built on Chrome's V8 engine. It can handle many connections at once without waiting for one to finish before starting another. This makes it great for real-time apps and high-traffic websites. It enables server-side development, supports asynchronous, event-driven programming, and efficiently handles scalable network applications.

## Single-Threaded Event Loop Model&#x20;

NodeJS operates on a single thread but efficiently handles multiple concurrent requests using an event loop.&#x20;

* **Client Sends a Request:** The request can be for data retrieval, file access, or database queries.
* **NodeJS Places the Request in the Event Loop:** If the request is non-blocking (e.g., database fetch), it is sent to a worker thread without blocking execution.
* **Asynchronous Operations Continue in Background:** While waiting for a response, NodeJS processes other tasks.
* **Callback Execution:** Once the operation completes, the callback function executes, and the response is sent back to the client.

## Components of NodeJS Architecture

* **V8 Engine:** Compiles JavaScript to machine code for fast execution.
* **Event Loop:** Manages asynchronous tasks without blocking the main thread.
* **Libuv:** Handles I/O operations, thread pool, and timers.
* **Non-Blocking I/O:** Executes tasks without waiting for previous ones to complete.

{% hint style="info" %}
_It is ideal for building RESTful APIs and GraphQL APIs._&#x20;
{% endhint %}

## Start a simple web server

```javascript
//server.js
let http = require('http');
http.createServer(function (req, res) {
  res.writeHead(200, {'Content-Type': 'text/html'});
  res.end('Hello World!');
}).listen(8080);
```

Execute this file as `node server.js` and navigate to `http://localhost:8080`.&#x20;

### REST API Endpoints made using Express

The `require` keyword is used to import modules, and those modules can be installed using the `npm` package manager.

{% code overflow="wrap" %}
```javascript
//server.js
const express = require('express');
const app = express();
const port = 3000;

// Middleware to parse JSON requests
app.use(express.json());

// In-memory "database" (array)
let users = [
  { id: 1, name: 'Alice' },
  { id: 2, name: 'Bob' }
];

// GET all users
app.get('/api/users', (req, res) => {
  res.json(users);
});

// GET user by ID
app.get('/api/users/:id', (req, res) => {
  const user = users.find(u => u.id === parseInt(req.params.id));
  if (!user) return res.status(404).json({ message: 'User not found' });
  res.json(user);
});

// POST a new user
app.post('/api/users', (req, res) => {
  const newUser = {
    id: users.length + 1,
    name: req.body.name
  };
  users.push(newUser);
  res.status(201).json(newUser);
});

app.listen(port, () => {
  console.log(`Server running at http://localhost:${port}`);
});
```
{% endcode %}

## What is Middleware

* **Middleware is just a function** in Express that sits between the request (coming from the client) and the response (going back to the client).
* It has access to:
  * `req` → the request object (data coming in)
  * `res` → the response object (data going out)
  * `next()` → a function to pass control to the next middleware

### 🛣️ Think of it like a Highway Checkpost

Imagine a car (the request) traveling to the city (your final route handler).

* On the way, there are checkposts (middlewares).
* Each checkpost can:
  * **Inspect** the car (read data from `req`)
  * **Modify** the car (change or add data to `req` or `res`)
  * **Stop** the car (end the request, e.g., send an error response)
  * Or **let it pass** by calling `next()`

Only after passing through all checkposts does the car reach the destination (your route handler).

### How It Fits Together in a Web App

* **Node.js**: Runs the server-side JavaScript, managing asynchronous operations (e.g., handling multiple HTTP requests or database queries via the event loop).
* **Express.js**: Provides the routing (/api/users) and middleware (express.json(), authenticate) to process requests and responses. Modifications can be done to the request body before it hits the route (Actual backend logic).&#x20;
* **API**: Defines endpoints that handle client requests (e.g., GET to fetch users, POST to add a user), interacting with a database (MongoDB) or memory.
* **Data Flow**: Client sends a request (e.g., via browser or curl) → Node.js/Express processes it → Middleware validates/parses → Route logic interacts with database/memory → Response sent back.

***

## Development introduction

* **Platform**: Node.js combines JavaScript and C++ for efficient server-side execution, leveraging OS capabilities.
* **Backend Focus**: Designed for backend tasks, excluding DOM-related operations like modification or creation.
* **Purpose**: Optimized for web application backend development with JavaScript.

