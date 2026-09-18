# Diagrams

## Not Full UML—Just Enough to Make the Code Mechanical

Same instinct as HLD's diagram step: don't try to draw something perfect. For LLD specifically, the goal is just to nail down three things for each entity in the system:

1. **The main entities**—the core nouns in the system (e.g., `ParkingSpot`, `Vehicle`, `Ticket`).
2. **The fields each entity is expected to hold**—just the data, not the logic.
3. **Which entity owns which method**—by name only. Don't work out the method body here.

## Why Method Names Are Enough

You don't need to code the methods at this stage—you just need to say which entity a given method belongs to, and give it a name that makes its purpose obvious (`assignSpot()`, `calculateFee()`). Once you know which entity owns which behavior and how the entities connect to each other, writing the actual code afterward becomes mostly mechanical: you already know exactly where every piece of logic belongs, so you're not making architectural decisions and writing code at the same time under pressure.

## How to Use This in an Interview

Spend your diagramming time on the connections, not the polish. A rough box-and-arrow sketch that correctly shows "this entity holds a reference to that one" and "this method lives here, not there" is worth far more than a clean-looking diagram that gets the relationships wrong. The diagram is scaffolding for the code you're about to write, not a deliverable in its own right.
