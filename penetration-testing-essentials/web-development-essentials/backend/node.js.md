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

Multiple middleware can exists for security purpose. Let's understand with the help of an example.&#x20;

#### **Directory Structure (for reference)**

```
project/
│
├── users.json
└── app.js
```

`users.json` (example content):

```json
[
  { "id": 1, "name": "John Doe", "email": "john@example.com" },
  { "id": 2, "name": "Alice Smith", "email": "alice@example.com" },
  { "id": 3, "name": "Bob Johnson", "email": "bob@example.com" }
]
```

***

#### **app.js**

```javascript
const express = require('express');
const fs = require('fs');
const path = require('path');

const app = express();
const PORT = 3000;

// =====================
// MIDDLEWARE 1: Logger
// =====================
app.use((req, res, next) => {
    console.log(`[${new Date().toISOString()}] ${req.method} request for ${req.url}`);
    next(); // pass control to the next middleware
});

// =====================
// MIDDLEWARE 2: Request Timer
// =====================
app.use((req, res, next) => {
    req.requestTime = Date.now(); // attach timestamp to request
    next();
});

// =====================
// MIDDLEWARE 3: Validate ID for /users/:id
// =====================
function validateUserId(req, res, next) {
    const id = req.params.id;
    if (id && isNaN(parseInt(id))) {
        return res.status(400).json({ error: "Invalid user ID. Must be a number." });
    }
    next();
}

// =====================
// ROUTES
// =====================

// GET /users → list all users
app.get('/users', (req, res) => {
    const usersPath = path.join(__dirname, 'users.json');
    fs.readFile(usersPath, 'utf8', (err, data) => {
        if (err) {
            return res.status(500).json({ error: 'Failed to read users data' });
        }
        const users = JSON.parse(data);
        res.json({
            requestTime: req.requestTime,
            count: users.length,
            users
        });
    });
});

// GET /users/:id → get user by id
app.get('/users/:id', validateUserId, (req, res) => {
    const usersPath = path.join(__dirname, 'users.json');
    fs.readFile(usersPath, 'utf8', (err, data) => {
        if (err) {
            return res.status(500).json({ error: 'Failed to read users data' });
        }
        const users = JSON.parse(data);
        const user = users.find(u => u.id === parseInt(req.params.id));
        if (!user) {
            return res.status(404).json({ error: 'User not found' });
        }
        res.json({
            requestTime: req.requestTime,
            user
        });
    });
});

// Handle undefined routes
app.use((req, res) => {
    res.status(404).json({ error: 'Route not found' });
});

// Start server
app.listen(PORT, () => {
    console.log(`Server running on http://localhost:${PORT}`);
});
```

***

## Development introduction

* **Platform**: Node.js combines JavaScript and C++ for efficient server-side execution, leveraging OS capabilities.
* **Backend Focus**: Designed for backend tasks, excluding DOM-related operations like modification or creation.
* **Purpose**: Optimized for web application backend development with JavaScript.

Let's see how the NodeJS is made to handle multiple web application requests and how it responds accordingly.&#x20;

{% code overflow="wrap" %}
```javascript
const http = require('http'); //we imported http module for creating the server

const server = http.createServer((req, res) => { 
//the req and res will have the corresponding request and response of the website
  const url = req.url;

  // Navigation bar HTML
  const navBar = `
    <nav>
      <ul>
        <li><a href="/home">Home</a></li>
        <li><a href="/contact-us">Contact Us</a></li>
      </ul>
    </nav>
  `;

  // Handle different routes
  if (url === '/home') {
    res.writeHead(200, { 'Content-Type': 'text/html' });
    res.write(`
      <html>
        <body>
          ${navBar}
          <h1>You're in the homepage</h1>
        </body>
      </html>
    `);
  } else if (url === '/contact-us') {
    res.writeHead(200, { 'Content-Type': 'text/html' });
    res.write(`
      <html>
        <body>
          ${navBar}
          <h1>You're in the contact-us page</h1>
        </body>
      </html>
    `);
  } else {
    res.writeHead(404, { 'Content-Type': 'text/html' });
    res.write(`
      <html>
        <body>
          ${navBar}
          <h1>404 - Page Not Found</h1>
        </body>
      </html>
    `);
  }

  res.end();
});

const PORT = 3000;
server.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`);
});
```
{% endcode %}

In the above code we've written separate logic for everything. This can be simplified using Express JS which has special functions for each specific type of request made and serves output accordingly. You can see in the below code that how express JS simplifies the code&#x20;

{% code overflow="wrap" %}
```javascript
const express = require('express');
const app = express();

// Middleware to parse POST request body
app.use(express.urlencoded({ extended: true }));

// Navigation bar
const navBar = `
  <nav>
    <ul>
      <li><a href="/home">Home</a></li>
      <li><a href="/contact-us">Contact Us</a></li>
    </ul>
  </nav>
