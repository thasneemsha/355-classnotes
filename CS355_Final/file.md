

### 1. Daytime Server (`datetimeServer.js`)

This server listens on port `3013`, captures a connection, returns the current date and time string to the client, and gracefully tears down the connection via a four-way handshake.

```javascript
const net = require('net');

const PORT = 3013;
const server = net.createServer();

server.on('connection', newConnection);

function newConnection(socket) {
    console.log(`New client connected from port: ${socket.remotePort}`);
    
    const dateTime = new Date().toString();
    
    socket.write(dateTime + '\n', () => {
         socket.end();
    });
}

server.on('error', serverError);

function serverError(err) {
    throw err;
}

server.on('listening', serverListening);

function serverListening() {
    console.log(`Server bound and listening on port ${PORT}`);
}

server.listen(PORT);

```

### 2. Daytime Client (`datetimeClient.js`)

This client initiates a connection to the daytime server, prints out the formatted binary chunk received as a string, and listens for the server's close signals.

```javascript
const net = require('net');
const SERVER_ADDRESS = '127.0.0.1';
const PORT = 3013;

const client = net.createConnection({ port: PORT, host: SERVER_ADDRESS });

client.on('data', connectionData);
function connectionData(data) {
    console.log('Data received from server: ' + data.toString());
}

client.on('end', connectionEnd);
function connectionEnd() {
    console.log('Client disconnected from server.');
}

client.on('connect', connectionStart);
function connectionStart() {
    console.log('Connected to the server successfully.');
}

```

### 3. TCP Echo Server (`echoServer.js`)

Listens on port `3015`. Any subsequent text written by the client stream is piped back immediately to its own source connection loop.

```javascript
const net = require('net');
const PORT = 3015;
const server = net.createServer();

server.on('connection', newConnection);
function newConnection(socket) {
    console.log(`New client connected from port: ${socket.remotePort}`);
    socket.pipe(socket);
    socket.on('end', () => {
        console.log(`Client at port ${socket.remotePort} disconnected.`);
    });
}

server.on('error', serverError);
function serverError(err) {
    throw err;
}

server.on('listening', serverListening);
function serverListening() {
    console.log(`Echo Server bound and listening on port ${PORT}`);
}

server.listen(PORT);

```

### 4. Chat Server (`chatServer.js`)

Tracks an array of multi-client sockets simultaneously. When it captures payload strings from a given instance, it dynamically broadcasts that context to all other verified client arrays except for the source transmitter.

```javascript
const net = require('net');
const PORT = 3013;
const connections = [];
const server = net.createServer();

server.on('connection', newConnection);
function newConnection(socket) {
socket.on('data', coon
socket.on('end', ()
socket.on('error', ()

    console.log(`New client connected from port: ${socket.remotePort}`);
    connections.push(socket);

    socket.on('data', (data) => {
        console.log('Received data: ' + data.toString().trim());
        
        connections
            .filter(s => s !== socket)
            .forEach(s => s.write(data));
    });

    socket.on('end', () => {
        terminate(socket);
    });

    socket.on('error', () => {
        terminate(socket);
    });
}

function terminate(socket) {
    console.log(`Client from port ${socket.remotePort} disconnected.`);
    const index = connections.indexOf(socket);
    if (index !== -1) {
        connections.splice(index, 1);
    }
    socket.end();
}

function serverError(err) {
    throw err;
}

function serverListening() {
    console.log(`Chat Server bound and listening on port ${PORT}`);
}


server.on('error', serverError);
server.on('listening', serverListening);

server.listen(PORT);

```

### 6. Chat Client (`chatClient.js`)

Integrates the `readline` system module streams to query console terminal interface keys. It prompts for a handle name, handles user text dynamically, cleans the line on receiving incoming messages using layout coordinates, and logs out securely if `/exit` is typed.

```javascript
const net = require('net');
const readline = require('readline');

const SERVER_ADDRESS = '127.0.0.1';
const PORT = 3013;
let nickname = '';

const io = readline.createInterface({
    input: process.stdin,
    output: process.stdout
});

const socket = net.createConnection({ port: PORT, host: SERVER_ADDRESS });

function chat() {
    io.question(`[${nickname}]: `, (message) => {
        if (message.trim() === '/exit') {
            socket.end();
            process.exit(0);
        } else {
            socket.write(`[${nickname}]: ${message}\n`);
            chat();
        }
    });
}

socket.on('connect', () => {
    console.log('Connected to the Chat Server.');
    io.question('Choose a nickname: ', (answer) => {
        nickname = answer.trim();
        chat();
    });
});

socket.on('data', (data) => {
    readline.clearLine(process.stdout, 0);
    readline.cursorTo(process.stdout, 0);
    
    process.stdout.write(data.toString());
    process.stdout.write(`[${nickname}]: `);
});

socket.on('end', () => {
    console.log('\nDisconnected from chat server.');
    process.exit(0);
});

```
