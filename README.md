
# WiFi Package Purchasing System

A simple full-stack web application that allows students to buy WiFi packages and admins to monitor purchases in real time.

It demonstrates practical use of **WebSockets** for live updates and two-way messaging.

## Features

### Student Side

- View available WiFi packages (1 Day, 7 Days, 30 Days)
- Purchase a package by entering name and student ID
- Instantly receive a unique voucher code after purchase
- See success message: **"You are logged in!"**
- Receive live messages sent by the admin

### Admin Side

- Live dashboard that updates automatically when a student buys a package
- See student name, student ID, package, price, time, and voucher code
- Send predefined messages to all connected students (e.g. "Voucher Ready", "Please wait 5 minutes", etc.)

### Technical Highlights

- Real-time communication using WebSockets
- Clear step-by-step WebSocket implementation (easy to teach)
- REST API for packages and purchases
- Dummy voucher generation on every purchase
- Clean, full-screen and responsive UI

## Tech Stack

| Layer     | Technology                    |
| --------- | ----------------------------- |
| Backend   | Node.js + Express             |
| Real-time | `ws` (WebSocket library)    |
| Frontend  | Vanilla HTML, CSS, JavaScript |
| Storage   | In-memory (resets on restart) |

## Project Structure

```text
wifi-packages/
├── server.js              # Main server (Express + WebSocket)
├── package.json
├── public/
│   ├── index.html         # Student purchase page
│   ├── admin.html         # Admin monitoring dashboard
│   └── style.css          # Shared styles
└── README.md
```

## Getting Started

### 1. Prerequisites

- [Node.js](https://nodejs.org/) (v16 or higher recommended)
- npm (comes with Node.js)

### 2. Installation

```bash
# Clone the repository (or download the folder)
cd Campus-wifi-

# Install dependencies
npm install
```

### 3. Run the Server

```bash
npm start
```

You should see:

```text
WebSocket server is ready
Server running at http://localhost:3000
Student page → http://localhost:3000
Admin page   → http://localhost:3000/admin.html
```

### 4. Open the Application

| Role    | URL                              |
| ------- | -------------------------------- |
| Student | http://localhost:3000            |
| Admin   | http://localhost:3000/admin.html |

> **Tip:** Open the student and admin pages in two browser windows side by side to watch the real-time updates happen.

## How to Use

### As a Student

1. Open the student page.
2. Choose a package and click **Buy Now**.
3. Enter your full name and student ID.
4. Click **Confirm Purchase**.
5. You will see the success screen with the message **"You are logged in!"** and your voucher code.
6. If the admin sends a message, it will pop up in the top-right corner of the page.

### As an Admin

1. Open the admin page.
2. The connection status will show **"Live – Connected to server"**.
3. When a student makes a purchase, a new row appears instantly in the table (with the voucher code).
4. Use the buttons under **"Send Message to Students"** in the sidebar to broadcast messages.

## WebSocket Implementation (Teaching Notes)

The WebSocket logic is clearly separated and numbered inside `server.js` for easy explanation:

```javascript
// Step 1: Import the WebSocket library
const WebSocket = require('ws');

// Step 2: Create the HTTP server
const server = http.createServer(app);

// Step 3: Create the WebSocket server
const wss = new WebSocket.Server({ server });

// Step 4: Handle new connections
wss.on('connection', function connection(ws) { ... });

// Step 5: Handle incoming messages
ws.on('message', function incoming(rawMessage) { ... });

// Step 6: Handle disconnection
ws.on('close', function () { ... });

// Step 7: Error handling
ws.on('error', function (error) { ... });
```

A helper function `broadcast()` is used to send data to all connected clients (both for new purchases and admin messages).

## API Endpoints

| Method | Endpoint           | Description                        |
| ------ | ------------------ | ---------------------------------- |
| GET    | `/api/packages`  | Returns list of available packages |
| POST   | `/api/purchase`  | Creates a new purchase + voucher   |
| GET    | `/api/purchases` | Returns all purchases (for admin)  |

## Voucher Generation

Every successful purchase generates a random voucher in the format:

```text
WIFI-XXXXXX
```

Example: `WIFI-K7P2X9`

The voucher is:

- Shown to the student on the success screen
- Displayed in the admin table in real time

## Notes & Limitations

- Data is stored in memory. Restarting the server clears all purchases.
- No authentication is implemented (admin page is open).
- No real payment gateway — this is a demonstration project.
- Designed for teaching WebSockets and basic full-stack concepts.

## Possible Future Improvements

- [ ] Persist data with SQLite or MongoDB
- [ ] Add simple admin login
- [ ] Generate unique vouchers with expiry
- [ ] Send private messages to a specific student
- [ ] Add package management (create/edit/delete) for admin
- [ ] Deploy to a cloud platform (Render, Railway, etc.)

## License

MIT
