
# **CS355 Final Project: Course + Study Resources Mashup**

**Goal:** Build a Node.js server that lets a student input a course or topic, fetches books from Open Library, then fetches tutorial videos from YouTube, and displays them on one page. Everything is **synchronous** and fully explained in a screencast.

---

## **Step 1: Set Up Your Development Environment**

### **1.1 IDE / Text Editor**

You can use any of these:

* **VS Code** (recommended) → free, popular, has Node.js support
* Sublime Text / Atom / WebStorm
* VS Code Extensions I recommend:

  * Node.js support (builtin)
  * Live Server (optional for testing static HTML)

**Download:** [VS Code](https://code.visualstudio.com/)

---

### **1.2 Install Node.js**

* Download the latest LTS version from: [Node.js](https://nodejs.org/en/)
* Verify installation:

```bash
node -v
npm -v
```

> ✅ Only Node.js built-in libraries will be used, no npm modules for API calls or server.

---

## **Step 2: Plan the Project Structure**

Create a folder for your project:

```
CS355-FP-########  (replace ###### with your student ID)
│
├── index.js         # main Node.js server file
├── public/
│   └── index.html   # HTML form page
├── style.css        # optional CSS
└── README.md        # optional project notes
```

* `index.js` → server code, handles form, API calls, sends results
* `public/index.html` → user input form
* `style.css` → make the output look nicer (optional)

---

## **Step 3: Get API Access**

### **3.1 Open Library API (Free, No Key)**

* Base URL: `https://openlibrary.org/search.json?q=<search_term>`
* Example: `https://openlibrary.org/search.json?q=data+structures`

### **3.2 YouTube Data API (Free, API Key)**

* Steps to get an API key:

  1. Go to [Google Cloud Console](https://console.cloud.google.com/)
  2. Create a new project
  3. Enable **YouTube Data API v3**
  4. Create **API key** → copy it for your code
* Example API call:

```http
https://www.googleapis.com/youtube/v3/search?part=snippet&q=data+structures+tutorial&type=video&key=YOUR_API_KEY
```

> ✅ You do **not** need OAuth 2.0 Authorization Code. API Key is enough.

---

## **Step 4: Code the Server (Node.js)**

### **4.1 Start a Basic HTTP Server**

```js
// index.js
const http = require('http');
const https = require('https');
const url = require('url');
const querystring = require('querystring');

const PORT = 3000;

const server = http.createServer((req, res) => {
    if (req.method === 'GET' && req.url === '/') {
        // Serve HTML form
        res.writeHead(200, {'Content-Type': 'text/html'});
        res.end(`<form method="GET" action="/search">
                    <input name="topic" placeholder="Enter course or topic" required/>
                    <button type="submit">Search</button>
                 </form>`);
    } else if (req.method === 'GET' && req.url.startsWith('/search')) {
        // Parse query
        const query = url.parse(req.url).query;
        const { topic } = querystring.parse(query);

        // Step 1: Call Open Library API
        const libUrl = `https://openlibrary.org/search.json?q=${encodeURIComponent(topic)}`;
        https.get(libUrl, (libRes) => {
            let data = '';
            libRes.on('data', chunk => data += chunk);
            libRes.on('end', () => {
                const books = JSON.parse(data).docs.slice(0,5); // top 5 books

                // Step 2: Call YouTube API
                const ytUrl = `https://www.googleapis.com/youtube/v3/search?part=snippet&q=${encodeURIComponent(topic+' tutorial')}&type=video&maxResults=5&key=YOUR_API_KEY`;

                https.get(ytUrl, (ytRes) => {
                    let ytData = '';
                    ytRes.on('data', chunk => ytData += chunk);
                    ytRes.on('end', () => {
                        const videos = JSON.parse(ytData).items;

                        // Step 3: Send combined HTML to user
                        let html = `<h1>Top Books for ${topic}</h1><ul>`;
                        books.forEach(book => {
                            html += `<li>${book.title} by ${book.author_name?.[0] || 'Unknown'}</li>`;
                        });
                        html += `</ul><h1>YouTube Tutorials</h1><ul>`;
                        videos.forEach(video => {
                            html += `<li><a href="https://www.youtube.com/watch?v=${video.id.videoId}" target="_blank">${video.snippet.title}</a></li>`;
                        });
                        html += `</ul>`;
                        res.writeHead(200, {'Content-Type': 'text/html'});
                        res.end(html);
                    });
                });
            });
        });
    } else {
        res.writeHead(404);
        res.end('Page not found');
    }
});

server.listen(PORT, () => console.log(`Server running on http://localhost:${PORT}`));
```

* This handles **both API calls in order** → synchronous
* Uses only **built-in Node libraries** (`http`, `https`, `url`, `querystring`)

---

## **Step 5: Test the Project**

1. Run server:

```bash
node index.js
```

2. Open browser → [http://localhost:3000](http://localhost:3000)
3. Type a course or topic → submit
4. Server fetches **Open Library books → YouTube tutorials** → displays results

---

## **Step 6: Sequence Diagram**

* Use [https://sequencediagram.org/](https://sequencediagram.org/)
* Entities:

  * User → Server → Open Library API → Server → YouTube API → Server → User
* Show **order of requests/responses** → synchronous workflow

---

## **Step 7: Screencast Recording**

* Record screen while:

  1. Explaining the **HTML form**
  2. Submitting a topic → server calls Open Library → shows top books
  3. Server calls YouTube → shows tutorial links
  4. Highlight **synchronous API calls** (print logs optional)

* Recommended tools:

  * [OBS Studio](https://obsproject.com/) → free
  * YouTube Live (optional)

* Length: 15–20 minutes ideal

* Show code + running app clearly

---

## **Step 8: Submission**

* Create directory: `CS355-FP-########` (replace with your Student ID)

* Include:

  1. `index.js` (server code)
  2. `public/index.html` (form)
  3. Optional `style.css`
  4. **Sequence diagram PNG**
  5. Screencast (file or YouTube link)

* Zip the folder → submit on BrightSpace **before 2025/12/22 23:59**

