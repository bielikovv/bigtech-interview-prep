# API Gateway

## What It Is

An API gateway is a single entry point that sits between clients and your backend services, handling the cross-cutting concerns that every request needs, before the request actually reaches the service that does the real work.

## What It Actually Does

- **Routing:** directs each incoming request to the correct backend service (similar to Layer 7 load balancing, but often with more application-aware logic—like versioning or feature flags).
- **Authentication/authorization:** verifies who's making the request before it ever reaches a backend service, so individual services don't each need to reimplement this.
- **Rate limiting:** enforces per-client request limits at the edge, before wasted load even reaches your services. (See [Rate Limiting](rate-limiting.md).)
- **Request/response transformation:** can reshape a request or response—for example, aggregating calls to multiple backend services into a single response for the client, which matters a lot for mobile clients trying to minimize round trips.
- **Logging & monitoring:** a natural, centralized place to log every request that enters the system, since everything passes through it.
- **SSL termination:** decrypts HTTPS traffic once at the gateway so backend services can talk plain HTTP internally, simplifying certificate management.

## API Gateway vs. Load Balancer

These get confused because they sit in similar positions in the request path, but they solve different problems. A load balancer distributes load across identical instances of one service. An API gateway routes and shapes requests across many *different* services, and adds cross-cutting logic (auth, rate limiting, transformation) that has nothing to do with load distribution. In practice, a real request path often goes: DNS → load balancer → API gateway → the actual backend service (which might have its own load balancer in front of it too).

## Why Not Just Let Every Service Handle This Itself?

You could, but then every single service needs to reimplement authentication, rate limiting, and logging—and any inconsistency between services becomes a real security and reliability risk. Centralizing it in one gateway means you fix and audit this logic in exactly one place.

## The Tradeoff: A New Single Point of Failure and Bottleneck

Everything now passes through the gateway, so it needs to be highly available (usually run as multiple redundant instances behind a load balancer) and fast, since it adds a hop to every single request. It can also become a bottleneck if it does too much heavy processing per request.

## How to Bring This Up in an Interview

Worth mentioning explicitly when the system has multiple distinct services (microservices-style) or when auth/rate limiting is a real requirement—it's the natural place to say "this is where auth and rate limiting live" instead of hand-waving those concerns into each service individually.
