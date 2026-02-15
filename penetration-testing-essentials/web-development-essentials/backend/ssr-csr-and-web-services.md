# SSR, CSR and Web Services

## Server-Side Rendering (SSR)

**Server-Side Rendering (SSR)**\
It is a technique where a web application's UI is generated as HTML on the server for each client request, rather than relying on client-side JavaScript to build it.

**How it works**

* **HTTP Requests:** _A user's browser sends an HTTP request to the server for a specific web page (e.g., a list of blogs)._
* **Server-Side Generation:** _The server processes the request, retrieves necessary data, and renders the application's components into an HTML string. The server retrieves the list of blogs and embeds it with style into an HTML file._
* **HTML Response:** _The fully formed HTML document, along with any required JavaScript, is sent back to the user's browser._

**Use Case**

* _This works well when the client is a browser because browsers can directly interpret HTML._
* _But if the request is made from **non-browser clients** like IoT devices, mobile apps, or other systems, HTML is not efficient. They need structured data formats instead of styled pages._
* _For this purpose, we use **APIs such as SOAP and REST**, which allow the server to respond with machine-readable data (like XML or JSON) that any device can understand and process._

{% hint style="info" %}
_SSR made the web talk in HTML, but when machines beyond browsers joined the conversation, developers were forced to invent new standards for communication — **SOAP** and **REST**._
{% endhint %}

***

## Web Services&#x20;

### SOAP (Simple Object Access Protocol)

SOAP is a protocol that defines strict rules for communication between systems. It uses XML as the message format.&#x20;

**Example SOAP Request (get user details):**

```xml
<soap:Envelope xmlns:soap="http://www.w3.org/2003/05/soap-envelope">
  <soap:Body>
    <getUserRequest>
      <userId>123</userId>
    </getUserRequest>
  </soap:Body>
</soap:Envelope>
```

**Example SOAP Response:**

```xml
<soap:Envelope xmlns:soap="http://www.w3.org/2003/05/soap-envelope">
  <soap:Body>
    <getUserResponse>
      <name>John Doe</name>
      <email>john@example.com</email>
    </getUserResponse>
  </soap:Body>
</soap:Envelope>
```

{% hint style="info" %}
_Because SOAP was too strict, heavy with XML, and hard for lightweight devices to consume, REST was introduced as a simpler, flexible, and JSON-friendly alternative that made web communication faster and easier for everyone._
{% endhint %}

### REST  (Representational State Transfer)

REST is an **architectural style**, not a protocol. It is lighter, simpler, and designed for the modern web. REST commonly uses **HTTP** and data formats like **JSON** (or sometimes XML).

#### **Key Principles**

* Uses **resources (nouns)** like `/users`, `/blogs`.
* HTTP methods map to actions:
  * **GET** → Read
  * **POST** → Create
  * **PUT/PATCH** → Update
  * **DELETE** → Delete
* **Stateless** (each request is independent).

#### **🛠 RESTful API**

A RESTful API is a web service that follows REST principles. Clients interact with resources using URLs and HTTP verbs, and the server responds with JSON/XML.

**Example REST API Request (get user details):**

```
GET /api/users/123 HTTP/1.1
Host: example.com
Accept: application/json
```

**Example REST API Response (JSON):**

```json
{
  "id": 123,
  "name": "John Doe",
  "email": "john@example.com"
}
```

<details>

<summary>Best Practices for REST APIs</summary>

#### **1. Use Proper HTTP Methods (All methods should be respected)**

* **GET** → Retrieve data (should be safe and idempotent).
* **POST** → Create new resources.
* **PUT** → Update/replace a resource.
* **PATCH** → Partially update a resource.
* **DELETE** → Remove a resource.

***

#### **2. Use Meaningful Resource URIs**

* Keep URLs **noun-based**, not action-based.
  * ✅ `/users/123/posts`
  * ❌ `/getUserPosts?id=123`
* Use **plural nouns** for collections (`/users`), singular for single items (`/users/123`).

***

#### **3. Use HTTP Status Codes Properly**

* **200 OK** → Success for GET/PUT/POST (if resource returned).
* **201 Created** → Resource successfully created (POST).
* **204 No Content** → Successful request but no body (DELETE).
* **400 Bad Request** → Client-side error, invalid input.
* **401 Unauthorized** → Missing/invalid authentication.
* **403 Forbidden** → Authenticated but no permission.
* **404 Not Found** → Resource does not exist.
* **500 Internal Server Error** → Server-side error.

***

#### **4. Support Filtering, Sorting, and Pagination**

* Filtering: `/users?role=admin`
* Sorting: `/users?sort=createdAt_desc`
* Pagination: `/users?page=2&limit=10`
* Keeps API responses **efficient and scalable**.

***

#### **5. Use Versioning**

* Always version your API to avoid breaking clients:
  * `/api/v1/users`
  * `/api/v2/users`

***

#### **6. Use Consistent Naming Conventions**

