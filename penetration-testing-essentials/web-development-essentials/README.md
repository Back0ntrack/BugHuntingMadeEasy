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

## Web Application&#x20;

<figure><img src="../../.gitbook/assets/Web Application (2).png" alt=""><figcaption></figcaption></figure>

### Frontend (Client-Side)

* **HTML (HyperText Markup Langauge):** _structure of the webpage_
* **CSS (Cascading Style Sheets):** _styling, layout, animations_
  * **CSS Frameworks:** _Bootstrap, Tailwind CSS etc._
* **JavaScript (JS):** _interactivity, logic, DOM manipulation_
* **Frontend Frameworks/Libraries:** _React.js, Angular, Vue.js, Solid.js, Svelte_

> _A **frontend framework** is a **pre-built collection of code, structures, and tools** that helps developers build user interfaces faster and more efficiently instead of writing everything from scratch._

{% hint style="info" %}
_Frameworks = shortcuts + structure + consistency_
{% endhint %}

### Backend (Server-Side)

* **Server-Side Languages:** _(handles business logic, processes requests)_
  * **PHP:** _server-side scripting language commonly used for web apps and WordPress._
  * **Python:** _versatile programming language known for simplicity and readability._
  * **Node.js:** _JavaScript runtime that lets you run JS code on the server._
  * **Java:** _robust, object-oriented language widely used in enterprise systems._
  * **Ruby (Rails):** _Ruby framework following “convention over configuration” for rapid development._
  * **.NET (C#, ASP.NET):** _.NET framework for building dynamic, server-side web applications._
* **Database Layer (Data Storage & Retrieval)**
  * **Relational Databases (SQL-based)**
    * _MySQL / MariaDB (commonly paired with PHP in LAMP/XAMPP)_
    * _PostgreSQL (advanced relational DB)_
    * _Microsoft SQL Server (enterprise DB)_
  * **NoSQL Databases (non-relational)**
    * _MongoDB (document-based, popular with Node.js/MEAN/MERN stack)_
    * _Redis (in-memory, caching, sessions)_
    * _Cassandra (distributed, scalable DB)_
* **Web Servers and Operating Systems**&#x20;
  * **Windows Based**
    * **IIS - Internet Information Services:** _.NET, ASP.NET, C#_
  * **All Rounder (Both Linux and Windows)**
    * **Apache:** _PHP, Laravel, Python (with mod\_wsgi), Perl_
      * **Nginx:** _PHP (via PHP-FPM), Laravel, Python (uWSGI/Gunicorn), Node.js (reverse proxy), Ruby on Rails_
      * **Caddy:** _Go apps, PHP (via FastCGI), Node.js, Python_
      * **LiteSpeed:** _PHP, Laravel (optimized for WordPress and PHP apps)_
      * **Tomcat:** _Java, Spring Boot (Servlet, JSP apps)_
      * **Jetty:** _Java, Spring apps_
* **Middleware and APIs**
  * _REST API_
  * _GraphQL_
  * _gRPC/SOAP_
* **Supporting Services**
  * **Caching** _(Redis, Memcached — speeds up responses)_
  * **Message Queues** _(RabbitMQ, Kafka — background tasks, async jobs)_
  * **CDN (Content Delivery Network)** _(Cloudflare, Akamai — speeds up static file delivery)_
  * **Load Balancer** _(distributes traffic across servers)_
  * **Reverse Proxy** _(adds security, routes requests)_
