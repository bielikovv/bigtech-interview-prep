# Rate Limiting

## What It Is

A rate limiter caps how many requests a client (a user, an API key, an IP address) can make within a given time window. Anything beyond the limit gets rejected—usually with an HTTP 429 (Too Many Requests)—instead of being processed.

## Why You Need One

Without a limit, a single misbehaving client (a buggy retry loop, a scraper, or an outright malicious actor) can consume a disproportionate share of your system's capacity, degrading service for everyone else. It's also a basic defense against abuse and certain kinds of denial-of-service behavior—not a complete DDoS solution on its own, but a necessary layer.

## Common Algorithms

- **Fixed window:** count requests in a fixed time window (e.g., a clock-aligned minute); reset the count at the boundary. Simple, but has a burst problem at window edges—a client can send the full limit right at the end of one window and again right at the start of the next, doubling the effective rate briefly.
- **Sliding window:** instead of a fixed clock-aligned window, track requests over a rolling window that moves with the current time. Smooths out the fixed-window edge-burst problem, at the cost of a bit more bookkeeping.
- **Token bucket:** a bucket holds a set number of tokens; each request consumes one token, and tokens refill at a steady rate over time. Allows some burstiness (you can spend banked-up tokens all at once) while still enforcing a long-run average rate. This is the most commonly used approach in practice because it balances simplicity with flexibility.
- **Leaky bucket:** requests enter a queue (the "bucket") and get processed at a fixed, steady rate; if the queue overflows, new requests get dropped. Smooths bursts into a constant output rate, which token bucket doesn't guarantee.

## Where to Enforce It

- **At the API gateway / edge:** the most common place—reject over-limit requests before they consume any backend resources at all. (See [API Gateway](api-gateway.md).)
- **Per-service:** sometimes individual services need their own limits for specific expensive operations, on top of the edge-level limit.

## What Key Do You Rate-Limit On?

This matters more than the algorithm choice, in practice: limiting by IP address is easy but breaks down behind shared IPs (NAT, corporate networks) and is trivial to bypass by rotating IPs; limiting by user ID or API key is more precise but requires the request to already be authenticated by the time it's rate-limited.

## Distributed Rate Limiting

If you have multiple gateway/service instances, they need a shared view of each client's current count—usually backed by a fast shared store like Redis, since each instance can't just track counts in its own local memory without undercounting the true global rate.

## How to Bring This Up in an Interview

Worth mentioning as soon as the system is public-facing or has any notion of per-client fairness or abuse prevention. Naming token bucket specifically, and explaining briefly why it beats a naive fixed window, is usually enough to demonstrate real understanding without over-investing time here.
