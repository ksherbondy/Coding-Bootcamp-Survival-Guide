# Node, Express, Routing, and Middleware: A Beginner Mental Model

> **Purpose:** This guide explains how a basic Node/Express server works, with a special focus on routing and middleware.  
> It is written for beginners who are learning backend JavaScript and need a clear mental picture of what `app.use()`, `app.get()`, `req`, `res`, routes, and middleware actually mean.

---

## Table of Contents

1. [Who This Guide Is For](#who-this-guide-is-for)
2. [The Big Idea](#the-big-idea)
3. [Old Websites: The Missing Mental Model](#old-websites-the-missing-mental-model)
4. [Modern Routing: Paths Do Not Always Mean Files](#modern-routing-paths-do-not-always-mean-files)
5. [What Is Node.js?](#what-is-nodejs)
6. [What Is Express?](#what-is-express)
7. [A Tiny Express Server](#a-tiny-express-server)
8. [What Is a Route?](#what-is-a-route)
9. [Breaking Down `app.get()`](#breaking-down-appget)
10. [Request and Response](#request-and-response)
11. [Sending Text, HTML, and JSON](#sending-text-html-and-json)
12. [What Is Middleware?](#what-is-middleware)
13. [Middleware Runs in Order](#middleware-runs-in-order)
14. [Middleware Should Go Before the Routes It Affects](#middleware-should-go-before-the-routes-it-affects)
15. [Common Middleware Examples](#common-middleware-examples)
16. [Morgan Example: Logging Requests](#morgan-example-logging-requests)
17. [Static Files vs. API Routes](#static-files-vs-api-routes)
18. [404 Routes](#404-routes)
19. [Full Beginner Example](#full-beginner-example)
20. [Common Beginner Confusions](#common-beginner-confusions)
21. [Practice Lab](#practice-lab)
22. [Quick Reference](#quick-reference)
23. [Final Mental Model](#final-mental-model)

---

## Who This Guide Is For

This guide is for students who are learning Node and Express for the first time.

You may have seen code like this:

```js
app.use(morgan("dev"));

app.get("/", (req, res) => {
  res.status(200).send("It's working");
});

app.get("/data", (req, res) => {
  res.status(200).json(data);
});
```

At first, this can look strange.

What is `app`?

What does `use` mean?

What is `morgan`?

What is a route?

What are `req` and `res`?

Why does `/` mean the home page?

Why does `/data` send JSON?

This guide slows all of that down.

---

## The Big Idea

An Express server listens for requests and sends responses.

A beginner-friendly version:

```text
Browser or frontend app makes a request
   ↓
Express server receives the request
   ↓
Middleware may run first
   ↓
A matching route handles the request
   ↓
Server sends a response
```

Routes answer this question:

```text
What should happen when someone visits this path?
```

Middleware answers this question:

```text
What should happen to the request before it reaches the final route?
```

That is the core idea.

---

## Old Websites: The Missing Mental Model

Many beginners get confused by routing because they do not know how older websites were commonly built.

A simple old-school website might have looked like this:

```text
my-website/
├── index.html
├── about.html
├── calendar.html
├── contact.html
└── style.css
```

Each page was often a real file.

| User Visits | Server Sends |
|---|---|
| `/` | `index.html` |
| `/index.html` | `index.html` |
| `/about.html` | `about.html` |
| `/calendar.html` | `calendar.html` |
| `/contact.html` | `contact.html` |

In that older mental model:

```text
URL path often pointed directly to an HTML file.
```

So if someone visited:

```text
example.com/about.html
```

The server could send back the file:

```text
about.html
```

That is a helpful starting point.

---

## Modern Routing: Paths Do Not Always Mean Files

Modern web servers and frameworks do not always map URLs directly to files.

Instead, the server can define behavior for a path.

For example:

```text
/        → send homepage
/about   → send about page
/data    → send JSON
/login   → check login form
/users   → return user data
```

In Express, you can define those paths yourself.

```js
app.get("/", (req, res) => {
  res.send("Home page");
});

app.get("/about", (req, res) => {
  res.send("About page");
});

app.get("/data", (req, res) => {
  res.json({ message: "Here is some data" });
});
```

Notice this important idea:

```text
/about does not have to be a physical about.html file.
```

It can be a route.

A route is server logic connected to a path.

---

## What Is Node.js?

Node.js lets JavaScript run outside the browser.

Before Node, JavaScript was mostly known as a browser language.

It made web pages interactive.

Node allows JavaScript to be used for things like:

- backend servers
- command-line tools
- file processing
- APIs
- automation scripts
- development tools

Simple version:

```text
Browser JavaScript runs in the browser.
Node.js JavaScript runs on your computer or server.
```

Node is what allows us to build a server with JavaScript.

---

## What Is Express?

Express is a small web framework for Node.js.

Node can create servers by itself, but Express makes server code easier to write and read.

Express helps with:

- routes
- middleware
- request handling
- response handling
- APIs
- serving static files
- organizing backend logic

Simple version:

```text
Node.js lets JavaScript run as a server.
Express makes building that server easier.
```

---

## A Tiny Express Server

```js
import express from "express";

const app = express();
const PORT = 3000;

app.get("/", (req, res) => {
  res.status(200).send("It's working");
});

app.listen(PORT, () => {
  console.log(`Server is running on http://localhost:${PORT}`);
});
```

### What This Does

```text
Import Express.
Create an Express app.
Define a route for GET /.
Start listening on port 3000.
```

When someone visits:

```text
http://localhost:3000/
```

The server responds:

```text
It's working
```

---

## What Is a Route?

A route connects an HTTP method and a path to a function.

```js
app.get("/", (req, res) => {
  res.status(200).send("It's working");
});
```

This route says:

```text
When a GET request comes to /,
run this function.
```

A route usually has three main pieces:

```text
HTTP method + path + handler function
```

### HTTP Method

The method describes the type of action.

| Method | Beginner Meaning |
|---|---|
| GET | Ask for data |
| POST | Send new data |
| PUT | Replace data |
| PATCH | Update part of data |
| DELETE | Delete data |

### Path

The path describes the URL location.

Examples:

```text
/
/data
/users
/api/products
/login
```

### Handler Function

The handler function decides what response to send.

```js
(req, res) => {
  res.send("Hello");
}
```

---

## Breaking Down `app.get()`

Look at this route:

```js
app.get("/", (req, res) => {
  res.status(200).send("It's working");
});
```

| Part | Meaning |
|---|---|
| `app` | The Express application |
| `.get()` | Handle a GET request |
| `"/"` | The root/home path |
| `(req, res) => {}` | Function that runs when the route matches |
| `req` | Request object |
| `res` | Response object |
| `res.status(200)` | Set HTTP status to success |
| `.send("It's working")` | Send text back to the client |

Beginner translation:

```text
If someone makes a GET request to the home route,
send back a successful response that says "It's working."
```

---

## Request and Response

Most Express route handlers use `req` and `res`.

```js
app.get("/data", (req, res) => {
  res.status(200).json(data);
});
```

### `req`

`req` means request.

It represents what came into the server.

It may include:

- URL path
- HTTP method
- headers
- query parameters
- route parameters
- request body
- cookies
- authentication information

### `res`

`res` means response.

It represents what the server sends back.

You use it to send:

- text
- HTML
- JSON
- status codes
- files
- redirects

Simple mental model:

```text
req = what the client sent to the server
res = what the server sends back to the client
```

---

## Sending Text, HTML, and JSON

Express can send different kinds of responses.

### Sending Text

```js
app.get("/", (req, res) => {
  res.status(200).send("It's working");
});
```

### Sending HTML

```js
app.get("/about", (req, res) => {
  res.status(200).send("<h1>About Page</h1><p>This is the about page.</p>");
});
```

### Sending JSON

```js
const data = {
  name: "Sample App",
  status: "online"
};

app.get("/data", (req, res) => {
  res.status(200).json(data);
});
```

### Why JSON Matters

Frontend apps often need data, not full HTML pages.

```text
React app requests /data
   ↓
Express sends JSON
   ↓
React uses that data to update the screen
```

---

## What Is Middleware?

Middleware is code that runs during the request/response process.

It can run before a route sends the final response.

```js
app.use(morgan("dev"));
```

This registers Morgan as middleware.

Morgan logs incoming requests.

A beginner-friendly way to say it:

```text
Middleware is something the request passes through before reaching the final route handler.
```

Another mental model:

```text
Middleware is a checkpoint in the request pipeline.
```

### Request Pipeline

```text
Request comes in
   ↓
Middleware
   ↓
More middleware
   ↓
Route handler
   ↓
Response goes out
```

Middleware can:

- log requests
- parse JSON
- check authentication
- validate input
- sanitize input
- handle CORS
- serve static files
- block bad requests
- attach useful information to `req`

---

## Middleware Runs in Order

Express checks middleware and routes in the order they are registered.

This matters.

```js
app.use(morgan("dev"));

app.get("/", (req, res) => {
  res.send("Home");
});
```

Request flow:

```text
GET /
   ↓
morgan logs the request
   ↓
/ route sends "Home"
```

The file is read top to bottom when the server starts, and Express registers handlers in that order.

Then, when a request comes in, Express moves through the registered middleware/routes in order.

---

## Middleware Should Go Before the Routes It Affects

A strong beginner rule:

```text
Put middleware before the routes you want it to affect.
```

Global middleware often goes near the top:

```js
app.use(morgan("dev"));
app.use(express.json());
```

Then routes come after:

```js
app.get("/", homeHandler);
app.get("/data", dataHandler);
```

### Not All Middleware Has To Affect Every Route

Middleware can be placed before only the routes that need it.

```js
app.get("/", (req, res) => {
  res.send("Public home page");
});

app.use(requireLogin);

app.get("/dashboard", (req, res) => {
  res.send("Private dashboard");
});
```

In this example:

```text
/ does not require login
/dashboard does require login
```

Because `requireLogin` was registered after `/` but before `/dashboard`.

### Route-Specific Middleware

Middleware can also be attached directly to one route.

```js
app.get("/dashboard", requireLogin, (req, res) => {
  res.send("Private dashboard");
});
```

That means:

```text
For GET /dashboard,
run requireLogin first,
then run the route handler.
```

---

## Common Middleware Examples

### Logging

Logs request information.

```js
app.use(morgan("dev"));
```

Useful for seeing requests in the terminal.

### JSON Parsing

Allows Express to read JSON request bodies.

```js
app.use(express.json());
```

Useful for POST/PUT/PATCH requests where the client sends JSON.

### Static Files

Serves files from a folder.

```js
app.use(express.static("public"));
```

Useful for serving HTML, CSS, JavaScript, and images.

### CORS

Allows or restricts requests from other origins.

```js
app.use(cors());
```

Often used when a frontend app and backend API run on different ports or domains.

### Authentication

Checks whether the user is logged in or has a valid token.

```js
app.use(authMiddleware);
```

### Validation and Sanitation

Validation checks whether incoming data has the expected shape.

Sanitation cleans or normalizes incoming input.

Example use cases:

- trim whitespace
- remove unwanted characters
- prevent unsafe input
- normalize email casing
- check required fields

---

## Morgan Example: Logging Requests

Morgan is a request logger middleware.

```js
import express from "express";
import morgan from "morgan";

const app = express();

app.use(morgan("dev"));

app.get("/", (req, res) => {
  res.status(200).send("It's working");
});
```

When someone visits `/`, Morgan may print request information in the terminal.

Example idea:

```text
GET / 200 4.123 ms - 12
```

This helps developers see:

```text
What route was hit?
What method was used?
What status code was returned?
How long did it take?
```

Beginner translation:

```text
Morgan is like a request activity log for your server.
```

---

## Static Files vs. API Routes

Express can serve static files and API routes.

### Static File Example

Imagine this folder:

```text
public/
├── index.html
├── about.html
├── style.css
└── script.js
```

You can serve it with:

```js
app.use(express.static("public"));
```

Then a browser can request files from that folder.

```text
/             → public/index.html
/about.html   → public/about.html
/style.css    → public/style.css
/script.js    → public/script.js
```

This is closer to the older website model.

### API Route Example

```js
app.get("/data", (req, res) => {
  res.json({
    message: "This is data from the server"
  });
});
```

This does not send an HTML file.

It sends JSON.

### Difference

| Type | What It Sends |
|---|---|
| Static file route | Existing files like HTML, CSS, JS, images |
| API route | Data or custom responses generated by server code |

Modern projects often use both.

```text
Express serves frontend files from public/
Express also provides API data at /api/items
```

---

## 404 Routes

A 404 means:

```text
The requested route or resource was not found.
```

In Express, you can add a catch-all handler near the bottom.

```js
app.use((req, res) => {
  res.status(404).send("404 - Page not found");
});
```

Why near the bottom?

Because Express checks routes in order.

You want the 404 handler to run only after Express has checked the routes above it.

```text
Request comes in
   ↓
Check /
   ↓
Check /data
   ↓
Check /api/users
   ↓
No match?
   ↓
Send 404
```

If you put the 404 handler too early, it may catch requests before they reach valid routes.

---

## Full Beginner Example

```js
import express from "express";
import morgan from "morgan";

const app = express();
const PORT = 3000;

const data = [
  { id: 1, name: "HTML" },
  { id: 2, name: "CSS" },
  { id: 3, name: "JavaScript" }
];

// Middleware
app.use(morgan("dev"));
app.use(express.json());
app.use(express.static("public"));

// Routes
app.get("/", (req, res) => {
  res.status(200).send("It's working");
});

app.get("/data", (req, res) => {
  res.status(200).json(data);
});

app.get("/about", (req, res) => {
  res.status(200).send("<h1>About</h1><p>This is a beginner Express app.</p>");
});

// 404 handler
app.use((req, res) => {
  res.status(404).send("404 - Route not found");
});

// Start server
app.listen(PORT, () => {
  console.log(`Server running on http://localhost:${PORT}`);
});
```

### Request Flow

If the user visits:

```text
http://localhost:3000/data
```

The flow is:

```text
Request: GET /data
   ↓
Morgan logs the request
   ↓
express.json checks for JSON body
   ↓
express.static checks public folder
   ↓
app.get("/") does not match
   ↓
app.get("/data") matches
   ↓
Server sends JSON response
```

---

## Common Beginner Confusions

### Confusion 1: “Is `/` always `index.html`?”

Not always.

Historically, `/` often served `index.html`.

In Express, `/` means the root path, and you decide what response it sends.

It could send text, HTML, JSON, a file, a redirect, an error, or anything your server logic allows.

### Confusion 2: “Does `/data` mean there is a `data.html` file?”

No.

In Express:

```js
app.get("/data", handler);
```

means:

```text
When someone sends a GET request to /data, run this handler.
```

There does not have to be a physical file named `data`.

### Confusion 3: “Does middleware always go at the top?”

Not always.

Middleware should go before the routes it needs to affect.

Global middleware usually goes near the top.

Route-specific middleware can go directly inside a route.

### Confusion 4: “Why does order matter?”

Because Express checks middleware and routes in the order they were registered.

If something sends a response early, later handlers may not run.

### Confusion 5: “What is `req`?”

`req` is the incoming request.

It contains information from the client.

### Confusion 6: “What is `res`?”

`res` is the outgoing response.

You use it to send something back.

### Confusion 7: “Why does my browser show JSON?”

Because the route sent JSON.

```js
res.json(data);
```

That is expected for API routes.

### Confusion 8: “Why is my route not working?”

Common causes:

- server is not running
- wrong port
- wrong path
- wrong HTTP method
- middleware order issue
- route is below a catch-all handler
- typo in the URL
- route file was not imported
- response was already sent earlier

### Confusion 9: “Why do I need `express.json()`?”

If a client sends JSON in the request body, Express needs middleware to parse it.

```js
app.use(express.json());
```

Without it, `req.body` may be undefined.

### Confusion 10: “Why is my terminal showing GET /data 200?”

That is probably Morgan logging the request.

It is showing that someone requested `/data` and the server responded successfully.

---

## Practice Lab

This lab builds a small Express server.

### Step 1: Create a Folder

```bash
mkdir express-routing-practice
cd express-routing-practice
```

### Step 2: Initialize npm

```bash
npm init -y
```

### Step 3: Install Express and Morgan

```bash
npm install express morgan
```

### Step 4: Add Module Type and Start Script

Open `package.json` and add:

```json
{
  "type": "module",
  "scripts": {
    "start": "node server.js"
  }
}
```

Your full `package.json` will include more fields than this.

### Step 5: Create `server.js`

```bash
touch server.js
```

### Step 6: Add Server Code

```js
import express from "express";
import morgan from "morgan";

const app = express();
const PORT = 3000;

const data = [
  { id: 1, topic: "Routes" },
  { id: 2, topic: "Middleware" },
  { id: 3, topic: "JSON" }
];

app.use(morgan("dev"));
app.use(express.json());

app.get("/", (req, res) => {
  res.status(200).send("It's working");
});

app.get("/data", (req, res) => {
  res.status(200).json(data);
});

app.use((req, res) => {
  res.status(404).send("404 - Route not found");
});

app.listen(PORT, () => {
  console.log(`Server running on http://localhost:${PORT}`);
});
```

### Step 7: Run the Server

```bash
npm start
```

### Step 8: Test in the Browser

Open:

```text
http://localhost:3000/
```

You should see:

```text
It's working
```

Open:

```text
http://localhost:3000/data
```

You should see JSON.

Open:

```text
http://localhost:3000/not-real
```

You should see the 404 message.

### Step 9: Watch the Terminal

Each request should be logged by Morgan.

That proves the middleware is running before the route response finishes.

---

## Quick Reference

| Concept | Beginner Meaning |
|---|---|
| Node.js | Lets JavaScript run outside the browser |
| Express | Framework for building Node servers |
| Server | Program that listens for requests and sends responses |
| Route | Path plus method plus handler |
| Middleware | Function that runs during the request/response process |
| `app.use()` | Register middleware or shared behavior |
| `app.get()` | Handle GET requests |
| `req` | Incoming request |
| `res` | Outgoing response |
| `res.send()` | Send text/HTML/other response |
| `res.json()` | Send JSON response |
| `res.status()` | Set HTTP status code |
| `express.json()` | Parse JSON request bodies |
| `express.static()` | Serve static files from a folder |
| Morgan | Middleware that logs requests |
| 404 handler | Runs when no route matched |

---

## Common Code Patterns

### Basic Route

```js
app.get("/", (req, res) => {
  res.send("Home");
});
```

### JSON Route

```js
app.get("/data", (req, res) => {
  res.json(data);
});
```

### Middleware

```js
app.use(morgan("dev"));
```

### JSON Body Parser

```js
app.use(express.json());
```

### Static Files

```js
app.use(express.static("public"));
```

### 404 Handler

```js
app.use((req, res) => {
  res.status(404).send("404 - Not found");
});
```

### Route-Specific Middleware

```js
app.get("/dashboard", requireLogin, (req, res) => {
  res.send("Dashboard");
});
```

---

## Final Mental Model

Start with the old website picture:

```text
/              → index.html
/about.html    → about.html
/contact.html  → contact.html
```

Then upgrade the model:

```text
/        → route handler sends home response
/about   → route handler sends about response
/data    → route handler sends JSON
/api     → route handler sends API data
```

Express lets the server decide what happens when a path is requested.

Middleware is what the request passes through before it reaches the route that sends the final response.

The cleanest beginner model:

```text
Request comes in
   ↓
Middleware registered before the route runs first
   ↓
Express checks for a matching route
   ↓
Route handler uses req to understand the request
   ↓
Route handler uses res to send the response
```

And the key rule:

```text
Put middleware before the routes you want it to affect.
```

Once that picture clicks, Express routing stops feeling random.

It becomes a controlled request pipeline.
