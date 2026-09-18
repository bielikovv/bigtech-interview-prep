# Components Roadmap: The Order to Learn Them In

These aren't meant to be read in a random order. A few of these concepts (CAP theorem, consistent hashing) are theory that every other component quietly depends on, so they come first. After that, it roughly follows the order things actually get touched when you design a real system: traffic hits a load balancer, gets routed by a gateway, hits caches before it hits a database, and the database itself needs to handle replication and scale beyond one machine.

Go through these one by one, in this order:

1. [CAP Theorem & Consistency Models](cap-theorem.md) — the foundational tradeoff every distributed system is quietly making. Read this first; everything else references it.
2. [Consistent Hashing](consistent-hashing.md) — the trick that makes load balancers, caches, and sharded databases scale without falling over every time a node joins or leaves.
3. [Load Balancers](load-balancing.md) — the first thing incoming traffic hits.
4. [API Gateway](api-gateway.md) — what sits behind the load balancer to route, authenticate, and shape traffic into your actual services.
5. [Caching](caching.md) — how to avoid hitting your database for the same data over and over.
6. [CDN](cdn.md) — caching's geographically-distributed cousin, for static and semi-static content.
7. [Rate Limiting](rate-limiting.md) — how to protect a system from being overwhelmed, by its own users or by abuse.
8. [SQL vs. NoSQL Databases](databases-sql-vs-nosql.md) — the foundational storage choice, and what it actually hinges on.
9. [Database Replication](database-replication.md) — how a database survives beyond a single machine.
10. [Database Sharding](database-sharding.md) — how a database scales beyond a single machine's capacity.
11. [Message Queues & Pub/Sub](message-queues.md) — how systems talk to each other without being tightly coupled or blocking on each other.
12. [Blob/Object Storage](blob-storage.md) — where the big, unstructured stuff (videos, images, files) actually lives.

Each file is a first-pass explanation I wrote out myself while learning the topic—expect it to get corrected and refined as I review it.
