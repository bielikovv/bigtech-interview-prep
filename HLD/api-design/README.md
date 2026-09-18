# API Design (REST vs. gRPC vs. GraphQL)

## What It Is

Once you've decided what your services need to do, you still need to decide how clients and services actually talk to each other over the network—what the request looks like, what the response looks like, and what contract binds the two sides together. That's API design. It's a separate decision from the [API Gateway](../components/api-gateway.md) (which is *where* that traffic gets intercepted and shaped) and from [Message Queues](../components/message-queues.md) (which is for async, non-request/response communication). This is specifically about the synchronous, "client asks, server answers" contract.

## REST

REST models everything as operations on **resources**, identified by URLs, using standard HTTP methods to say what you want to do to that resource:

- `GET /users/123` — read a user
- `POST /users` — create a user
- `PUT /users/123` — replace a user
- `PATCH /users/123` — partially update a user
- `DELETE /users/123` — delete a user

The appeal is that it maps directly onto HTTP, which every client, browser, and piece of infrastructure already understands natively—caching, status codes (200, 404, 500), and tooling all just work. It's also just easy to reason about and explain: nouns are resources, verbs are HTTP methods, done.

**Downsides:** it can lead to over-fetching (the endpoint returns a full user object when the client only needed the name) or under-fetching (the client needs data from three different resources, so it has to make three separate round trips). For a mobile client on a slow connection, that's a real cost.

## gRPC

gRPC is a way for services to call functions on each other directly, as if they were local—"call this remote procedure with these arguments, get this response back"—instead of thinking in terms of resources and HTTP verbs. It's built on HTTP/2 and uses Protocol Buffers (a compact binary format) instead of JSON, which makes it significantly faster and smaller over the wire than REST/JSON.

**Why it matters:** this is the default choice for internal service-to-service communication in a microservices architecture, where you control both ends of the connection and performance actually matters at scale. It also generates strongly-typed client/server code from a shared `.proto` schema, which catches mismatches at compile time instead of at runtime.

**Downsides:** it's not natively supported in browsers (needs a proxy layer like grpc-web), it's less human-readable for debugging (binary, not JSON you can eyeball), and it's overkill for a simple public-facing API where REST's simplicity and ubiquity win.

## GraphQL

GraphQL flips the model: instead of the server deciding what shape each endpoint returns, the client sends a query describing exactly what fields it wants, across potentially multiple resources, and gets back exactly that—nothing more, nothing less. This directly solves REST's over-fetching/under-fetching problem, since one GraphQL query can pull in a user, their posts, and their followers in a single round trip, with only the fields actually needed.

**Why it matters:** it shines when you have many different clients (web, iOS, Android) with very different data needs hitting the same backend, since each client can ask for exactly what it wants instead of the backend needing a custom endpoint per client shape.

**Downsides:** caching is much harder than REST (you can't just cache by URL anymore, since the query itself defines the response shape), and it pushes real complexity into the server—a single expensive nested query can accidentally hammer your database in a way a fixed REST endpoint never would, so you generally need extra safeguards (query depth limits, cost analysis) that REST doesn't require.

## Which One to Actually Reach For

- **Public-facing API, external developers, simplicity matters:** REST. It's the lowest-friction choice for anyone outside your organization to integrate with.
- **Internal service-to-service calls inside your own system:** gRPC. You control both ends, so you can take the performance win without paying REST's human-readability tax.
- **Multiple client types with very different, overlapping data needs:** GraphQL. Worth the added backend complexity when over/under-fetching would otherwise mean maintaining a pile of custom endpoints.

These aren't mutually exclusive in a real system—it's common to see gRPC internally between services, with a gateway translating that to REST or GraphQL at the edge for external clients.

## How to Bring This Up in an Interview

Default to REST when describing your API in an interview—it's the fastest to communicate clearly, everyone already knows the HTTP verb conventions, and it lets you spend your limited time on the parts of the design that actually differentiate a strong answer (data model, scaling, consistency), instead of relitigating protocol choice. Only reach for gRPC or GraphQL if the interviewer's requirements specifically call for it—e.g., they mention very high-throughput internal service calls (gRPC) or wildly different client data needs (GraphQL). Naming the tradeoff briefly, even while defaulting to REST, is enough to show you know these exist and why you didn't pick them.
