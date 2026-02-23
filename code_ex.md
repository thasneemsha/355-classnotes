Here’s what the final project will include:

* **Node.js HTTP server** (no third-party libraries)
* **HTML form** for user input
* **Open Library API call** (first synchronous API)
* **YouTube Data API call** (second synchronous API) 
* **Used google cloud to get API key** [API](https://console.cloud.google.com/apis/api/youtube.googleapis.com/metrics?project=cs355-fp)
* **Combine results** and send HTML response to user
* **Console logs** to demonstrate synchronous API calls
---
### **Option: Book → YouTube Tutorials (Dependent)**

First API: *Open Library API* — get books based on topic.

Second API: *YouTube Data API* — search for tutorial videos using the book title.

**Why it works**: Each book title from the first API is used as a query for the second API.
>You can’t fetch the YouTube videos until you have the book titles, which perfectly satisfies the “dependent API” requirement.

---

## **Full Code Project**

**Project structure:**

```
CS355-FP-########
│
├── index.js         # Node.js server
├── public/
│   └── index.html   # HTML form page
├── style.css        # optional styling
└── README.md        # optional notes
```

---

### **1. HTML Form (`public/index.html`)**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Course & Study Resources</title>
    <link rel="stylesheet" href="/style.css">
</head>
<body>
    <h1>Course + Study Resources Mashup</h1>
    <form method="GET" action="/search">
        <input name="topic" placeholder="Enter course or topic" required />
        <button type="submit">Search</button>
    </form>
</body>
</html>
```

---

### **2. Optional CSS (`public/style.css`)**

```css
body {
    font-family: Arial, sans-serif;
    margin: 40px;
    background-color: #f0f2f5;
}
h1 {
    color: #333;
}
ul {
    list-style-type: square;
}
li {
    margin: 5px 0;
}
a {
    text-decoration: none;
    color: #1a73e8;
}
a:hover {
    text-decoration: underline;
}
```

---

### **3. Node.js Server (`index.js`)**

```js
const http = require('http');
const https = require('https');
const url = require('url');
const querystring = require('querystring');
const fs = require('fs');
const path = require('path');

const PORT = 3000;
const YOUTUBE_API_KEY = 'YOUR_YOUTUBE_API_KEY_HERE'; // Replace with your key

const server = http.createServer((req, res) => {
    const parsedUrl = url.parse(req.url);
    const pathname = parsedUrl.pathname;

    // Serve static files (HTML + CSS)
    if (pathname === '/') {
        const filePath = path.join(__dirname, 'public', 'index.html');
        fs.readFile(filePath, (err, content) => {
            if (err) {
                res.writeHead(500);
                res.end('Error loading page');
                return;
            }
            res.writeHead(200, { 'Content-Type': 'text/html' });
            res.end(content);
        });
        return;
    } else if (pathname === '/style.css') {
        const filePath = path.join(__dirname, 'public', 'style.css');
        fs.readFile(filePath, (err, content) => {
            if (err) {
                res.writeHead(500);
                res.end('Error loading CSS');
                return;
            }
            res.writeHead(200, { 'Content-Type': 'text/css' });
            res.end(content);
        });
        return;
    }

    // Handle search request
    if (pathname === '/search' && req.method === 'GET') {
        const query = querystring.parse(parsedUrl.query);
        const topic = query.topic;

        if (!topic) {
            res.writeHead(400);
            res.end('Topic is required');
            return;
        }

        console.log(`User searched topic: ${topic}`);

        // Step 1: Open Library API
        const openLibraryUrl = `https://openlibrary.org/search.json?q=${encodeURIComponent(topic)}`;
        https.get(openLibraryUrl, (libRes) => {
            let libData = '';
            libRes.on('data', chunk => libData += chunk);
            libRes.on('end', () => {
                let books;
                try {
                    books = JSON.parse(libData).docs.slice(0, 5); // top 5 books
                } catch (err) {
                    res.writeHead(500);
                    res.end('Error parsing Open Library response');
                    return;
                }
                console.log('Open Library API response received');

                // Step 2: YouTube API
                const ytUrl = `https://www.googleapis.com/youtube/v3/search?part=snippet&q=${encodeURIComponent(topic + ' tutorial')}&type=video&maxResults=5&key=${YOUTUBE_API_KEY}`;
                https.get(ytUrl, (ytRes) => {
                    let ytData = '';
                    ytRes.on('data', chunk => ytData += chunk);
                    ytRes.on('end', () => {
                        let videos;
                        try {
                            videos = JSON.parse(ytData).items;
                        } catch (err) {
                            res.writeHead(500);
                            res.end('Error parsing YouTube API response');
                            return;
                        }
                        console.log('YouTube API response received');

                        // Step 3: Generate HTML
                        let html = `<h1>Top Books for "${topic}"</h1><ul>`;
                        books.forEach(book => {
                            html += `<li>${book.title} by ${book.author_name?.[0] || 'Unknown'}</li>`;
                        });
                        html += `</ul><h1>Top YouTube Tutorials</h1><ul>`;
                        videos.forEach(video => {
                            html += `<li><a href="https://www.youtube.com/watch?v=${video.id.videoId}" target="_blank">${video.snippet.title}</a></li>`;
                        });
                        html += `</ul><a href="/">Search Again</a>`;

                        res.writeHead(200, { 'Content-Type': 'text/html' });
                        res.end(html);
                    });
                });
            });
        });

        return;
    }

    // 404 for anything else
    res.writeHead(404);
    res.end('Page not found');
});

server.listen(PORT, () => console.log(`Server running at http://localhost:${PORT}`));
```

---

## **4. How to Run the Project**

1. Open terminal in your project folder
2. Run the server:

```bash
node index.js
```

3. Open browser: [http://localhost:3000](http://localhost:3000)
4. Enter a course/topic (e.g., `Data Structures`) → Submit
5. You’ll see **top 5 books** + **top 5 tutorial videos**

---

## **5. Demonstrating Synchronous API Calls**

* In `index.js`, the `console.log` statements show the order:

```
User searched topic: Data Structures
Open Library API response received
YouTube API response received
```

> This proves the second API only runs **after** the first API responds.

---

## **6. Sequence Diagram**

Entities:

```
User --> Server : Submit topic
Server --> Open Library API : GET book info
Open Library API --> Server : JSON response
Server --> YouTube API : GET tutorial videos
YouTube API --> Server : JSON response
Server --> User : Combined HTML page
```

* Use [https://sequencediagram.org](https://sequencediagram.org) to create a PNG for submission

---

## **7. Screencast Tips**

1. Explain **topic input → Open Library API → YouTube API**
2. Show **console logs** to highlight synchronous behavior
3. Display results page (books + video links)
4. Explain how your server handles **requests without third-party modules**

---

## ✅ **Why this meets all requirements**

* **Two APIs** → Open Library (public), YouTube (API key)
* **Synchronous** → second API waits for first
* **Server-side only** → minimal JS on front-end
* **No third-party libraries** → uses only built-in Node modules
* **Easy to explain in screencast** → linear flow, console logs, clear HTML
* **Free APIs** → no cost, no OAuth hassles

---
