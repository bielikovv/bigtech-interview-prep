# My Low-Level Design Journey: Building the OOP Design Muscle

## Do You Even Need This?

From what I've seen and heard, LLD shows up specifically at Amazon—I haven't come across it being a hard requirement at other Big Tech companies. Meta gets mentioned sometimes as a maybe, but nowhere near as consistently as it does for Amazon. Same advice as the [HLD roadmap](../HLD/Roadmap.md): confirm what your target company and level actually expects before sinking real time into this.

## What LLD Actually Is

The interview shape rhymes with HLD—you're still handed a system to design—but the deliverable is completely different. Instead of drawing infrastructure (load balancers, caches, databases), you're expected to model the system as clean, working object-oriented code. That means you actually need real design-pattern knowledge, not just the ability to name them in passing. In practice, creational patterns cover the vast majority of what comes up: **Factory Method** and **Abstract Factory** are the two you'll reach for most; **Singleton** is worth knowing too, but it shows up far less often than the two factory patterns.

## How This Folder Is Structured

- **`/requirements`** — Functional and non-functional requirements, same two-part shape as HLD, but the actual content of each is meaningfully different for LLD. See [Functional Requirements](requirements/functional-requirements.md) and [Non-Functional Requirements](requirements/non-functional-requirements.md).
- **`/diagrams`** — Not full UML. Just entities, their fields, and which entity owns which method. See [diagrams/README.md](diagrams/README.md).
- **`/examples`** — Worked LLD problems and the code that came out of them. Empty for now; will fill in as I work through real problems.

## The Order I'm Taking

Same overall flow as an HLD interview—functional requirements first, then non-functional, then a diagram, then the actual build—but each step is scoped and weighted differently once you're modeling objects instead of infrastructure. The requirements and diagrams files above go into the actual differences; this roadmap is just the map.

## What's Still Coming

The `/examples` folder is genuinely empty right now—I want to work through several real LLD problems (parking lot, elevator system, that kind of classic set) before I have anything worth sharing there.
