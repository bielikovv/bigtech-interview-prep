# Load Balancers

## What It Is

A load balancer sits in front of a group of servers and distributes incoming traffic across them, so no single server gets overwhelmed while others sit idle. It's usually the very first component traffic hits after DNS resolution, before anything else in your system.

## Why You Need One

Once a single server can't handle your traffic (or you need redundancy so one server dying doesn't take the whole system down), you run multiple identical instances of your service. Something has to decide, for each incoming request, which instance handles it—that's the load balancer's entire job.

## Layer 4 vs. Layer 7

- **Layer 4 (transport layer):** balances based on IP and port, without looking at the actual content of the request. Faster, since it doesn't need to parse anything, but less flexible—it can't route based on the URL path or headers.
- **Layer 7 (application layer):** understands HTTP, so it can route based on the URL path, headers, cookies, or request content (e.g., `/api/video/*` goes to the video service, `/api/users/*` goes to the user service). More flexible, slightly more overhead.

Most modern systems lean on Layer 7 load balancing because that path-based routing flexibility is usually worth the small overhead.

## Load Balancing Algorithms

- **Round robin:** cycle through servers in order. Simple, works fine when all servers and all requests are roughly equal.
- **Weighted round robin:** same idea, but some servers get more traffic than others (useful if servers have different capacities).
- **Least connections:** send the request to whichever server currently has the fewest active connections. Better than round robin when request processing times vary a lot.
- **Consistent hashing:** route based on a hash of some request property (like a user ID), so the same client consistently lands on the same server. Useful for session affinity or when a server holds client-specific cached state. (See [Consistent Hashing](consistent-hashing.md).)

## Health Checks

A load balancer needs to know which servers are actually healthy, or it'll keep routing traffic to a dead instance. It does this by periodically pinging each server (a health check endpoint) and pulling any server that fails to respond out of rotation until it recovers.

## High Availability of the Load Balancer Itself

The load balancer can't be a single point of failure either. In practice, this usually means running multiple load balancer instances behind a DNS entry or a floating IP, so if one load balancer goes down, traffic fails over to another.

## How to Bring This Up in an Interview

Load balancers are usually a quick mention early in the design ("traffic comes in, hits a Layer 7 load balancer, gets routed to the right service"), not something you need to dwell on—unless the interviewer specifically pushes on it (e.g., asking about the algorithm choice or what happens if the load balancer itself fails).
