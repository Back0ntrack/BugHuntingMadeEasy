# JavaScript (JS)

## Basics

JavaScript (often abbreviated as JS) is a high-level, interpreted programming language primarily used for adding interactivity and dynamic behavior to web pages. It's a versatile, multi-paradigm language supporting object-oriented, imperative, and functional programming styles, and it's dynamically typed, meaning variables don't need explicit type declarations and types are determined at runtime.

### What JavaScript can do  ?&#x20;

Its role in web development is to handle the logic and behavior layer: while HTML structures the content and CSS styles it, JavaScript makes it interactive. For example, it can validate form inputs before submission, update page elements in real-time (like showing/hiding sections), fetch data from APIs, or create animations.

### **Where JavaScript runs**

Primarily, JavaScript runs in web browsers on the client side. It can also run on servers (e.g., with Node.js for backend logic), in non-browser environments like desktop apps (using Electron), or in IoT devices.

1. **Browser (client-side)**
   * Every major browser has a JS engine:
     * Chrome/Edge → V8
     * Firefox → SpiderMonkey
     * Safari → JavaScript Core
   * In browsers, JS interacts with:
     * **DOM (Document Object Model):** structure of the page.
     * **BOM (Browser Object Model):** browser APIs (window, navigator, location).
     * **Web APIs:** fetch, localStorage, WebSockets, etc.
2. **Server (Node.js, Deno, Bun)**
   * Node.js (built on V8) allows JS outside the browser — on servers, CLIs, automation, etc.
   * Provides system APIs (filesystem, network, processes).

## The Real Scenario

When you open a web page (say `https://example.com`), the process goes like this:

#### 1. Browser Loads the Page

* The browser requests the HTML from the server.
* While parsing the HTML:
  * If it encounters a `<script>` tag, it pauses rendering (unless `async` or `defer` is used).
  * Downloads the script (if external).
  * Executes the JS using its built-in engine (V8 in Chrome, SpiderMonkey in Firefox, etc.).

#### 2. JS Gains Access to Browser APIs

Once the code runs, it doesn’t run in isolation — it runs inside the **browser environment**. This environment gives JS access to:

**(A) DOM (Document Object Model)**

* A **tree-like structure** that represents all HTML elements as objects.
*   Example:

    ```html
    <h1 id="title">Hello</h1>
    ```

    JavaScript can do:

    ```js
    document.getElementById("title").innerText = "Hi, Abhishek!";
    ```

    → Changes what the user sees immediately.

DOM is the bridge between **static HTML** and **dynamic JavaScript-driven behavior**.

**(B) BOM (Browser Object Model)**

* Represents the **browser window and environment**, beyond just the HTML page.
* Examples:
  * `window.open("https://google.com")` → opens new tab/window.
  * `navigator.userAgent` → tells what browser/device is being used.
  * `location.href = "/login"` → redirects user.
  * `history.back()` → goes back in browsing history.

The BOM is basically “what the browser itself exposes to JS.”

**(C) Web APIs (Extra Features)**

Modern browsers give JS access to a rich set of APIs:

* **Networking**
  * `fetch("api/data")` → send HTTP requests without reloading the page (AJAX concept).
  * `WebSocket` → real-time communication with server.
