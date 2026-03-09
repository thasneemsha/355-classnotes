***Thasneem Mohamed***

# Asynchronous Programming in Node.js


* **Node.js** is a runtime environment that allows JavaScript to be executed outside the browser.
* It is powered by **V8**, Google’s open-source JavaScript engine. Unlike traditional server environments,
* Node.js uses a **single-threaded event loop** to handle multiple operations **asynchronously**, making it highly efficient for I/O-heavy tasks such as networking, file operations, and database queries.
* While JavaScript itself is **single-threaded**, Node.js leverages **libuv**, a C library, to handle asynchronous operations under the hood using threads.
* This means you can start multiple operations without waiting for each one to finish, achieving **concurrency** in your applications.


### Node.js Architecture Overview

Node.js architecture is made up of several layers that allow asynchronous programming while keeping the API simple for developers:

#### Node.js API (JavaScript)

* Provides high-level **interfaces** for interacting with the operating system.
* Abstracts away the details of system calls, letting developers focus on application logic.
* Examples include file system access (`fs` module), networking (`http`, `https`), and timers (`setTimeout`, `setInterval`).

#### Node.js Standard Library

* Most modules are written in JavaScript and are part of Node.js core:

  * `console` → output messages
  * `timers` → scheduling functions
  * `http` → handling HTTP requests and responses
* These **core modules** are optimized for performance and reliability.

#### Node.js Bindings (C/C++ Libraries)
* Node.js provides bindings to **battle-tested C/C++ libraries**:

  * **zlib** → compression and decompression
  * **open-ssl** → encryption, TLS, crypto modules
  * **http-parser** → parsing HTTP requests and responses
  * **c-ares** → DNS resolution

* Bindings bridge **JavaScript and native code**, allowing developers to use powerful low-level libraries from JavaScript seamlessly.

#### V8-JavaScript Engine

**V8** is the engine that executes JavaScript in Node.js. it combine **interpreter + compiler**.

  * Code is first parsed into an Abstract Syntax Tree (AST).
  * The interpreter runs code immediately.
  * The compiler optimizes code to machine code in the background.
  * Once optimized, compiled code replaces interpreted code for faster execution.

* This process repeats in **secondary passes**, allowing V8 to discover further optimizations.

* This design enables **high performance for both synchronous and asynchronous JavaScript**.

#### Libuv and the Event Loop

**libuv** is a multi-platform C library responsible for **asynchronous I/O** in Node.js.

* Uses an **event loop** to manage tasks without blocking the main thread.
* Handles tasks differently based on type:

  1. **CPU-bound tasks** → pushed to a **Task Queue** and processed by libuv threads
  2. **I/O-bound tasks** → sent to the OS for processing (e.g., reading files, network calls)

#### Callbacks

* Asynchronous functions take a **callback** as a secondary argument.
* The callback defines **what happens after the task finishes**.

```javascript
const fs = require('fs');
fs.readFile('example.txt', 'utf8', (err, data) => {
    if(err) console.error(err);
    else console.log(data);
});
```

* In this example, the **file read is non-blocking**, allowing other tasks to run concurrently.

### Concurrency in Node.js

**Concurrency** means starting multiple tasks without waiting for each one to finish.

Node.js achieves concurrency using:

* **Event Loop** → continuously checks for completed tasks and executes callbacks
* **OS asynchronous APIs** → offload I/O tasks to the operating system
* **libuv thread pool** → handles CPU-heavy asynchronous tasks


#### DNS Resolution Example

The **Domain Name System (DNS)** translates human-readable domains into IP addresses.

* Node.js uses the **c-ares library** for DNS resolution.
* Example: resolving multiple domains asynchronously:

```javascript
const dns = require("dns");
const domains = ["venus.cs.qc.cuny.edu", "mars.cs.qc.cuny.edu", "earth.cs.qc.cuny.edu"];

for(let i = 0; i < domains.length; i++){
    resolve(domains[i]);
}

function resolve(domain){
    dns.resolve(domain, (err, records) => {
        if(err) console.error("Failed to resolve", domain);
        else console.log(domain, records);
    });
}
```


* All domain resolutions run **concurrently**.
* This is **non-blocking**: Node.js moves to the next iteration while waiting for DNS responses.

* Can we do the same task at the same time?
> Yes, in parallel. yes the time in which, the time it takes is the slowest time.

* What if we want to doo 7million task at the same time?
* Launching millions of async calls at once can **overwhelm memory and network resources**. So, **batching** or using **queues** to limit concurrent requests.

##### Sequential DNS Resolution

* Sometimes, sequential execution is required to **avoid flooding resources**.

```javascript
const dnsPromises = require("dns").promises;

async function resolveSequential(domains) {
    for (const domain of domains) {
        try {
            const records = await dnsPromises.resolve(domain);
            console.log(domain, records);
        } catch (err) {
            console.error("Failed to resolve", domain);
        }
    }
}

resolveSequential(domains);
```

