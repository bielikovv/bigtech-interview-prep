# My High-Level Design Journey: Building the System Design Muscle

## Do You Even Need This?

High-level design matters, and learning it makes you a better engineer regardless of whether an interview asks for it. That said, don't assume every Big Tech interview loop requires it. From what I've seen, Google doesn't require HLD or LLD at Software Engineer III (L4) or below—so if your experience is thinner and you need to prioritize, you can focus fully on DSA for those roles. Amazon, Meta, and Netflix, on the other hand, reportedly expect HLD even at lower levels. Know exactly what the company and level you're targeting actually requires before you decide what to skip.

## Where I Started

I came into this with a head start: years as a backend/web dev engineer had already given me a working understanding of the fundamental system components—rate limiters, load balancers, API gateways, caching, databases, CDNs, and the rest. If you don't have that background yet, learning these building blocks individually is step one, non-negotiable.

**A note on this repo's current state:** detailed write-ups on each of these components are still in progress. This roadmap describes the path I took; the actual component breakdowns will be filled into this folder over time, so don't expect them here yet.

## The Order I Took

1. **Core building blocks, one at a time.** Before touching a full system design, I designed the pieces in isolation—a rate limiter, a database, something like Redis. This part is genuinely boring at first, because you don't yet know what belongs on the diagram, and nailing functional vs. non-functional requirements is hard when you have nothing to anchor them to. I leaned on AI heavily here to speed up the learning curve. Push through it anyway—this is where the actual intuition for the rest of HLD comes from.
2. **Classic, well-trodden systems.** Once the building blocks felt solid, I moved to designing familiar systems—social networks, LinkedIn-style platforms—to practice assembling components into something coherent.
3. **Streaming systems.** This was an important category on its own: how Spotify chunks and encodes audio into different bitrates and streams it over a TCP connection; how Netflix and YouTube handle adaptive bitrate (ABR) streaming, adjusting chunk quality to your current network conditions; how Twitch's ingest servers handle real-time chunking and delivery for live streams.
4. **Caching and CDN-heavy designs.** Systems where caching strategy and CDN placement are the actual crux of the problem, not an afterthought.
5. **Geospatial systems.** Location-based designs like Uber-style GPS tracking—a distinct enough category of problems to be worth its own practice.

## The Time Investment

Roughly 2 hours a day for about 20 days—noticeably less than the DSA grind. That's not because HLD is easier; it's because the backend experience and prior system design exposure meant I wasn't starting from zero. If you're coming in without that background, budget more time, especially for the building-blocks phase above.

## How This Folder Is Structured

- **`/requirements`** — Functional and non-functional requirements: how to define what a system must do, and what qualities it must have while doing it. Placeholder content for now.
- **`/components`** — The individual building blocks (rate limiters, load balancers, API gateways, caching, databases, CDNs, etc.), each broken down on its own before you ever combine them. Not filled in yet.
- **`/api-design`** — How to go from requirements to an actual API surface: REST vs. gRPC vs. GraphQL, and when each one actually earns its place.
- **`/diagrams`** — The system diagrams themselves, tying requirements, components, and API design together into something you could actually draw and talk through in an interview. **Deliberately on hold**—see [diagrams/README.md](diagrams/README.md) for why.

## What's Still Coming

Functional and non-functional requirement gathering is its own skill and deserves real depth—that, along with the individual component breakdowns and the streaming/caching/geospatial system write-ups, will be added to the folders above as I finish preparing them. Diagrams specifically are on hold on purpose, not just unstarted—I want real reps drawing them before I lock in conventions I'd probably have to rewrite anyway.
