# Web Socket

#### 🧠 Problem

Traditional HTTP communication is **request–response based**, meaning:

* The client must **ask** for updates each time (polling or long polling).
* This leads to **high latency** and **inefficient bandwidth usage** for real-time data.
* Applications like chats, live dashboards, or multiplayer games need **instant bidirectional communication**, which HTTP can’t provide efficiently.

***

#### 🧭 Approach

To solve this, the **WebSocket protocol** (RFC 6455) was introduced.

It enables a **persistent, full-duplex connection** between the client and server over a single TCP socket:

* Once established, **both client and server can send data anytime**.
* Connection upgrades from HTTP → WebSocket using a handshake (`Upgrade: websocket`).
* Works with `ws://` or `wss://` (secure) schemes.

This eliminates the overhead of repeated HTTP requests, allowing **real-time data flow** with minimal latency.

***

#### 💡 Solution

Use WebSocket for scenarios requiring **instant data exchange** and **state synchronization**:

* Chat systems
* Live notifications or dashboards
* Collaborative editing tools
* Gaming or streaming platforms

**Example (Node.js + Browser):**

```js
// Server (Node.js)
import { WebSocketServer } from "ws";
const wss = new WebSocketServer({ port: 8080 });

wss.on("connection", ws => {
  console.log("Client connected");
  ws.send("Welcome!");
  
  ws.on("message", msg => {
    console.log("Received:", msg);
    ws.send(`You said: ${msg}`);
  });
});
```

```js
// Client (Browser)
const socket = new WebSocket("ws://localhost:8080");

socket.onopen = () => socket.send("Hello Server!");
socket.onmessage = e => console.log("Server:", e.data);
```

**Key benefits:**

* Persistent connection → lower latency
* Bidirectional communication
* Lightweight compared to HTTP polling

**Limitations:**

* Requires an active connection (won’t work when tab is closed or offline).
* Harder to scale horizontally (requires sticky sessions or shared state).
