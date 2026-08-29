# 01. How a Web Application Actually Works

## From button click to database and back again

A modern web application can look like one thing on the screen while actually involving several different systems.

A student may write:

```js
fetch("/api/users")
```

and see data appear. That single line can hide:

```text
browser
JavaScript runtime
HTTP
network stack
server
router
middleware
business logic
database driver
database
JSON serialization
HTTP response
browser event loop
state update
render
```

This guide builds one mental model that connects those pieces.

---

# The Big Picture

```text
USER
 ↓
BROWSER EVENT
 ↓
JAVASCRIPT HANDLER
 ↓
HTTP REQUEST
 ↓
SERVER
 ↓
ROUTE
 ↓
APPLICATION LOGIC
 ↓
DATABASE
 ↓
APPLICATION LOGIC
 ↓
HTTP RESPONSE
 ↓
BROWSER
 ↓
JAVASCRIPT
 ↓
STATE / DOM UPDATE
 ↓
SCREEN
```

The first important idea is:

> The browser and server are different programs.

They may both use JavaScript, but they do not share normal application memory.

---

# 1. The Browser Is One Runtime

Frontend JavaScript executes inside a browser environment.

The browser gives JavaScript capabilities such as:

```text
DOM
events
fetch
timers
localStorage
console
```

These are not all features of the JavaScript language itself. They are capabilities exposed by the environment.

For example:

```js
document.querySelector("#button");
```

requires the browser DOM.

---

# 2. The Server Is Another Runtime

A Node/Express server is a different running program.

```js
app.get("/api/users", (req, res) => {
  res.json({ name: "Ada" });
});
```

This code runs on the server, not in the browser.

The browser communicates with it through requests.

---

# 3. HTTP Is the Message Exchange

A request carries information such as:

```text
method
URL
headers
optional body
```

Example:

```text
GET /api/users/42
```

A response carries:

```text
status
headers
body
```

Example:

```text
200 OK

{
  "id": 42,
  "name": "Ada"
}
```

The browser is not directly calling an Express function. It sends a message that causes the server to decide which code should run.

---

# 4. Routes Match Requests to Code

```js
app.get("/api/users/:id", getUser);
```

Read this as:

```text
If a GET request matches this path,
run getUser.
```

A route maps an external request to internal program behavior.

---

# 5. Middleware Is a Pipeline

A request may travel through:

```text
request
  ↓
logger
  ↓
authentication
  ↓
JSON parser
  ↓
validation
  ↓
route handler
```

Middleware can inspect, modify, reject, annotate, or pass the request onward.

---

# 6. Application Logic Decides What Should Happen

Projects may use different names:

```text
route
controller
service
repository
model
```

The important idea is separation of responsibility.

Example:

```text
route:
Which request is this?

controller:
What request data do I need?

service:
What business rule should happen?

database layer:
How do I read or write persistent data?
```

---

# 7. The Database Is Another System

Conceptually:

```text
Node application
     ↓ query
database
     ↓ result
Node application
```

The server asks for data. The database returns results. The server decides what the client should receive.

---

# 8. Data Crosses Boundaries by Representation

If the server has:

```js
const user = {
  id: 42,
  name: "Ada"
};
```

and sends:

```js
res.json(user);
```

the browser does not receive the server's object reference.

Conceptually:

```text
server object
   ↓ serialize
JSON representation
   ↓ network
browser
   ↓ parse
new browser-side value
```

Different runtimes, different memory.

---

# 9. `fetch()` Starts Work That Finishes Later

```js
const response = await fetch("/api/users/42");
const user = await response.json();
```

The network operation takes time. The browser performs networking work and JavaScript later receives the result.

That is one reason asynchronous programming exists.

---

# 10. State Changes Drive the UI

Vanilla JavaScript may do:

```js
element.textContent = user.name;
```

React may do:

```js
setUser(user);
```

The mental chain is:

```text
response arrives
     ↓
JavaScript receives data
     ↓
state changes
     ↓
rendering logic runs
     ↓
screen changes
```

---

# Trace One Full Request

Suppose the user clicks Save:

```js
await fetch("/api/profile", {
  method: "PUT",
  headers: {
    "Content-Type": "application/json"
  },
  body: JSON.stringify(profile)
});
```

Trace:

```text
1. click event occurs
2. JavaScript handler runs
3. profile is serialized
4. browser creates HTTP request
5. request reaches server
6. Express receives it
7. middleware parses JSON
8. PUT route matches
9. server validates input
10. database is updated
11. database reports result
12. server creates response
13. response reaches browser
14. fetch promise settles
15. frontend continues
16. state/UI may update
```

---

# Debugging by Layer

Instead of:

```text
"The app is broken."
```

ask:

```text
Did the click handler run?
Did fetch run?
What request was sent?
Did the server receive it?
Did the route match?
Did validation fail?
Did the database query run?
What status came back?
Did the frontend parse it?
Did state update?
```

That turns one giant problem into a sequence of observable boundaries.

---

# Common Beginner Confusions

## "My frontend variable should exist on the backend"

No. Different runtime, different memory.

## "My React component calls my Express route"

Not directly. They communicate through requests and responses.

## "JSON is the database"

No. JSON is a representation format.

## "CORS means my API is authenticated"

No. CORS is a browser-origin policy, not identity verification.

---

# Final Mental Model

Draw:

```text
BROWSER
  |
  | HTTP
  v
SERVER
  |
  | query
  v
DATABASE
```

Then trace one value across every boundary.

Ask:

```text
Where was it created?
How did it cross?
What representation did it have?
Who owns it now?
```
