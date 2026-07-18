## Core Mental Model

Networks are systems that allow independent computers to communicate.
Applications create data.
The operating system prepares it.
Network devices forward it.
Protocols define the rules.

## Troubleshooting Methodology

Observe
→ Gather evidence
→ Identify the failing layer
→ Test assumptions
→ Verify
→ Document

## Key Idea

Networking is the study of communication between systems, not the memorization of protocols or commands.

## Lesson 1 Reinforcement

- Networking is communication between systems.
- Troubleshooting begins by asking where communication stopped.
- IP addresses identify the destination network.
- MAC addresses identify the destination device on the local network.
- The loopback interface (`lo`) lets a computer communicate with itself through the full networking stack.
- `127.0.0.1` is "this computer," not "the network."

# Engineering Thinking

This introduces one of the most important principles in networking:

> **The operating system owns the network stack.**

Your applications don't decide where packets go.

Your Python code doesn't.

Your browser doesn't.

The operating system does.

Every application simply says:

> "I want to send data to this destination."

The operating system then decides:

- Should this stay local?
- Should it use the loopback interface?
- Should it go out through `eth0`?
- Should it be sent to the default gateway?
- Should it be dropped?

This is why networking knowledge is valuable even when you're writing Python. A bug may not be in your code at all—it could be in routing, addressing, or firewall rules.

## Mental Model

Application
         │
        ▼
Operating System
         │
        ▼
TCP/IP Stack
         │
        ▼
Network Interface
         │
        ▼
Local Network
         │
        ▼
Gateway
         │
        ▼
Routers
         │
        ▼
Destination

# Security Perspective

Imagine you receive an alert:

> "An employee connected to a malicious IP."

A junior analyst might ask:

> "Which IP?"

An experienced security engineer also asks:

- Which interface sent the traffic?
- Was it local or external?
- Which gateway forwarded it?
- Which firewall allowed it?
- Was it encrypted?
- Can we reconstruct the packet path?

The difference is that the senior engineer thinks in terms of the communication journey, not just the endpoint.

## Note

- The operating system decides how packets are delivered.
- Loopback (`127.0.0.1`) never leaves the computer.
- Private IP addresses (like `192.168.x.x`) identify devices on a local network.
- Every communication has a source, a destination, and a path.
- Troubleshooting starts by understanding that path.