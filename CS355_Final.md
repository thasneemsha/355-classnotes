 ## 1. What Are Streams and Their Use Cases in Node.js?

> Streams are a way to represent contiguous data in programming. 

  They allow applications to begin processing data immediately upon receiving it, rather than waiting for the entire data to be downloaded or loaded into memory.
  This makes streams highly efficient for handling large, dynamic, or real-time data sources.

 According to Structure and Interpretation of Computer Programs (SICP - Structure and Interpretation of Computer Programs), 
 >streams are defined as delayed list structures, where data is consumed head-first and expands as new data arrives.
 This means that only small portions of data are processed at a time rather than storing everything in memory at once.

 Because streams are abstract data structures, their implementation varies depending on the software system. 
 * In Node.js, streams are implemented using linked lists of buffer objects.
 * Each buffer has a fixed array size, which by default is approximately 2^16 bytes.
 * As data arrives, it fills the current buffer. Once the buffer becomes full, the stream moves to the next buffer.
 * After the buffer is filled, the stream sends data signals and provides observers with copies of the completed buffer sections called chunks. 
  >These chunks are processed before they are removed from the head of the stream, allowing streams to process potentially infinite amounts of data efficiently.
---
### Types of Streams in Node.js

#### 1. Readable Streams - are used to consume or read incoming data.
>Reading files using `fs.createReadStream()`

They are commonly managed using:

* `"data"` event → triggered when a chunk is available
* `"end"` event → triggered when all data has been consumed

#### 2. Writable Streams - are used to send or write data.
>Writing files using `fs.createWriteStream()`

Important methods include:

* `write()` → writes chunks into the stream
* `end()` → indicates that no more data will be written


```javascript
source.on("data", function(chunk) {
    console.log(`New data received: ${chunk}`);
    destination.write(chunk);
});

source.on("end", function() {
    console.log("Finished reading");

    destination.end(function() {
        console.log("Finished writing");
    });
});
```
>In this example:
>* The `"data"` event is triggered whenever a new chunk of data becomes available from the readable stream.
>* The `write()` method sends that chunk to the writable stream.
>* The `"end"` event occurs when there is no more data to read.
>* The `end()` method signals that no more data will be written to the destination stream.


#### 3. Duplex Streams - support both reading and writing operations simultaneously.

#### 4. Transform Streams - are a special type of duplex stream that can modify data while it passes through the stream.

> The `pipe()` Utility Function-  which simplifies the transfer of data from a readable stream directly into a writable stream.

Example:

```javascript
const fs = require('fs');

const readStream = fs.createReadStream('input.txt');
const writeStream = fs.createWriteStream('output.txt');

readStream.pipe(writeStream);
```

---

### Use Cases of Streams

#### 1. Large File Processing

Streams are commonly used when handling very large files such as videos, logs, or database exports. 
Loading an entire large file into memory could consume excessive RAM and slow down the application. By using streams, files can be processed chunk by chunk.

```
Example:
* Reading large `.csv` datasets
* Streaming video files
* File uploads and downloads
```

#### 2. Unknown or Dynamic Endpoints

Some data sources do not have a predefined ending point. In these situations, streams allow applications to continue processing data as long as it is being received.

```
Examples:
* User-generated input
* Live chat systems
* API responses with continuous updates
```

#### 3. Infinite Data Streams

Certain systems generate data continuously without ever stopping. Streams are ideal for processing this type of infinite data.

```
Examples:
* CCTV camera feeds
* Live sensor monitoring
* Real-time stock market updates
* Audio/video broadcasting
```
>### Advantages of Streams
>* Efficient Memory Usage
>* Faster Processing of large data
>* useful for Real-Time Data Handling

---

## 2. 
