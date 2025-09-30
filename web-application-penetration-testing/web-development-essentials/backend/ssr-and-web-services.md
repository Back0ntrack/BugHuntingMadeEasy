# SSR and Web Services

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
