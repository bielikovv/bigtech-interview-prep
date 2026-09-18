# Non-Functional Requirements

## A Genuinely Different Set of Axes Than HLD

This is the part that trips people up coming straight from HLD practice. [HLD's non-functional requirements](../../HLD/requirements/non-functional-requirements.md) are about the system's operational qualities—availability, consistency, scalability. LLD's non-functional requirements are about the quality of the *object model itself*. Four axes cover most of what comes up:

## 1. Extensibility (Optional)

Not every design needs this—don't force it where it doesn't belong. Look for objects in your model that might reasonably grow new variants later. The classic example is a delivery domain: today it's just "deliver by car," but tomorrow it might need "deliver by bike" or "deliver by drone." That's exactly the shape **Abstract Factory** exists for—define an abstract class and an abstract method, then let concrete subclasses implement each variant without touching the code that already works.

If a model doesn't have an obvious extension point, leave extensibility out entirely rather than inventing one. When you do call it out, keep it to one short, concrete parenthetical, e.g.:

> Extensibility (we need to make sure we can easily add new delivery types in the future without touching existing delivery logic).

## 2. Thread Safety

Make sure you've actually thought through and resolved the race conditions your design could hit—anywhere multiple threads could read or write shared state at the same time. State it briefly, with a concrete example of where a race condition could occur in your specific design, e.g.:

> Thread safety (two threads assigning the same parking spot simultaneously must not both succeed).

## 3. Performance

Relevant specifically when the design leans on real algorithmic work—e.g., finding the nearest available spot, or picking the optimal assignment among several candidates. State a target the same way you would in HLD: concretely, tied to the specific operation that needs it, not as a generic "the system should be fast."

## 4. Data Integrity

Make sure the operations you're modeling actually preserve consistency, especially anything involving multiple pieces of state that must change together. The classic example is a bank transfer between two accounts: you're not just decrementing one balance and incrementing another—both operations need to be tracked/logged together so a failure mid-transfer can't leave the system in an inconsistent state (one account debited, the other never credited).

## Keep This Section Short

Same rule as HLD: state each axis in a sentence or two, with a concrete example tied to your actual design. This isn't the place to prove you know every pattern—it's the place to show you know which qualities this specific object model actually needs.
