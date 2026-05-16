>Thasneem Mohamed
## Consuming Third-Party APIs

>A third-party API is an external service that allows applications to communicate and exchange data over the Internet.

APIs are widely used in modern web development because they allow developers to access data or functionality from another system without building everything from scratch. Common examples include weather APIs, payment APIs, map services, social media APIs, and job search APIs.

>API stands for Application Programming Interface. It acts as a bridge between two software systems. A client application sends a request to an API server, the server processes the request, and then returns a response.

Most modern APIs use HTTP requests and return data in JSON format because JSON is lightweight, easy to read, and simple to process in JavaScript and Node.js.

The communication process usually follows this flow:

1. Client sends request
2. API server processes request
3. API server returns response
4. Client extracts and displays useful data

Most APIs use common HTTP methods such as:

* GET → retrieve data
* POST → create or send data
* PUT → update data
* DELETE → remove data

Example of a GET request:

```http id="ezk6dl"
GET /api/search
```

Most API responses are returned as JSON objects.

Example:

```json id="mx8mw7"
{
  "name": "Computer",
  "definition": "An electronic device used for processing data."
}
```

JSON data must usually be parsed inside Node.js applications before it can be displayed or manipulated.

---

### Public APIs, Authentication, and API Keys

APIs are generally divided into two categories: public APIs and protected APIs.

Public APIs are open for use and usually do not require authentication. These APIs are useful for learning and testing because developers can immediately send requests and receive responses. An example discussed in class is the Dictionary API.

Protected APIs require authentication before access is granted. Authentication is commonly handled using API keys, access tokens, or OAuth systems. These security methods help track usage, prevent abuse, and limit unauthorized access.

An API key is a unique identifier assigned to an application or developer. The key is sent along with the request headers to verify the identity of the client.

Example:

```http id="wwa8aj"
Authorization-Key: YOUR_API_KEY
```

Some APIs also require additional headers such as:

* User-Agent
* Host
* Content-Type

If the authentication information is missing or incorrect, the server may reject the request with an error response such as:

* 401 Unauthorized
* 403 Forbidden

API keys should always be stored securely because exposing them publicly may allow other users to abuse the service.

---

### Dictionary API and USA Jobs API

The Dictionary API is an example of a public API. It allows applications to retrieve word definitions, pronunciations, meanings, and synonyms. Since it is public, no authentication is required.

Example request:

```http id="2qv6b6"
GET https://api.dictionaryapi.dev/api/v2/entries/en/computer
```

The API returns a JSON response containing information about the searched word.

Example:

```json id="n9p2ud"
[
  {
    "word": "computer",
    "meanings": [
      {
        "partOfSpeech": "noun",
        "definitions": [
          {
            "definition": "An electronic device for processing data."
          }
        ]
      }
    ]
  }
]
```

Applications consuming the API must extract specific fields from the JSON response such as:

* Word
* Definition
* Part of speech

The application should also handle possible errors such as:

* Invalid search terms
* Network failure
* API downtime

The USA Jobs API is an example of an authenticated API that provides job listing data from the United States government employment database. Unlike the Dictionary API, this API requires authentication through API keys and headers.

Typical returned data includes:

* Job title
* Agency
* Salary
* Location
* Job description

Example request structure:

```http id="h9hjli"
GET /api/search?keyword=developer
```

Example authentication headers:

```http id="0x3o95"
Authorization-Key: YOUR_API_KEY
User-Agent: your_email@example.com
```

Without valid credentials, the API may deny access to the client.

---

### Asynchronous Programming and API Consumption in Node.js

API communication is naturally asynchronous because requests travel across the Internet and responses may take time to return. If synchronous programming were used, the entire application would stop and wait until the API response arrived. This would make applications slow and unresponsive.

Node.js solves this problem using asynchronous programming techniques. Instead of blocking execution, Node.js allows the program to continue running while waiting for the API response.

Common asynchronous techniques include:

* Callbacks
* Promises
* async/await

Modern Node.js applications commonly use async/await because the syntax is cleaner and easier to understand.

Example:

```js id="b0p45u"
const response = await fetch(url);
```

The process of consuming an API in Node.js generally follows these steps:

1. User enters input
2. Application sends HTTP request
3. API processes request
4. JSON response is returned
5. Application parses JSON data
6. Data is displayed to the user

Asynchronous programming is important because:

* It improves performance
* Prevents application freezing
* Allows multiple operations simultaneously
* Supports scalable web applications

---

### API Mashups and Combining Multiple APIs

An API mashup is an application that combines data from multiple APIs into a single system. Mashups are common in modern web applications because they allow developers to integrate different services together and create richer user experiences.

The asynchronous API mashup discussed combines:

* Dictionary API
* USA Jobs API

The workflow may look like this:

1. User searches for a keyword such as “developer”
2. Dictionary API returns the definition of the word
3. USA Jobs API searches for jobs related to the keyword
4. The application combines and displays both results together

Example output:

* Definition of “developer”
* Related government job listings

Mashups provide several advantages:

* Dynamic applications
* Real-time information
* Richer user experience
* Integration of multiple services

However, consuming multiple APIs also introduces challenges such as:

* Authentication problems
* Network delays
* Invalid responses
* API rate limits
* Missing data
* Server downtime

Applications should therefore:

* Validate API responses
* Handle errors properly
* Secure API keys
* Avoid excessive requests
* Follow API documentation carefully

---

