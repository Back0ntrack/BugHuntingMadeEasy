---
cover: ../../.gitbook/assets/all_in_one_cover.PNG
coverY: 0
---

# Web Development Essentials

## Web application distribution (Readymade)

In web applications, **distribution** means the way a web application is made available or delivered to users — whether it’s freely available for download and modification, or provided as a licensed/proprietary service. E.g., `Wordpress` is an open-source software that anyone can download and modify it to create their own website on WordPress.&#x20;

Web applications are mainly of **two types**:

### 1. **Open-Source Web Applications**

* Code is freely available → developers can customize it.
* Widely used by organizations because they are cost-effective and flexible.
* Examples:
  * **WordPress** – Blogging, websites, e-commerce.
  * **OpenCart** – E-commerce sites.
  * **Joomla** – CMS (Content Management System).
  * **Drupal** – Websites, portals.
  * **Magento (Adobe Commerce, open-source edition)** – Online stores.
  * **phpBB** – Forums.
  * **MediaWiki** – Wikis (like Wikipedia).

***

### 2. **Closed-Source (Proprietary) Web Applications**

* Code is **not available** → cannot customize freely.
* Sold as a product or subscription (SaaS model).
* Easier to use but less flexible.
* Examples:
  * **Wix** – Website builder.
  * **Shopify** – E-commerce sites.
  * **Squarespace** – Website builder.
  * **Weebly** – Simple websites.
  * **DotNetNuke (DNN)** – CMS platform.
  * **Salesforce Commerce Cloud** – Enterprise e-commerce.
  * **HubSpot CMS** – Marketing-focused websites

***

## Web Application Layout

Different layers of web application&#x20;

<table><thead><tr><th width="217.99993896484375">Category</th><th>Description</th></tr></thead><tbody><tr><td><strong>Web Application Infrastructure</strong></td><td>Describes the structure of required components, such as the database, needed for the web application to function as intended. </td></tr><tr><td><strong>Web Application Components</strong></td><td>The components that make up a web application represent all the components that the web application interacts with. These are divided into the following three areas: <code>UI/UX</code>, <code>Client</code>, and <code>Server</code> components. </td></tr><tr><td><strong>Web Application Architecture</strong></td><td>Architecture comprises all the relationships between the various web application components.</td></tr></tbody></table>

### Web Application Infrastructure

Various models of web applications are available

* `Client-Server`: Most adopted model of web application. In this model, web applications have two types of components, those in the front end, which are usually interpreted and executed on the client-side (browser), and components in the back end, usually compiled, interpreted, and executed by the hosting server.
* `Many servers - One database`: This model separates the database onto its own database server and allows the web applications' hosting server to access the database server to store and retrieve data.
* `Many servers - Many Databases`: Each web application's data is hosted in a separate database. The web application can only access private data and only common data that is shared across web application. This is most commonly used model.&#x20;

### Web Application Components

Each web application can have a different number of components.

1. `Client`
2. `Server`
   * Webserver
   * Web Application Logic
   * Database
3. `Services` (Microservices)
   * 3rd Party Integrations
   * Web Application Integrations
4. `Functions` (Serverless)

### Web Application Architecture

* `Presentation Layer`: These can be accessed by the client via the web browser and are returned in the form of HTML, JavaScript, and CSS.&#x20;
* `Application Layer`: This layer ensures that all client requests (web requests) are correctly processed. Various criteria are checked, such as authorization, privileges, and data passed on to the client.&#x20;
* `Data Layer`: The data layer works closely with the application layer to determine exactly where the required data is stored and can be accessed.&#x20;

An examaple of a web application architecture could look something like this:&#x20;

<figure><img src="../../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

### Microservices

We can think of microservices as independent components of the web application, which in most cases are programmed for one task only. For example, for an online store, we can decompose core tasks into the following components:

* Registration
* Search
* Payments
* Ratings
* Reviews

## Front End V/s Back End

### Frontend

The front end of a web application contains the user's components directly through their web browser (client-side). It typically includes `HTML`, `CSS` and `JavaScript`.&#x20;

<figure><img src="../../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

This includes everything that the user sees and interact with, like the page's main elements such as the title and text HTML, the design and animation of all elements CSS, and what functions each part of a page performs JavaScript.&#x20;

### Backend

The back end of a web application drives all of the core web application functionalities, all of which is executed at the back end server, which processes everything required for the web application to run correctly.

<table><thead><tr><th width="171.60003662109375">Component</th><th valign="top">Description</th></tr></thead><tbody><tr><td><code>Back end Servers</code></td><td valign="top">The hardware and operating system that hosts all other components and are usually run on operating systems like <code>Linux</code>, <code>Windows</code>, or using <code>Containers</code>.</td></tr><tr><td><code>Web Servers</code></td><td valign="top">Web servers handle HTTP requests and connections. Some examples are <code>Apache</code>, <code>NGINX</code>, and <code>IIS</code>.</td></tr><tr><td><code>Databases</code></td><td valign="top">Databases (<code>DBs</code>) store and retrieve the web application data. Some examples of relational databases are <code>MySQL</code>, <code>MSSQL</code>, <code>Oracle</code>, <code>PostgreSQL</code>, while examples of non-relational databases include <code>NoSQL</code> and <code>MongoDB</code>.</td></tr><tr><td><p><code>Development</code></p><p> <code>Frameworks</code></p></td><td valign="top">Development Frameworks are used to develop the core Web Application. Some well-known frameworks include <code>Laravel</code> (<code>PHP</code>), <code>ASP.NET</code> (<code>C#</code>), <code>Spring</code> (<code>Java</code>), <code>Django</code> (<code>Python</code>), and <code>Express</code> (<code>NodeJS JavaScript</code>).</td></tr></tbody></table>
