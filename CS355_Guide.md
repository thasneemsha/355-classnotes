# CS355 - Guide

### Overview

* Students will **choose two APIs** and create a mashup that uses them **synchronously**.
* Once the code is completed, students will present their project in a **short recorded screencast**, walking through their code and demoing the final result.
* **All components are required for a project grade**. Students will have at least 2 weeks to work on the project.

**Resources & Links:**

* [Sequence Diagram Tool](https://sequencediagram.org/)
* [Chat](https://chatgpt.com/c/697806f2-a880-8327-83fe-4a9d43804e11)
* [Gemini](https://gemini.google.com/app/f0c257a540b6f72c)
* [to make MD file](https://dillinger.io/)
* [Syllabus](https://raymondlaw.github.io/cs355/misc/syllabus/CSCI_355___FA2025___Internet_and_Web_Technologies__MW_.pdf)

**Learning Resources:**

* Learn JavaScript: [Video Tutorial](https://www.youtube.com/watch?v=lfmg-EJ8gm4)
* Assignment Solutions:
  * [Assignment 1](https://www.youtube.com/watch?v=hjxurNOD-k4)
  * [Assignment 2](https://www.youtube.com/watch?v=xGg0RTG-FKg)
  * [Assignment 3](https://www.youtube.com/watch?v=fcUQrvxTcks)
  * [Assignment 4](https://www.youtube.com/watch?v=-kvvPiKbxvs)
* Final Project Details: [Final Project Page](https://raymondlaw.github.io/cs355/assessment/06-final-project/final-project.html)

---

## Project Description

* Students must choose **two APIs** and create a mashup that uses them synchronously.
* Students must complete the **HTTP and Third Party API topics** and **Assessment 4** to understand HTTP/HTTPS usage in Node.js.
* Example projects from previous semesters:

  * **Nasa API + Google Drive**: Upload a new photo from Space to a user’s Google Drive.
  * **OpenDota API + Wunderlist**: Create calendar events for league matches.
  * **OpenWeatherMap + OpenBreweryDB**: Display weather and brewery info by zip code.

---

## Important Dates

* **Phase 1 (API Choices Submission)**: 2025/12/20 23:59:00
  Submit API choices via **Microsoft Form**.

  * Must use a **CUNY Microsoft account (@login.cuny.edu)**.
  * If the link doesn’t work, try an **incognito window**.

* **Phase 2 (Project Submission)**: 2025/12/22 23:59:00 via **BrightSpace**

---

## Deliverables

> **No partial credit** if any of the following are missing:

1. **Project Code**

   * Must satisfy all restrictions and guidelines.

2. **HTTP Sequence Diagram**

   * Must show all HTTP requests/responses between client and server.
   * **Do not include TCP handshakes/ACKs**.

3. **Screencast**

   * Walkthrough of project and code.
   * **Mandatory**; students submitting code without a recording will receive a zero.

---

## Code Requirements

A successful project should follow **four phases of execution**:

1. End user visits the home page → server sends a form.
2. User submits form → server sends **first API request**.
3. Server parses first response → sends **second API request**.

   * Must be **server-driven** (not user-triggered).
4. Server parses second response → sends final results to user.

**Additional Requirements:**

* Two APIs must be **queried synchronously** without race conditions.
* At least one API must have **authentication**.
* Score ceilings depend on authentication type:

| Authentication               | Maximum Score |
| ---------------------------- | ------------- |
| Public API                   | 0%            |
| API Key                      | 90%           |
| OAuth 2.0 Client Credential  | 100%          |
| OAuth 2.0 Authorization Code | 110%          |

**API List:** [Public APIs](https://github.com/marcelscruz/public-apis)

**Restrictions:**

* No **third-party modules** (e.g., npm, GitHub, Express).

  * Only **Node.js built-in libraries** allowed (fs, http, https, querystring, url).
* No **Promise, Fetch, or Async/Await**; use **callback/observer pattern**.
* Avoid **APIs used in demos**:

  * [Dictionary API](https://dictionaryapi.dev/)
  * [USAJobs API](https://developer.usajobs.gov/)
  * [Todoist API](https://developer.todoist.com/)
  * [Spotify API](https://developer.spotify.com/)
* **No setTimeout** except for testing API timeouts.

---

## Project Phases

### Phase 1: Choosing APIs

* Check the **class API pairing list**: [Read-only document](https://cuny907-my.sharepoint.com/:x:/g/personal/raymond_law43_login_cuny_edu/IQB-Lsh8GjpeS6Ipp2aPGUyVAcpJenZu2wDHIIYsdXiK5hY?rtime=41e9nuUw3kg)
* Each pairing must be **unique**.
* Steps:

  * Set up accounts for chosen APIs.
  * Test authentication & endpoints.
  * Draw draft **sequence diagram**.
  * Use **HTTP debugger (Bruno)** for requests.
  * Submit API choices via **Microsoft Form**.

---

### Phase 2: Creating the Project

1. Create a **formal HTTP sequence diagram**

   * [Sequence Diagram Tool](https://sequencediagram.org/)
   * Include 4 entities: **User, Server, API1, API2**
2. Build project according to diagram.
3. Ensure **resilience to unexpected input** (return 404 when needed).
4. Test **synchronous operation** using a delayed `.end()` call for API requests.

**Testing Synchronous Calls Example:**

```js
const api_request_a = https.request(url, options, callback);
setTimeout(() => api_request_a.end(...), 5000);
```

* Add print statements:
  `API 1 has been called`, `API 2 has been called`
* Adjust delay to confirm **API 2 waits for API 1**.

Reference: [N Domains Synchronously](https://raymondlaw.github.io/cs355/lectures/03-asynchronous-programming/asynchronous-programming.html#n-domains-synchronously-recursion)

---

### Phase 3: Screencast Presentation

**Include:**

* Project description: APIs, endpoints, input/output, synchronous order.
* Walkthrough of **sequence diagram**.
* Walkthrough of **code execution**:

  * Landing page form
  * Capturing user input
  * Sending first API request
  * Capturing first response
  * Sending second API request
  * Returning final response
* Demonstrate **resilience** and **multiple request handling**
* Address these questions:

  * How do you ensure synchronous API requests?
  * What is cached? Duration? Risks of stale cache?

**Screencast Length:** Ideal 15–20 minutes (no strict minimum).
**Tools:** [OBS](https://obsproject.com/) or YouTube Live.

---

## Submission

* **Directory structure:** `CS355-FP-########` (replace # with Student ID)
* **Submit 3 items via BrightSpace**:

  1. Screencast (file or YouTube link)
  2. Project code (no `node_modules` or third-party libs)
  3. Sequence diagram (export PNG)
* **Delete cache files** before submission.
* **Due Date:** 2025/12/22 23:59:00

---

## **Goal of the Project**

You need to **build a Node.js web application** that:

1. **Uses two APIs** (one must have authentication).

2. **Combines their data** in a “mashup.”

3. **Works synchronously**, meaning:

   * The second API request must wait for the first API response.
   * The user does **not** manually trigger the second request.

4. You must **record a screencast** showing how your project works.

---

## **What You Will Actually Do**

Think of it as **4 main steps in your code**:

1. **User visits your site**

   * Show a form (HTML page).
   * Example: “Enter your city to see weather and local breweries.”

2. **User submits the form**

   * Your server gets the data.
   * Send the **first API request** using the input.
   * Example: Send city name to OpenWeatherMap API.

3. **Server receives first API response**

   * Parse the data.
   * Send the **second API request** using the first API’s response.
   * Example: Use weather info to search nearby breweries using OpenBreweryDB.

4. **Server receives second API response**

   * Parse it.
   * Send **final combined results** back to the user in HTML or JSON.
   * Example: Show breweries + weather info on a page.

---

## **Extra Requirements**

* You **cannot use third-party libraries** like Express or axios.

  * Only built-in Node.js libraries like `http`, `https`, `fs`, `querystring`, `url`.

* **No Promises, Fetch, or Async/Await**.

  * You must use **callbacks** (or observer pattern) to handle asynchronous requests.

* **Sequence diagram**

  * Show every HTTP request/response between the **User, Server, and two APIs**.
  * Abstract away TCP details (don’t show handshakes).

* **Screencast**

  * Walk through your project: explain APIs, endpoints, request order, and show your code running.
  * Ideal length: 15–20 minutes.

* **Scoring depends on authentication**

  * Public API only → 0%
  * API Key → 90%
  * OAuth 2.0 Client Credential → 100%
  * OAuth 2.0 Authorization Code → 110%

The main point is: **your server should handle everything automatically** in the right order, no manual clicks for the second API request.