* Stick to **lowercase, hyphenated, or snake\_case** consistently.
* Example: `/user-profiles` or `/user_profiles`

***

#### **7. Use JSON as Standard Format**

* Lightweight, widely supported, easy to parse.
* Include **meaningful error messages** in JSON for client debugging.

***

#### **8. Implement Proper Authentication & Authorization**

* Use **OAuth 2.0**, JWT, or API keys depending on the use case.
* Keep sensitive data secure and avoid exposing unnecessary info.

***

#### **9. HATEOAS (Optional Advanced)**

* Hypermedia as the engine of application state: include links to related resources in responses.
* Example: A user object might include a link to `/users/123/posts`.

***

#### **10. Document Your API**

* Use tools like **Swagger/OpenAPI** or **Postman** to provide clear API documentation.
* Helps clients understand how to use your API correctly.

***

#### **11. Handle Errors Gracefully**

* Return structured error objects with code, message, and details:

```json
{
  "error": {
    "code": 400,
    "message": "Invalid user ID",
    "details": "User ID must be a positive integer"
  }
}
```

***

#### **12. Maintain Idempotency Where Needed**

* GET, PUT, DELETE should be **idempotent** — repeated calls should not cause unintended effects.

</details>

{% hint style="info" %}
_REST gave devices structured endpoints to talk to servers, but GraphQL took it further by letting every device ask for exactly what it needs — no more, no less — all in a single powerful query._
{% endhint %}

### GraphQL (Graph Query Language)

GraphQL is a **query language and runtime** for APIs, created by Facebook in 2012 and open-sourced in 2015. Unlike REST (which exposes fixed endpoints), GraphQL lets the **client decide exactly what data it wants** and get it in a single request.

#### 🔑 Key Points

* **Client-driven:** The client specifies fields, not the server.
* **Single endpoint:** Instead of multiple URLs like `/users` or `/users/123/posts`, GraphQL uses a single endpoint (usually `/graphql`).
* **No over-fetching/under-fetching:** You only get the data you ask for — nothing extra, nothing missing.
* **Flexible & device-friendly:** Great for mobile apps, IoT devices, and complex UIs that need different amounts of data.
* **Strongly typed:** Uses a schema to define the structure of data and supported queries.

#### 🛠 GraphQL API

A GraphQL API works by:

1. **Defining a Schema:** The server declares types (like `User`, `Post`) and what queries/mutations are allowed.
2. **Sending Queries/Mutations:** The client sends a query (for reading data) or a mutation (for writing/updating data).
3. **Getting JSON Response:** The server executes the query and returns only the requested fields in JSON format.

#### 📖 Example

**GraphQL Query (get user with only name and email):**

```graphql
{
  user(id: 123) {
    name
    email
  }
}
```

**GraphQL Response:**

```json
{
  "data": {
    "user": {
      "name": "John Doe",
      "email": "john@example.com"
    }
  }
}
```

**GraphQL Mutation (create a new user):**

```graphql
mutation {
  createUser(name: "Alice", email: "alice@example.com") {
    id
    name
    email
  }
}
```

**Response:**

```json
{
  "data": {
    "createUser": {
      "id": "456",
      "name": "Alice",
      "email": "alice@example.com"
    }
  }
}
```

👉 **Summary:**

* **SOAP** = Heavy, XML, strict protocol (enterprise-grade).
* **REST** = Lightweight, resource-based, JSON/XML (modern standard).
* **GraphQL** = Flexible, query-based, single endpoint, JSON (solves REST’s inefficiencies).

> _Whether it’s SOAP with its rigid XML, REST with its clean resources, or GraphQL with its flexible queries — all were born for the same mission: to let machines talk to servers and share data across the web._

***

## CSR (Client-Side Rendering)

**Client-Side Rendering (CSR)** is a technique where a web application's user interface (UI) is generated in the client's browser using JavaScript, rather than on the server. Instead of the server sending a fully formed HTML page, it sends a minimal required data with JavaScript that builds the UI on the client side.

#### How It Works

* **Initial HTTP Request:** The user's browser requests a web page from the server. Instead of sending a full HTML page, the server usually sends a **minimal HTML shell** along with JavaScript files.
* **JavaScript Execution:** The browser downloads and executes the JavaScript. The scripts fetch data (via APIs like REST or GraphQL) and dynamically build the HTML and UI in the browser.
* **Rendering:** The fully interactive page is displayed to the user, updated as needed without requiring full page reloads.

#### Practical Example

* User visits /blogs on a website.
* Server sends a basic HTML file with \<script src="app.js"> and a \<div id="root">.
* The browser runs app.js, which calls an API (e.g., /api/blogs) to get JSON data: \[{"title": "Post1", "content": "Blah"}].
* JavaScript (e.g., React) builds the HTML for the blog list and inserts it into the \<div id="root">, showing the styled page.

{% hint style="info" %}
_SSR made the server speak HTML, but CSR handed the mic to the browser, letting it build pages on the fly and handle dynamic interactions like a true client-side powerhouse._
{% endhint %}
