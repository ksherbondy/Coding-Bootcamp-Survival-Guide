# How the Internet Works: A Beginner Guide for Web Developers

> **Purpose:** This guide fills in the missing foundation that many beginner coding modules skip.  
> It is written for students who are learning web development and need a plain-English explanation of what the Internet is, how websites load, and why terms like browser, server, DNS, IP address, HTTP, and API matter.

---

## Table of Contents

1. [Who This Guide Is For](#who-this-guide-is-for)
2. [The Big Idea](#the-big-idea)
3. [Internet vs. Web](#internet-vs-web)
4. [The Internet Is a Network of Networks](#the-internet-is-a-network-of-networks)
5. [Clients and Servers](#clients-and-servers)
6. [What Happens When You Visit a Website?](#what-happens-when-you-visit-a-website)
7. [DNS: Turning Names Into Addresses](#dns-turning-names-into-addresses)
8. [IP Addresses](#ip-addresses)
9. [URLs: Web Addresses Explained](#urls-web-addresses-explained)
10. [Protocols: The Rules Computers Follow](#protocols-the-rules-computers-follow)
11. [HTTP and HTTPS](#http-and-https)
12. [Ports](#ports)
13. [APIs and JSON](#apis-and-json)
14. [Frontend, Backend, and Full Stack](#frontend-backend-and-full-stack)
15. [Hosting and Deployment](#hosting-and-deployment)
16. [Localhost: Your Computer as a Server](#localhost-your-computer-as-a-server)
17. [Common Beginner Confusions](#common-beginner-confusions)
18. [Practice Questions](#practice-questions)
19. [Quick Reference](#quick-reference)
20. [Further Reading](#further-reading)

---

## Who This Guide Is For

This guide is for students who are learning web development and feel like the course material is moving too fast.

A lot of beginner coding courses introduce terms like:

- browser
- server
- client
- DNS
- IP address
- URL
- HTTP
- HTTPS
- API
- JSON
- localhost
- port
- deployment

But they do not always slow down and explain what those words actually mean.

That can make students feel lost, even when they are capable of learning the material.

This guide is meant to fill in those missing steps.

---

## The Big Idea

The Internet is not one giant computer.

The Internet is a worldwide system of connected networks. Your home Wi-Fi, school network, phone network, internet provider, websites, cloud servers, and data centers are all part of a much larger system.

These devices and networks can communicate because they follow shared rules.

Those shared rules are called **protocols**.

A simple way to think about it:

```text
The Internet = connected networks + shared communication rules
```

When you open a website, your computer is not magically pulling a page out of the air. Your browser is asking another computer for files, and that computer sends files back.

Those files may include:

- HTML
- CSS
- JavaScript
- images
- videos
- fonts
- JSON data

Your browser then reads those files and builds the page you see.

---

## Internet vs. Web

People often use **Internet** and **Web** as if they mean the same thing, but they are not exactly the same.

### The Internet

The **Internet** is the global network system.

It is the infrastructure that lets computers, phones, servers, routers, and networks communicate.

The Internet supports many services, including:

- websites
- email
- file transfers
- video calls
- online games
- messaging apps
- streaming services
- cloud storage

### The Web

The **World Wide Web**, usually called **the Web**, is one service that runs on the Internet.

The Web is what you use when you open a browser and visit websites.

Examples:

```text
Internet service: Web browsing
Tool used: Browser
Common protocol: HTTP or HTTPS
Example: Visiting https://example.com
```

```text
Internet service: Email
Tool used: Email client or webmail
Common protocols: SMTP, IMAP, POP3
Example: Sending a message through Gmail or Outlook
```

### Simple Summary

```text
The Internet is the road system.
The Web is one type of traffic that travels on those roads.
```

---

## The Internet Is a Network of Networks

A **network** is a group of devices that can communicate with each other.

A small network might include:

```text
Laptop
Phone
Printer
Router
Smart TV
```

All of those devices may connect to the same home Wi-Fi router.

That home network connects to an **Internet Service Provider**, usually called an **ISP**.

Examples of ISPs include companies that provide cable, fiber, wireless, or satellite Internet service.

The ISP connects your home network to larger networks.

Those larger networks connect to other networks.

That is why the Internet is often described as a **network of networks**.

### Basic Connection Path

```text
Your laptop
   ↓
Your Wi-Fi router
   ↓
Your ISP
   ↓
Larger Internet networks
   ↓
The website's server
```

The exact path can change depending on traffic, location, routing, outages, and network decisions. Your request does not always travel the same path every time.

---

## Clients and Servers

Most web development is built around the **client/server model**.

### Client

A **client** is the device or program asking for something.

Examples:

- a web browser
- a mobile app
- a desktop app
- a command-line tool
- a frontend React app

When you use Chrome, Firefox, Safari, or Edge to open a website, your browser is acting as the client.

### Server

A **server** is a computer or program that provides something.

Examples:

- a web server that sends HTML, CSS, and JavaScript
- an API server that sends JSON data
- a database server that stores records
- an authentication server that verifies logins

A server listens for requests and sends back responses.

### Simple Client/Server Diagram

```text
Client makes a request
        ↓
+----------------+
|    Browser     |
+----------------+
        |
        |  "Please send me the homepage."
        v
+----------------+
|     Server     |
+----------------+
        |
        |  "Here is the HTML, CSS, and JavaScript."
        v
+----------------+
|    Browser     |
+----------------+
Browser displays the page
```

### Restaurant Analogy

A client/server relationship is like ordering food at a restaurant.

```text
You = client
Waiter/kitchen = server
Menu item = requested resource
Meal = response
```

You ask for something. The restaurant prepares it and gives it back.

In web development, the browser asks for a resource. The server sends back a response.

---

## What Happens When You Visit a Website?

When you type a website address into your browser and press Enter, several steps happen very quickly.

Example:

```text
https://www.example.com
```

### Step 1: You Enter a URL

You type a web address into the browser.

The browser needs to figure out where that website lives.

### Step 2: DNS Looks Up the IP Address

Computers do not use names like `example.com` directly.

They need an IP address.

DNS helps translate the domain name into an IP address.

```text
example.com → 93.184.216.34
```

### Step 3: The Browser Connects to the Server

Once the browser knows the IP address, it can contact the server.

### Step 4: The Browser Sends an HTTP Request

The browser asks the server for a resource.

Example request idea:

```text
GET / HTTP/1.1
Host: www.example.com
```

This means:

```text
Please give me the homepage for this website.
```

### Step 5: The Server Sends an HTTP Response

The server responds with data.

That response might include:

- HTML
- CSS
- JavaScript
- images
- JSON
- status codes
- headers

### Step 6: The Browser Builds the Page

The browser reads the HTML, applies the CSS, runs the JavaScript, loads images, and displays the page.

### Full Flow

```text
You type a URL
   ↓
Browser asks DNS for the IP address
   ↓
DNS returns the IP address
   ↓
Browser connects to the server
   ↓
Browser sends an HTTP/HTTPS request
   ↓
Server sends a response
   ↓
Browser renders the page
```

---

## DNS: Turning Names Into Addresses

DNS stands for **Domain Name System**.

DNS is often compared to a phonebook, but a more modern way to think about it is:

```text
DNS is the contact list for the Internet.
```

Humans prefer names:

```text
google.com
github.com
creatingcodingcareers.org
```

Computers need addresses:

```text
142.250.190.14
140.82.112.4
```

DNS connects the human-friendly name to the computer-friendly address.

### Why DNS Matters for Developers

DNS becomes important when you deploy websites.

For example, if you buy a domain name like:

```text
myportfolio.com
```

You need DNS records that tell the Internet where that domain should point.

Common DNS records include:

| Record Type | Beginner Meaning |
|---|---|
| A record | Points a domain to an IPv4 address |
| AAAA record | Points a domain to an IPv6 address |
| CNAME record | Points one name to another name |
| MX record | Handles email routing |
| TXT record | Stores text information, often for verification/security |

You do not need to master all DNS records at the beginning, but you should understand this:

```text
DNS helps browsers find the server connected to a domain name.
```

---

## IP Addresses

An **IP address** is a number used to identify a device on a network.

IP stands for **Internet Protocol**.

A common IPv4 address looks like this:

```text
192.168.1.10
```

An IPv6 address looks longer:

```text
2001:0db8:85a3:0000:0000:8a2e:0370:7334
```

### IPv4

IPv4 uses 32-bit addresses.

Example:

```text
8.8.8.8
```

IPv4 addresses are still very common, but there are not enough unique IPv4 addresses for every modern device.

### IPv6

IPv6 uses 128-bit addresses.

IPv6 was created to provide a much larger address space.

### Public vs. Private IP Addresses

Your home devices often have private IP addresses inside your local network.

Examples:

```text
192.168.0.5
10.0.0.12
172.16.1.8
```

These are commonly used inside homes, schools, and businesses.

Your router usually has a public-facing connection through your ISP.

A simplified version:

```text
Your laptop: private IP inside your house
Your router: connects your home network to the Internet
Your ISP: connects you to the wider Internet
```

---

## URLs: Web Addresses Explained

URL stands for **Uniform Resource Locator**.

A URL is the address of a resource on the Web.

Example:

```text
https://www.example.com:443/products/shoes?id=25#reviews
```

That URL has several parts.

| Part | Example | Meaning |
|---|---|---|
| Protocol/Scheme | `https://` | The rules used to access the resource |
| Domain | `www.example.com` | The human-readable server name |
| Port | `:443` | The specific doorway/service on the server |
| Path | `/products/shoes` | The location of the resource |
| Query String | `?id=25` | Extra information sent with the request |
| Fragment | `#reviews` | A section of the page to jump to |

### Simpler Example

```text
https://github.com/ksherbondy
```

This tells the browser:

```text
Use HTTPS.
Go to github.com.
Ask for the /ksherbondy page.
```

### File Paths on Websites

A URL path can look like a folder path:

```text
https://example.com/blog/post-one.html
```

But modern web apps do not always map directly to real folders and files. Many frameworks use routing systems that make URLs look like file paths even when the page is generated dynamically.

---

## Protocols: The Rules Computers Follow

A **protocol** is a set of rules.

Computers need protocols so they know how to communicate.

Without shared rules, one computer might send information in a way the other computer cannot understand.

### Common Internet Protocols

| Protocol | Used For |
|---|---|
| HTTP | Loading web pages and web resources |
| HTTPS | Secure web browsing |
| TCP | Reliable data transport |
| IP | Addressing and routing packets |
| DNS | Translating domain names to IP addresses |
| SMTP | Sending email |
| IMAP | Reading email from a mail server |
| FTP/SFTP | Transferring files |
| SSH | Secure remote access to another computer |

### Why Protocols Matter

When your browser requests a page, both the browser and server need to agree on how that request and response should be structured.

That agreement is the protocol.

---

## HTTP and HTTPS

HTTP stands for **Hypertext Transfer Protocol**.

HTTPS stands for **Hypertext Transfer Protocol Secure**.

HTTP is the main protocol browsers and servers use to communicate on the Web.

HTTPS is the secure version of HTTP. It encrypts the connection so other people on the network cannot easily read or change the data being sent.

### HTTP Request

A request is what the client sends to the server.

Example:

```text
GET /about.html HTTP/1.1
Host: example.com
```

Meaning:

```text
Please send me the about.html page from example.com.
```

### HTTP Response

A response is what the server sends back.

Example:

```text
HTTP/1.1 200 OK
Content-Type: text/html
```

Meaning:

```text
The request worked. Here is an HTML document.
```

### Common HTTP Methods

| Method | Beginner Meaning |
|---|---|
| GET | Ask for data |
| POST | Send new data |
| PUT | Replace existing data |
| PATCH | Update part of existing data |
| DELETE | Delete data |

### Common HTTP Status Codes

| Status Code | Meaning |
|---|---|
| 200 | OK / success |
| 201 | Created |
| 301 | Moved permanently |
| 400 | Bad request |
| 401 | Unauthorized |
| 403 | Forbidden |
| 404 | Not found |
| 500 | Server error |

### Example

When you visit a page that does not exist, the server may return:

```text
404 Not Found
```

That means the browser successfully reached the server, but the server could not find the requested resource.

---

## Ports

A **port** is like a numbered doorway into a computer.

A server may provide multiple services. Ports help identify which service a request is trying to reach.

Common ports:

| Port | Common Use |
|---|---|
| 80 | HTTP |
| 443 | HTTPS |
| 22 | SSH |
| 3000 | Common local development server |
| 5173 | Common Vite development server |
| 8080 | Common alternate web server port |

When you run a local React or Node project, you may see something like:

```text
http://localhost:5173
```

That means:

```text
Open a web server running on this computer using port 5173.
```

---

## APIs and JSON

API stands for **Application Programming Interface**.

That sounds complicated, but the beginner version is:

```text
An API is a structured way for one program to ask another program for something.
```

In web development, APIs often allow a frontend app to request data from a backend server.

### Example

A weather app may ask a weather API:

```text
What is the weather in San Diego?
```

The API may respond with JSON:

```json
{
  "city": "San Diego",
  "temperature": 72,
  "condition": "Sunny"
}
```

### JSON

JSON stands for **JavaScript Object Notation**.

It is a common data format used on the Web.

JSON looks a lot like JavaScript objects:

```json
{
  "name": "Kris",
  "role": "student",
  "isLearning": true
}
```

### Why APIs Matter

Modern websites often do not load everything as one static page.

Instead, they may load the page first, then use JavaScript to fetch data from APIs.

Example flow:

```text
React app loads
   ↓
React app calls an API
   ↓
API returns JSON
   ↓
React updates the page with the data
```

---

## Frontend, Backend, and Full Stack

### Frontend

The **frontend** is the part of a website or app the user interacts with directly.

Frontend technologies include:

- HTML
- CSS
- JavaScript
- React
- Vue
- Angular

Frontend work includes:

- page layout
- buttons
- forms
- navigation
- styling
- user interaction
- fetching data from APIs

### Backend

The **backend** is the server-side part of an application.

Backend technologies may include:

- Node.js
- Express
- Python
- Java
- C#
- Go
- Rust
- databases
- authentication systems

Backend work includes:

- handling requests
- connecting to databases
- validating data
- managing users
- protecting sensitive information
- sending responses to the frontend

### Full Stack

A **full-stack developer** works with both frontend and backend code.

Simple full-stack flow:

```text
User clicks button
   ↓
Frontend sends request
   ↓
Backend receives request
   ↓
Backend talks to database
   ↓
Backend sends response
   ↓
Frontend updates screen
```

---

## Hosting and Deployment

When you build a website on your own computer, only you can see it unless you make it available somewhere else.

**Deployment** means putting your app somewhere other people can access it.

**Hosting** means the service or server that makes your app available online.

Common hosting/deployment platforms include:

- GitHub Pages
- Netlify
- Vercel
- Render
- Railway
- AWS
- Azure
- Google Cloud

### Static Website

A static website usually consists of files like:

```text
index.html
style.css
script.js
images/
```

These files can be hosted on services like GitHub Pages, Netlify, or Vercel.

### Dynamic Website

A dynamic website may include:

- a backend server
- a database
- authentication
- APIs
- server-side logic

Dynamic apps usually need more than basic static hosting.

---

## Localhost: Your Computer as a Server

When learning web development, you will often run projects locally.

You may see addresses like:

```text
http://localhost:3000
http://localhost:5173
http://127.0.0.1:3000
```

`localhost` means:

```text
this computer
```

When you run a development server, your own computer temporarily acts like a server so your browser can load the project.

Example:

```text
npm run dev
```

Then the terminal may show:

```text
Local: http://localhost:5173/
```

That means your project is running on your machine and can be opened in your browser.

### Localhost vs. Live Website

| Localhost | Live Website |
|---|---|
| Runs on your computer | Runs on a public server |
| Usually only visible to you | Visible to other people |
| Used during development | Used after deployment |
| Example: `localhost:5173` | Example: `https://myapp.com` |

---

## Common Beginner Confusions

### Confusion 1: “Is the Internet the same as Wi-Fi?”

No.

Wi-Fi is a wireless way to connect your device to a local network.

The Internet is the larger global system of connected networks.

You can have Wi-Fi without Internet if your router is working but your ISP connection is down.

### Confusion 2: “Is Google the Internet?”

No.

Google is a company and search engine that helps you find things on the Web.

The Internet is the global network system.

### Confusion 3: “Is a browser the same as a search engine?”

No.

A browser is an application used to access websites.

Examples:

- Chrome
- Firefox
- Safari
- Edge

A search engine helps you search for websites.

Examples:

- Google
- Bing
- DuckDuckGo

You can use a search engine inside a browser.

### Confusion 4: “What is the difference between a website and a web app?”

A basic website mostly displays information.

A web app is more interactive and behaves more like software.

Examples of web apps:

- Gmail
- Google Docs
- Trello
- GitHub
- Canva

The line between website and web app can be blurry.

### Confusion 5: “Why does my React app use localhost?”

Because during development, your computer runs a temporary local server.

Your browser connects to that local server to view the project.

### Confusion 6: “Why does an API return JSON instead of a web page?”

Because APIs are usually designed for programs, not humans.

A browser page is meant to be read by people.

JSON is meant to be read by code.

---

## Practice Questions

Use these questions to check your understanding.

### Beginner Questions

1. What is the difference between the Internet and the Web?
2. What is a client?
3. What is a server?
4. What does DNS do?
5. Why do computers need IP addresses?
6. What is a URL?
7. What is HTTP used for?
8. Why is HTTPS important?
9. What does `localhost` mean?
10. What is JSON commonly used for?

### Applied Questions

1. When you visit a website, what steps happen before the page appears?
2. Why does a frontend app often need to call an API?
3. What is the difference between a static website and a dynamic website?
4. Why might a developer see `404 Not Found`?
5. Why does a deployed website need hosting?

### Challenge Prompt

Explain the following flow in your own words:

```text
Browser → DNS → IP address → Server → HTTP response → Rendered page
```

---

## Quick Reference

| Term | Beginner-Friendly Meaning |
|---|---|
| Internet | A global network of connected networks |
| Web | Websites and web apps accessed through browsers |
| Browser | App used to visit websites |
| Client | Device or program asking for something |
| Server | Computer or program that responds to requests |
| DNS | System that translates domain names into IP addresses |
| IP Address | Numeric address for a device on a network |
| URL | Address of a web resource |
| HTTP | Protocol used for web requests and responses |
| HTTPS | Secure version of HTTP |
| API | Way for programs to request data or services |
| JSON | Common format for sending structured data |
| Frontend | User-facing part of a website or app |
| Backend | Server-side logic and data handling |
| Deployment | Putting an app online |
| Hosting | Service/server that makes an app available |
| Localhost | Your own computer acting as a server |
| Port | Numbered doorway for a network service |

---

## Further Reading

These resources are beginner-friendly places to continue learning.

### MDN Web Docs

MDN is one of the best references for web technologies.

Suggested topics to search on MDN:

- How the Web works
- HTTP
- DNS
- URLs
- Client-side web APIs
- JavaScript basics

### freeCodeCamp

freeCodeCamp has beginner-friendly lessons and articles on web development.

Suggested topics:

- HTML
- CSS
- JavaScript
- APIs
- frontend development
- backend development

### W3Schools

W3Schools is useful for quick examples and beginner syntax references.

Suggested topics:

- HTML tutorial
- CSS tutorial
- JavaScript tutorial
- HTTP methods
- JSON

### The Odin Project

The Odin Project is a full web development curriculum with strong foundations.

Suggested topics:

- Internet basics
- Git basics
- HTML and CSS
- JavaScript
- Node.js

---

## Final Mental Model

When you are learning web development, keep this picture in your head:

```text
You use a browser.
The browser is the client.
The client asks a server for something.
DNS helps find the server.
HTTP/HTTPS defines how the request and response work.
The server sends files or data back.
The browser turns those files and data into the page you see.
```

That one flow connects almost everything you will learn later:

- HTML
- CSS
- JavaScript
- React
- APIs
- Node
- Express
- databases
- deployment
- debugging
- security

Once this foundation makes sense, the rest of web development has somewhere to attach.
