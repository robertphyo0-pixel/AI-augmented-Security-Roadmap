## Lesson 1: The Journey of a Web Request

> **A web application is a conversation between systems.**

Every request is a conversation.

Every vulnerability interrupts that conversation.

Every defense protects part of that conversation.

Every debugging session follows that conversation.

Every penetration test observes and manipulates that conversation.

## Architecture Discussion

When you enter:

```
https://example.com/login
```

your browser starts a chain of events.

```
You

↓

Browser

↓

DNS

↓

TCP Connection

↓

TLS Handshake

↓

HTTP Request

↓

Internet

↓

Load Balancer (sometimes)

↓

Reverse Proxy

↓

Web Server

↓

Python Application

↓

Framework (Flask/FastAPI)

↓

Business Logic

↓

Database

↓

Business Logic

↓

Framework

↓

Web Server

↓

Reverse Proxy

↓

HTTP Response

↓

Browser Rendering

↓

You
```

This diagram is the backbone of modern web security.

Every security topic fits somewhere along this path.

For example:

- DNS spoofing affects the DNS stage.
- TLS misconfiguration affects the encrypted connection.
- SQL Injection affects the application-to-database interaction.
- Cross-Site Scripting affects how the browser renders the response.
- Authentication flaws occur in business logic.
- Authorization flaws arise where access decisions are made.
## Key Concepts 
- Requests travel through multiple layers. 
- Python runs on the server. 
- Every trust boundary requires validation. 
- Architecture precedes security analysis. 

## Security Mindset 
- Before examining a vulnerability, understand: 
- System architecture 
- Data flow 
- Trust boundaries 
- Assets 
- Assumptions 

## Connections 
- Python: Server-side logic 
- Linux: Hosts web servers and applications 
- Networking: Transports requests 
- DevSecOps: Automates secure deployment 
- Cloud: Hosts scalable web architectures