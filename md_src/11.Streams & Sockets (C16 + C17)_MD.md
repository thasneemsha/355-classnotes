

> Thasneem Mohamed 

## What are Streams?

A stream in Node.js is an abstraction used to handle data as a continuous flow of chunks, instead of loading the entire data into memory at once.

Streams allow data to be processed piece by piece as it is being read or written.

Instead of loading all data at once: Read full data → process → store

*Streams*: Read small chunks → process immediately → continue

#### Advantages :

* Efficient memory usage
* Faster processing for large data
* Useful for real-time data handling

#### Examples of usage

* Reading large files
* Video/audio streaming
* HTTP requests and responses
* File copying

Types of Streams :

* Readable stream: source of data
* Writable stream: destination of data
* Duplex stream: both readable and writable
* Transform stream: modifies data while passing through

---

## Conceptual Model of Streams

A stream is conceptually similar to a pipeline where data flows from a source to a destination.

Model :

* Source produces data
* Data flows in small chunks
* Destination consumes data

Chunk data :
Data is divided into small pieces called chunks, which are processed individually.

Internal Buffer : A buffer temporarily stores data chunks before they are processed or passed forward.

---

## Events and Methods in Streams

Streams as Event Emitters :  Streams in Node.js use events to signal different stages of data flow.

Readable Stream Events : 

* data: emitted when a chunk is available
* end: emitted when no more data is available
* error: emitted when an error occurs
* readable: emitted when data is ready to be read

Writable Stream Events :

* drain: indicates it is safe to write more data
* finish: all data has been written
* error: write operation failed
* close: stream is closed

Readable Stream Methods :

* read(): reads a chunk of data
* pipe(): connects readable stream to writable stream
* unpipe(): removes pipe connection
* pause(): stops data flow
* resume(): resumes data flow

Writable Stream Methods :

* write(): writes data to stream
* end(): signals no more data will be written

---

## Buffers and Streams

A buffer is a temporary storage area used to hold binary data while it is being transferred or processed.

#### Why Buffers are Used

* Streams handle data in chunks
* Buffers store chunks temporarily before processing
* Helps manage speed differences between producer and consumer


>* Stream produces data
>*  Buffer stores chunks temporarily
>*  Application processes data from buffer

---

### Standard Input Readable Stream

process.stdin -->  In Node.js, standard input (keyboard input) is a readable stream.

* Initially in paused mode
* Emits data when user enters input

##### Use Case
* Command-line applications
* Interactive programs
* Taking user input in real time

---

## Chunk Manipulation

Chunk manipulation refers to processing or modifying each chunk of data as it flows through a stream.

* Converting chunk to string
* Filtering data
* Transforming format (JSON, text, etc.)
* Enables real-time processing
* Avoids loading full dataset into memory

Input chunk → process chunk → output chunk

---

## readable.end() Event

The end event is triggered when a readable stream has no more data to provide.

* All chunks have been read
* Data source is finished

data → data → data → end

* Closing file reading operations
* Triggering completion logic
* Finalizing processing steps

---

## Pipe Method

The pipe method is used to connect a readable stream directly to a writable stream.


> readableStream.pipe(writableStream)

* Automatically transfers data from source to destination
* Handles data flow internally
* Manages backpressure automatically

Readable stream → pipe → Writable stream

#### Advantages

* Simplifies code
* Reduces manual handling of data chunks
* Improves efficiency