* **Storage**
  * `localStorage.setItem("key","value")` → persists data in the browser (survives reloads).
  * `sessionStorage` → data valid only for current tab session.
  * `document.cookie` → small pieces of data sent with requests.
  * `IndexedDB` → Think of it as the browser’s **local database (**&#x4E;oSQL **database) engine** that web apps can use offline.
* **Other APIs**
  * `Geolocation` → get user’s location.
  * `Notification` → show desktop notifications.
  * `Service Workers` → offline functionality.

***

#### 3. Execution Flow (Simplified Timeline)

1. HTML parsed.
2. Browser sees `<script>` → sends code to JS engine.
3. JS engine runs code → interacts with **DOM**, **BOM**, **Web APIs**.
4. Modifications are **reflected live** in the user’s browser window.

***

#### Example in Action

{% code overflow="wrap" %}
```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <title>Sensitive Browser Surfaces Enumerator</title>
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <style>
    body{font-family:system-ui,-apple-system,"Segoe UI",Roboto,Arial;padding:18px;background:#fafafa;color:#111}
    h1{margin:0 0 12px}
    section{background:#fff;border:1px solid #e6e6e6;border-radius:8px;padding:12px;margin:10px 0;box-shadow:0 1px 2px rgba(0,0,0,0.03)}
    .row{display:flex;gap:8px;align-items:center;flex-wrap:wrap}
    button{padding:6px 10px;border-radius:6px;border:1px solid #cfcfcf;background:#fff;cursor:pointer}
    pre{background:#f7f7f7;padding:10px;border-radius:6px;overflow:auto;max-height:260px}
    video{max-width:240px;margin-top:8px;border:1px solid #ccc;border-radius:6px}
    .muted{color:#666;font-size:0.9rem}
  </style>
</head>
<body>
  <h1>Sensitive Browser Surfaces Enumerator</h1>
  <p class="muted">Dummy values are preloaded to simulate sensitive data. Nothing leaves this page.</p>

  <!-- No permission required -->
  <section id="no-perm">
    <h2>No Permission Needed</h2>

    <h3>Storage & Cookies</h3>
    <div class="row">
      <button id="inspect-storage">Inspect storage</button>
      <button id="inspect-cookies">Inspect cookies</button>
    </div>
    <pre id="storage-out">Not inspected yet.</pre>

    <h3>Service Worker & Cache API</h3>
    <div class="row">
      <button id="check-sw">Check SW / Cache</button>
    </div>
    <pre id="sw-out">Not checked yet.</pre>

    <h3>WebRTC Support</h3>
    <div class="row">
      <button id="check-webrtc">Check WebRTC</button>
    </div>
    <pre id="webrtc-out">Not checked yet.</pre>

    <h3>Sensors (availability only)</h3>
    <div class="row">
      <button id="check-sensors">Check sensors</button>
    </div>
    <pre id="sensors-out">Not checked yet.</pre>
  </section>

  <!-- Permission required -->
  <section id="with-perm">
    <h2>Permission Needed</h2>

    <h3>Geolocation</h3>
    <div class="row">
      <button id="req-geo">Request geolocation</button>
      <button id="clear-geo">Clear</button>
    </div>
    <pre id="geo-out">Not requested.</pre>

    <h3>Camera / Microphone / Screen</h3>
    <div class="row">
      <button id="req-camera">Request camera</button>
      <button id="req-mic">Request mic</button>
      <button id="req-screen">Request screen</button>
      <button id="stop-media">Stop media</button>
    </div>
    <pre id="media-out">No media active.</pre>
    <video id="video-preview" autoplay muted playsinline></video>

    <h3>Clipboard</h3>
    <div class="row">
      <button id="read-clipboard">Read clipboard</button>
      <button id="write-clipboard">Write sample</button>
    </div>
    <pre id="clipboard-out">No clipboard action yet.</pre>
  </section>

<script>
  // --- preload dummy sensitive-like values ---
  localStorage.setItem("jwt_token","eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.fake.payload.signature");
  sessionStorage.setItem("csrf_token","a1b2c3d4e5f6g7h8");
  document.cookie="SESSIONID=abcd1234efgh5678; path=/; SameSite=Lax";
  document.cookie="user_pref=darkmode=true; path=/";

  // --- preload dummy IndexedDB ---
  (function setupIndexedDB(){
    const request = indexedDB.open("demoDB", 1);
    request.onupgradeneeded = function(e) {
      const db = e.target.result;

      // Users store
      if (!db.objectStoreNames.contains("users")) {
        const usersStore = db.createObjectStore("users", { keyPath: "id" });
        usersStore.add({id: 1, username: "alice", email: "alice@example.com", token: "refresh-1234-abcdef"});
        usersStore.add({id: 2, username: "bob", email: "bob@example.com", token: "refresh-5678-ghijkl"});
      }

      // Transactions store
      if (!db.objectStoreNames.contains("transactions")) {
        const txnStore = db.createObjectStore("transactions", { keyPath: "txnId" });
        txnStore.add({txnId: "txn001", amount: 199.99, card: "4111111111111111"});
        txnStore.add({txnId: "txn002", amount: 59.49, card: "5500000000000004"});
      }
    };
  })();

  function infoToText(obj){ try{return JSON.stringify(obj,null,2);}catch(e){return String(obj);} }

  // --- No-permission section ---
  document.getElementById('inspect-storage').addEventListener('click', ()=>{
    const out={};
    try{ out.localStorage=Object.keys(localStorage).map(k=>k+': '+localStorage.getItem(k)); }catch(e){ out.localStorage='blocked'; }
    try{ out.sessionStorage=Object.keys(sessionStorage).map(k=>k+': '+sessionStorage.getItem(k)); }catch(e){ out.sessionStorage='blocked'; }
    out.indexedDB="loading...";
    document.getElementById('storage-out').textContent=infoToText(out);

    // Fetch IndexedDB contents
    const dbReq = indexedDB.open("demoDB", 1);
    dbReq.onsuccess = function(e){
      const db = e.target.result;
      const tx = db.transaction(db.objectStoreNames, "readonly");
      const result = {};
      let pending = db.objectStoreNames.length;

      Array.from(db.objectStoreNames).forEach(name=>{
        const store = tx.objectStore(name);
        const getAllReq = store.getAll();
        getAllReq.onsuccess = function(){
          result[name] = getAllReq.result;
          if (--pending === 0) {
            document.getElementById('storage-out').textContent += "\nIndexedDB -> " + infoToText(result);
          }
        };
      });
    };
  });

  document.getElementById('inspect-cookies').addEventListener('click', ()=>{
    document.getElementById('storage-out').textContent='document.cookie: '+(document.cookie||'(no cookies)');
  });

  document.getElementById('check-sw').addEventListener('click', async ()=>{
    const out={serviceWorker:!!navigator.serviceWorker,cacheAPI:!!window.caches};
    try{ out.regs=await navigator.serviceWorker.getRegistrations(); }catch(e){ out.regs='not accessible'; }
    document.getElementById('sw-out').textContent=infoToText(out);
  });

  document.getElementById('check-webrtc').addEventListener('click', ()=>{
    const out={RTCPeerConnection:!!window.RTCPeerConnection,getUserMedia:!!(navigator.mediaDevices&&navigator.mediaDevices.getUserMedia)};
    document.getElementById('webrtc-out').textContent=infoToText(out);
  });

  document.getElementById('check-sensors').addEventListener('click', ()=>{
    const out={DeviceMotion:!!window.DeviceMotionEvent,DeviceOrientation:!!window.DeviceOrientationEvent};
    document.getElementById('sensors-out').textContent=infoToText(out);
  });

  // --- Permission section ---
  document.getElementById('req-geo').addEventListener('click', ()=>{
    const out=document.getElementById('geo-out');
    if(!navigator.geolocation){out.textContent='Not supported';return;}
    navigator.geolocation.getCurrentPosition(
      pos=>{ out.textContent='Coords (rounded): '+pos.coords.latitude.toFixed(2)+','+pos.coords.longitude.toFixed(2); },
      err=>{ out.textContent='Error: '+err.message; });
  });
  document.getElementById('clear-geo').addEventListener('click',()=>document.getElementById('geo-out').textContent='Not requested.');

  let activeStream=null;
  const video=document.getElementById('video-preview');

  async function reqMedia(constraints,label){
    const out=document.getElementById('media-out');
    try{
      activeStream=await navigator.mediaDevices.getUserMedia(constraints);
      out.textContent=label+' granted. Tracks: '+activeStream.getTracks().length;
      if(constraints.video||constraints.displaySurface) video.srcObject=activeStream;
    }catch(e){ out.textContent=label+' denied: '+e.message; }
  }

  document.getElementById('req-camera').addEventListener('click',()=>reqMedia({video:true},'Camera'));
  document.getElementById('req-mic').addEventListener('click',()=>reqMedia({audio:true},'Microphone'));
  document.getElementById('req-screen').addEventListener('click',async ()=>{
    const out=document.getElementById('media-out');
    try{
      activeStream=await navigator.mediaDevices.getDisplayMedia({video:true});
      out.textContent='Screen granted. Tracks: '+activeStream.getTracks().length;
      video.srcObject=activeStream;
    }catch(e){ out.textContent='Screen denied: '+e.message; }
  });

  document.getElementById('stop-media').addEventListener('click',()=>{
    if(activeStream){ activeStream.getTracks().forEach(t=>t.stop()); activeStream=null; video.srcObject=null; document.getElementById('media-out').textContent='Stopped media.'; }
  });

  document.getElementById('read-clipboard').addEventListener('click',async()=>{
    const out=document.getElementById('clipboard-out');
    if(!navigator.clipboard||!navigator.clipboard.readText){out.textContent='Not supported';return;}
    try{const t=await navigator.clipboard.readText();out.textContent='Clipboard length: '+(t?t.length:0)+'\nContent: '+t;}catch(e){out.textContent='Denied: '+e.message;}
  });

  document.getElementById('write-clipboard').addEventListener('click',async()=>{
    const out=document.getElementById('clipboard-out');
    if(!navigator.clipboard||!navigator.clipboard.writeText){out.textContent='Not supported';return;}
    try{await navigator.clipboard.writeText('SensitiveToken-FAKE-XYZ123');out.textContent='Sample sensitive-looking token written.';}catch(e){out.textContent='Failed: '+e.message;}
  });
</script>
</body>
</html>

```
{% endcode %}

> The above scripts shows the power of JavaScript.&#x20;

{% code overflow="wrap" %}
```html
<!-- Send Push Notification -->
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <title>Device Info & Push/Sync Enumerator</title>
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <style>
    body{font-family:system-ui,-apple-system,"Segoe UI",Roboto,Arial;padding:18px;background:#fafafa;color:#111}
    h1{margin:0 0 12px}
    section{background:#fff;border:1px solid #e6e6e6;border-radius:8px;padding:12px;margin:10px 0;box-shadow:0 1px 2px rgba(0,0,0,0.03)}
    .row{display:flex;gap:8px;align-items:center;flex-wrap:wrap}
    button{padding:6px 10px;border-radius:6px;border:1px solid #cfcfcf;background:#fff;cursor:pointer}
    pre{background:#f7f7f7;padding:10px;border-radius:6px;overflow:auto;max-height:260px}
    .muted{color:#666;font-size:0.9rem}
  </style>
</head>
<body>
  <h1>Device Info & Push/Sync Enumerator</h1>
  <p class="muted">Dummy sensitive-looking values are shown. Nothing leaves this page.</p>

  <!-- No permission required -->
  <section id="no-perm">
    <h2>No Permission Needed</h2>

    <h3>Device Memory & Hardware Concurrency</h3>
    <div class="row">
      <button id="check-device">Check Device Info</button>
    </div>
    <pre id="device-out">Not checked yet.</pre>
  </section>

  <!-- Permission required -->
  <section id="with-perm">
    <h2>Permission Needed</h2>

    <h3>Push Notifications</h3>
    <div class="row">
      <button id="check-push">Check Push Permission</button>
      <button id="send-push" disabled>Send Demo Notification</button>
    </div>
    <pre id="push-out">Not checked yet.</pre>

    <h3>Background Sync</h3>
    <div class="row">
      <button id="check-sync">Check Background Sync Support</button>
    </div>
    <pre id="sync-out">Not checked yet.</pre>
  </section>

<script>
  function infoToText(obj){ try{return JSON.stringify(obj,null,2);}catch(e){return String(obj);} }

  // --- Device memory & concurrency ---
  document.getElementById('check-device').addEventListener('click', ()=>{
    const mem = navigator.deviceMemory || "unknown";
    const cpu = navigator.hardwareConcurrency || "unknown";
    const fakeSensitive = {
      deviceMemory: mem + " GB (could hint at high-end/low-end device)",
      cpuThreads: cpu + " logical processors",
      fakeSensitiveTag: "Looks like a fingerprint vector"
    };
    document.getElementById('device-out').textContent = infoToText(fakeSensitive);
  });

  // --- Push notifications ---
  const pushOut=document.getElementById('push-out');
  const sendPushBtn=document.getElementById('send-push');

  document.getElementById('check-push').addEventListener('click', async ()=>{
    if (!("Notification" in window)) {
      pushOut.textContent = "Push Notifications not supported in this browser.";
      return;
    }
    try {
      const perm = await Notification.requestPermission();
      if (perm === "granted") {
        pushOut.textContent = "Permission granted.\nDummy subscription:\n" +
          infoToText({
            endpoint: "https://fakeservice.push.example/subscription/abc123",
            keys: {
              p256dh: "FAKE_PUBLIC_KEY_XYZ987",
              auth: "FAKE_AUTH_TOKEN_123"
            }
          });
        sendPushBtn.disabled=false;
      } else {
        pushOut.textContent = "Push permission: " + perm;
        sendPushBtn.disabled=true;
      }
    } catch(e){
      pushOut.textContent = "Error requesting push permission: " + e.message;
      sendPushBtn.disabled=true;
    }
  });

  // Local demo notification (not real server push)
  sendPushBtn.addEventListener('click', ()=>{
    new Notification("🔔 Demo Notification", {
      body: "This is a locally triggered notification.\n(fake push simulation)",
      icon: "https://via.placeholder.com/64" // dummy icon
    });
  });

  // --- Background Sync ---
  document.getElementById('check-sync').addEventListener('click', ()=>{
    const out=document.getElementById('sync-out');
    if ('serviceWorker' in navigator && 'SyncManager' in window) {
      out.textContent = "Background Sync supported.\nDummy registration:\n" +
        infoToText({tag:"sync-fake-upload-001", status:"pending"});
    } else {
      out.textContent = "Background Sync not supported in this browser.";
    }
  });
</script>
</body>
</html>

```
{% endcode %}

There are other things that JavaScript can do with the browser, but to understand them, we first need to learn other web application concepts. So, let's move on.&#x20;

## Learning JS From Programming Concept&#x20;

## Variables

A variable is like a container that holds data that can be reused or updated later in the program.&#x20;

JavaScript has **three main ways** to declare variables:

### Var

* **`var`** → old, function-scoped, hoisted (dangerous quirks).

```javascript
function test() {
  console.log(x); // undefined (hoisted)
  var x = 10;
  console.log(x); // 10
}
```

### Let

* **`let`** → modern, block-scoped, re-assignable.

```javascript
if (true) {
  let y = 20;
  console.log(y); // 20
}
console.log(y); // ReferenceError
```

### Const

* **`const`** → modern, block-scoped, cannot be re-assigned.

```javascript
const z = 30;
z = 40; // ❌ TypeError

const user = {name: "Alice"};
user.name = "Bob"; // ✅ allowed
```

### Javascript template literal&#x20;

```javascript
const a = 5;
const b = 10;
console.log(`The sum of a and b is ${a + b}`); // Output: The sum of a and b is 15
```

### Data Types in JS

<figure><img src="../../../.gitbook/assets/image (6) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

JS is **loosely typed** → variable type can change on the fly.

```
let data = 42;        // number
data = "forty two";   // now string    
console.log(0 == "0");    // true
console.log(false == "0"); // true
console.log([] == 0);     // true
```

This behavior of JavaScript can be used to bypass checks.&#x20;

### Primitive Datatypes <a href="#primitive-datatypes" id="primitive-datatypes"></a>

Primitive datatypes represent single values and are immutable.

**1.** [**Number**](https://www.geeksforgeeks.org/javascript/javascript-numbers/)**:** Represents numeric values (integers and decimals).

```
let n = 42;
let pi = 3.14;
```

**2.** [**String**](https://www.geeksforgeeks.org/javascript/javascript-strings/)**:** Represents text enclosed in single or double quotes.

```
let s = "Hello, World!";
```

**3.** [**Boolean**](https://www.geeksforgeeks.org/javascript/javascript-boolean/)**:** Represents a logical value (true or false).

```
let bool= true;
```

**4.** [**Undefined**](https://www.geeksforgeeks.org/javascript/undefined-in-javascript/)**:** A variable that has been declared but not assigned a value.

```
let notAssigned;
console.log(notAssigned);
```

**Output**

```
undefined
```

**5.** [**Null**](https://www.geeksforgeeks.org/javascript/null-in-javascript/)**:** Represents an intentional absence of any value.

```
let empty = null;
```

**6.** [**Symbol**](https://www.geeksforgeeks.org/javascript/javascript-symbol-method/)**:** Represents unique and immutable values, often used as object keys.

```
let sym = Symbol('unique');
```

**7.** [**BigInt**](https://www.geeksforgeeks.org/javascript/javascript-bigint/)**:** Represents integers larger than Number.MAX\_SAFE\_INTEGER.

```
let bigNumber = 123456789012345678901234567890n;
```

## Non-Primitive Datatypes <a href="#nonprimitive-datatypes" id="nonprimitive-datatypes"></a>

Non-primitive types are objects and can store collections of data or more complex entities.

**1.** [**Object**](https://www.geeksforgeeks.org/javascript/objects-in-javascript/)**:** Represents key-value pairs.

```
let obj = {
    name: "Amit",
    age: 25
};
```

**2.**[ **Array**](https://www.geeksforgeeks.org/javascript/javascript-arrays/)**:** Represents an ordered list of values.

```
let a = ["red", "green", "blue"];
```

**3.** [**Function:**](https://www.geeksforgeeks.org/javascript/functions-in-javascript/) Represents reusable blocks of code.

```
function fun() {
    console.log("GeeksforGeeks");
}
```

{% hint style="info" %}
In JavaScript, `document.write()` is used to write directly to the HTML output stream.&#x20;
{% endhint %}

## Operators

### Arithmetic Operator

| Operator | Description                  |
| -------- | ---------------------------- |
| +        | Addition                     |
| -        | Subtraction                  |
| \*       | Multiplication               |
| \*\*     | Exponentiation               |
| /        | Division                     |
| %        | Modulus (Division Remainder) |
| ++       | Increment                    |
| --       | Decrement                    |

### Assignment Operator

| Operator | Example   | Same As      |
| -------- | --------- | ------------ |
| =        | x = y     | x = y        |
| +=       | x += y    | x = x + y    |
| -=       | x -= y    | x = x - y    |
| \*=      | x \*= y   | x = x \* y   |
| /=       | x /= y    | x = x / y    |
| %=       | x %= y    | x = x % y    |
| \*\*=    | x \*\*= y | x = x \*\* y |

### Logical Operator

| Operator | Description |
| -------- | ----------- |
| &&       | logical and |
| \|\|     | logical or  |
| !        | logical not |

## Conditionals

### If-Else If-Else Statement

```javascript
// JavaScript program to illustrate nested-if statement
let i = 20;
if (i == 10)
    console.log("i is 10");
else if (i == 15)
    console.log("i is 15");
else if (i == 20)
    console.log("i is 20");
else
    console.log("i is not present");
```

### Switch Case Statement

```javascript
let day = 3;
let dayName;

switch (day) {
    case 1:
        dayName = "Monday";
        break;
    case 2:
        dayName = "Tuesday";
        break;
    case 3:
        dayName = "Wednesday";
        break;
    case 4:
        dayName = "Thursday";
        break;
    case 5:
        dayName = "Friday";
        break;
    case 6:
        dayName = "Saturday";
        break;
    case 7:
        dayName = "Sunday";
        break;
    default:
        dayName = "Invalid day";
}

console.log(dayName);
```

## Loops&#x20;

```javascript
//------------------------------------
// 1. FOR LOOP
// Use case: when we know how many times to run.
// Example: printing the first 5 students in a class list.
let students = ["Alice", "Bob", "Charlie", "David", "Eve"];

for (let i = 0; i < students.length; i++) {
  console.log("Student #" + (i+1) + ": " + students[i]);
}

//------------------------------------
// 2. WHILE LOOP
// Use case: when we don’t know how many times beforehand, 
// keep running until a condition becomes false.
// Example: countdown timer.
let time = 5;

while (time > 0) {
  console.log("Countdown: " + time + " seconds left");
  time--;
}

console.log("⏰ Time's up!");

//------------------------------------
// 3. DO...WHILE LOOP
// Use case: when we want code to run at least once,
// even if the condition is false on the first check.
// Example: asking for password until correct.
let password;
const correctPassword = "secret123";

do {
  password = "secret123"; // Replace prompt with fixed value for demo
  console.log("Entered: " + password);
} while (password !== correctPassword);

console.log("✅ Access granted");

//------------------------------------
// 4. FOR...OF LOOP
// Use case: iterating over values of an iterable (arrays, strings, sets).
// Example: going through shopping items.
let cart = ["Milk", "Bread", "Eggs"];

for (let item of cart) {
  console.log("🛒 Buying: " + item);
}

//------------------------------------
// 5. FOR...IN LOOP
// Use case: iterating over keys (properties) of an object.
// Example: checking user profile fields.
let user = {
  name: "Alice",
  age: 25,
  country: "India"
};

for (let key in user) {
  console.log(key + " → " + user[key]);
}

//------------------------------------
// 6. FOREACH METHOD
// Not a loop keyword but common in arrays.
// Executes a function once for each array element.
// Example: sending reminder messages.
let emails = ["a@example.com", "b@example.com", "c@example.com"];

emails.forEach(function(email) {
  console.log("📧 Sending reminder to: " + email);
});

```

## XHR (XMLHttpRequest)

XHR is a JavaScript object that allows web pages to make HTTP requests to servers without reloading the entire page. Despite its name, XHR can request **any data type** (not just XML): JSON, HTML, plain text, binary, etc.

### Basic Usage&#x20;

```javascript
// 1. Create XMLHttpRequest object
let xhr = new XMLHttpRequest();

// 2. Configure it: method, URL, async or not
xhr.open("GET", "https://api.example.com/data", true);

// 3. Define what happens when the request is done
xhr.onload = function () {
  if (xhr.status === 200) {
    console.log("Response:", xhr.responseText); // raw text
  } else {
    console.error("Error:", xhr.status);
  }
};

// 4. Handle network errors
xhr.onerror = function () {
  console.error("Request failed");
};

// 5. Send request
xhr.send();
```

### POST with JSON

```javascript
let xhr = new XMLHttpRequest();
xhr.open("POST", "https://api.example.com/data", true);
xhr.setRequestHeader("Content-Type", "application/json");

xhr.onload = function () {
  if (xhr.status === 200) {
    console.log("Success:", xhr.responseText);
  }
};

xhr.onerror = function () {
  console.error("Request failed");
};

xhr.send(JSON.stringify({ user: "Abhishek", role: "Pentester" }));

```

**XMLHttpRequest (XHR)** is a JavaScript API introduced in the early 2000s that allows web pages to send and receive HTTP requests asynchronously without reloading the entire page, forming the backbone of AJAX and “Web 2.0” applications like Gmail and Google Maps. It lets developers request and submit data (in formats such as JSON, XML, text, or binary) using methods like `open()`, `send()`, and `setRequestHeader()`, while monitoring request progress through properties like `readyState` and `status`. Although powerful, XHR is verbose, relies heavily on callbacks, and has clunky error handling, which often makes the code harder to manage.&#x20;

## fetch request&#x20;

`fetch()` is a browser API for making HTTP requests (to servers, APIs, files, etc.). It's `promise-based` (returns a `Promise`), unlike the older XMLHttpRequest (XHR).&#x20;

### Request options

```javascript
fetch("https://api.example.com/data", {
  method: "POST",        // HTTP method (GET, POST, PUT, DELETE, PATCH, etc.)
  headers: {
    "Content-Type": "application/json",   // Request headers
    "Authorization": "Bearer token123"   // Custom headers
  },
  body: JSON.stringify({ name: "Abhishek", role: "pentester" }), // Request body
  mode: "cors",           // cors, no-cors, same-origin
  credentials: "include", // include, same-origin, omit (for cookies/auth)
  cache: "no-cache",      // default, no-store, reload, force-cache
  redirect: "follow",     // follow, error, manual
});
```

## JS DOM&#x20;

* When a web page is loaded, the browser creates a Document Object Model of the page.&#x20;
* The HTML DOM is a standard for how to get, change, add, or delete HTML elements.&#x20;
* JavaScript fully controls the page: it can modify elements, attributes, and styles; add or remove elements/attributes; and handle or create events.

<figure><img src="../../../.gitbook/assets/image (13) (1).png" alt=""><figcaption></figcaption></figure>

### DOM Manipulation

<figure><img src="../../../.gitbook/assets/image (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (5) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

### Finding HTML Elements

| Method                                  | Description                   |
| --------------------------------------- | ----------------------------- |
| document.getElementById(_id_)           | Find an element by element id |
| document.getElementsByTagName(_name_)   | Find elements by tag name     |
| document.getElementsByClassName(_name_) | Find elements by class name   |

### Changing HTML Elements

| Property                                        | Description                                                                              |
| ----------------------------------------------- | ---------------------------------------------------------------------------------------- |
| _element_.innerHTML =  _new html content_       | Change the inner HTML of an element                                                      |
| _element_._attribute = new value_               | Change the attribute value of an HTML element. E.g., `src` attribute, `href` attribute.  |
| _element_.style._property = new style_          | Change the style of an HTML element                                                      |
| **Method**                                      | **Description**                                                                          |
| _element_.setAttribut&#x65;_(attribute, value)_ | Change the attribute value of an HTML element                                            |

### Reacting to Events&#x20;

A JavaScript can be executed when an event occurs, like when a user clicks on an HTML element.&#x20;

Examples of HTML events:

* When a user clicks the mouse
* When a web page has loaded
* When an image has been loaded
* When the mouse moves over an element
* When an input field is changed
* When an HTML form is submitted
* When a user strokes a key

## JS as a Goldmine

### Hidden Endpoints&#x20;

A modern web application's JavaScript is often a client that communicates with a backend API. Developers may not link all API endpoints from the website's navigation. However, the JavaScript code will contain the URLs for every endpoint it needs to interact with. By analyzing the code, you can find these hidden or unlinked endpoints.

* Example: A developer might use a hidden API to load user profile information only after a specific button is clicked, or for administrative functions. The HTML might not contain a direct link, but the JavaScript will have the `fetch()` or `XMLHttpRequest` call.

```javascript
// In a script file, you might find this
function loadAdminDashboard() {
    fetch('/api/v1/admin/dashboard').then(response => {
        // ...handle response
    });
}
```

An attacker can spot `/api/v1/admin/dashboard` in the code, and then try to access it directly, even without administrator privileges. This can lead to broken access control if the server doesn't properly check the user's permissions.

***

### Sensitive Data Leaks

Developers might inadvertently include sensitive information in the JavaScript code that gets sent to the browser. This is a common mistake and can be a goldmine for an attacker.

* Example:
  * Hardcoded secrets: A developer might forget to remove an API key, a database connection string, or a secret for a third-party service before pushing the code to production.

```javascript
// In a config.js file, for example
const API_KEY = "sk_live_1234567890abcdef";
const DATABASE_URL = "postgres://user:password@db.example.com/production_db";
```

An attacker can use this leaked API key to make unauthorized requests, potentially costing the company money or accessing sensitive data.

***

### Vulnerable Parameter and Logic Flaws&#x20;

JavaScript code gives you insight into the request parameters the application uses and the expected values. By seeing how the front end is structured, you can learn how to craft a malicious request. This is not a bug in itself, but a prerequisite for finding other bugs.

* Example: A script might use a parameter named `user_id` to fetch user information. The front end might only allow the current user's ID to be used, but by analyzing the code, an attacker can discover this and try to use other IDs. This can lead to an insecure direct object reference (IDOR) vulnerability if the server doesn't properly validate that the user is authorized to access the requested data.

```javascript
// The code fetches the current user's profile
const userId = getLoggedInUserId();
fetch(`/api/profile?id=${userId}`);

// An attacker sees this and tries to manually send a request:
// GET /api/profile?id=123 (where 123 is another user's ID)
```

***

### Comment and Hidden Functionality

Developers often leave comments in their code for future reference. These comments can sometimes reveal valuable information about the application's functionality or potential weaknesses. Additionally, JavaScript might contain code for features that were never fully implemented or were hidden from the public.

* Example: A developer might leave a note about an old, insecure endpoint that was "temporarily disabled."

```javascript
// TODO: This endpoint is insecure and needs to be rewritten. For now, it's disabled.
// fetch('/api/v2/old/insecure-data-dump')
```

An attacker will see this and immediately test that `/api/v2/old/insecure-data-dump` endpoint to see if it's truly disabled or if they can still access it. This can lead to data leaks or other vulnerabilities.