`;

// GET request for /home
app.get('/home', (req, res) => {
  res.send(`
    <html>
      <body>
        ${navBar}
        <h1>You're in the homepage</h1>
      </body>
    </html>
  `);
});

// GET request for /contact-us
app.get('/contact-us', (req, res) => {
  res.send(`
    <html>
      <body>
        ${navBar}
        <h1>You're in the contact-us page</h1>
      </body>
    </html>
  `);
});

// 404 handler for unknown routes
app.use((req, res) => {
  res.status(404).send(`
    <html>
      <body>
        ${navBar}
        <h1>404 - Page Not Found</h1>
      </body>
    </html>
  `);
});
//If you didn't write these last function and a GET request is received at unknown path then response would be like. 
// Cannot GET /unknown-path

const PORT = 3000;
app.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`);
});
```
{% endcode %}

### Authentication&#x20;

There are two types of authentication in the NodeJS which is stateful and stateless. As the name suggests the stateful authentication mechanisms maintains certain information of the user with the help of a uid (user identifier).&#x20;

These `uid` can be transferred in three ways.&#x20;

* cookies (If our request is directly sent to the route without any middleware or APIs)
* response&#x20;
* headers (If there is any API (e.g., RESTful) then this will be used).

<figure><img src="../../../.gitbook/assets/image (150).png" alt=""><figcaption></figcaption></figure>

#### Key Differences

* **Stateful (Sessions)**:
  * Server stores session data (e.g., isAuthenticated).
  * Good for apps with trusted clients (e.g., web browsers).
  * Scales poorly for distributed systems (session storage overhead).
* **Stateless (JWT)**:
  * No server-side storage; token contains all info.
  * Ideal for APIs, mobile apps, IoT devices.
  * Client must send token with every request.

#### Stateful&#x20;

{% code overflow="wrap" %}
```javascript
const express = require('express');
const session = require('express-session');
const fs = require('fs');
const app = express();

// Middleware to parse JSON bodies
app.use(express.json());

// Session middleware
app.use(session({
  secret: 'my-secret-key',
  resave: false,
  saveUninitialized: false,
  cookie: { secure: false } // Set to true in production with HTTPS
}));

// Load users from JSON file
const users = JSON.parse(fs.readFileSync('user_details.json'));

// Middleware to check if user is authenticated
const isAuthenticated = (req, res, next) => {
  if (req.session.isAuthenticated) {
    return next();
  }
  res.status(403).json({ message: 'Forbidden: Please log in' });
};

// Login route
app.post('/login', (req, res) => {
  const { username, password } = req.body;
  const user = users.find(u => u.username === username && u.password === password);
  if (user) {
    req.session.isAuthenticated = true;
    req.session.username = username;
    return res.json({ message: 'Login successful' });
  }
  res.status(401).json({ message: 'Invalid credentials' });
});

// Protected home route
app.get('/home', isAuthenticated, (req, res) => {
  res.json({ message: `Logged in as ${req.session.username}` });
});

// Logout route
app.post('/logout', (req, res) => {
  req.session.destroy();
  res.json({ message: 'Logged out' });
});

// Start server
app.listen(3000, () => console.log('Stateful server running on port 3000'));
```
{% endcode %}

#### Stateless

{% code overflow="wrap" %}
```javascript
const express = require('express');
const jwt = require('jsonwebtoken');
const fs = require('fs');
const app = express();

// Middleware to parse JSON bodies
app.use(express.json());

// Load users from JSON file
const users = JSON.parse(fs.readFileSync('user_details.json'));

// Secret key for JWT (use environment variable in production)
const JWT_SECRET = 'my-jwt-secret';

// Middleware to verify JWT
const verifyToken = (req, res, next) => {
  const token = req.headers['authorization']?.split(' ')[1]; // Expect 'Bearer <token>'
  if (!token) {
    return res.status(403).json({ message: 'Forbidden: No token provided' });
  }
  try {
    const decoded = jwt.verify(token, JWT_SECRET);
    req.user = decoded;
    next();
  } catch (err) {
    res.status(403).json({ message: 'Forbidden: Invalid token' });
  }
};

// Login route
app.post('/login', (req, res) => {
  const { username, password } = req.body;
  const user = users.find(u => u.username === username && u.password === password);
  if (user) {
    const token = jwt.sign({ username }, JWT_SECRET, { expiresIn: '1h' });
    return res.json({ token, message: 'Login successful' });
  }
  res.status(401).json({ message: 'Invalid credentials' });
});

// Protected home route
app.get('/home', verifyToken, (req, res) => {
  res.json({ message: `Logged in as ${req.user.username}` });
});

// Start server
app.listen(3001, () => console.log('Stateless server running on port 3001'));
```
{% endcode %}

## How to execute lab cases

```sh
cd /var/www/html/node_xss_lab/case1
npm init -y
npm install express
node index.js
```
