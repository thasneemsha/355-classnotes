1. **The .on()**
   >They don’t run line-by-line. They emit events, and you “listen” to them using .on() 

    ```javascript
   object.on('eventName', callback)
   ```
* .on('data') → “new chunk of data arrived”
* .on('error') → “something went wrong”
* .on('end') → “data stream finished”
* .on('close') → “connection closed”

### STREAMS : 

> Sockets + HTTP requests ARE streams internally.

 🔹 Stream Events

| Event      | Meaning         |
| ---------- | --------------- |
| `"data"`   | Chunk received  |
| `"end"`    | Stream finished |
| `"error"`  | Error occurred  |
| `"close"`  | Stream closed   |
| `"pause"`  | Stream paused   |
| `"resume"` | Stream resumed  |


🔹 Stream Methods

| Method     | Meaning         |
| ---------- | --------------- |
| `pipe()`   | Connect streams |
| `read()`   | Read data       |
| `write()`  | Write data      |
| `pause()`  | Stop flow       |
| `resume()` | Resume flow     |


```javascript
   source.on("data", (chunk) => {
    console.log(chunk.toString());
});

  source.on("end", () => {
    console.log("Finished reading");
});

source.on("error", (err) => {
    console.log("Error:", err);
});
```
---

### SOCKET EVENTS + METHODS (TCP sockets)

Sockets are the most “raw” level.

 🔹 Socket Events

| Event       | Meaning                                     |
| ----------- | ------------------------------------------- |
| `"data"`    | Data chunk received                         |
| `"end"`     | No more data coming (half-close)            |
| `"close"`   | Connection fully closed                     |
| `"error"`   | Error occurred                              |
| `"connect"` | Client successfully connected (client-side) |
| `"timeout"` | Socket idle timeout                         |


 🔹 Socket Methods

| Method           | Meaning                                |
| ---------------- | -------------------------------------- |
| `write(data)`    | Send data                              |
| `end()`          | Finish sending + close writable side   |
| `destroy()`      | Force close connection                 |
| `setTimeout(ms)` | Set inactivity timeout                 |
| `setEncoding()`  | Convert buffer to string automatically |



```javascript
socket.on("data", (chunk) => {
    console.log("Received:", chunk.toString());
});

socket.on("error", (err) => {
    console.log("Socket error:", err);
});

socket.on("close", () => {
    console.log("Connection closed");
});

//same as writing the callback function like this:
socket.on('error', errorhandler);
function errorhandler(err){
through err;
}

socket.write("Hello");
socket.end();

```
---

### SERVER EVENTS + METHODS (net.createServer)

> Server handles multiple sockets.

🔹 Server Events

| Event          | Meaning                  |
| -------------- | ------------------------ |
| `"connection"` | New client connected     |
| `"error"`      | Server error             |
| `"close"`      | Server closed            |
| `"listening"`  | Server started listening |

 🔹 Server Methods

| Method         | Meaning            |
| -------------- | ------------------ |
| `listen(port)` | Start server       |
| `close()`      | Stop server        |
| `address()`    | Get server address |

```javascript
server.on("connection", (socket) => {
    console.log("Client connected");
});

server.listen(3000);
```

---
### HTTP MODULE EVENTS + METHODS

> HTTP is built on top of sockets + streams.

🔹 HTTP Server Events

HTTP server is also an EventEmitter.

| Event          | Meaning               |
| -------------- | --------------------- |
| `"request"`    | Incoming HTTP request |
| `"connection"` | TCP connection        |
| `"upgrade"`    | WebSocket upgrade     |
| `"close"`      | Server closed         |

 🔹 Request (IncomingMessage) Events

This is VERY IMPORTANT (stream-based):

| Event     | Meaning                 |
| --------- | ----------------------- |
| `"data"`  | Chunk of request body   |
| `"end"`   | Request body finished   |
| `"error"` | Error in request stream |

 🔹 Response Methods

| Method        | Meaning              |
| ------------- | -------------------- |
| `write()`     | Send response chunk  |
| `end()`       | Finish response      |
| `setHeader()` | Set response headers |
| `writeHead()` | Set status + headers |



```javascript
req.on("data", (chunk) => {
    body += chunk;
});

req.on("end", () => {
    console.log("Request fully received");
});
```
---
