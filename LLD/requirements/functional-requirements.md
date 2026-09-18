# Functional Requirements

## The Same Two Sections, Tighter Discipline

The overall exercise looks like [HLD's functional requirements step](../../HLD/requirements/functional-requirements.md)—state what the system needs to do, then explicitly call out what's out of scope. What changes in LLD is how aggressively you have to apply that scope discipline.

In HLD, "out of scope" mostly means "this is a whole other complex system, I'm parking it." In LLD, you're not sketching a system at the infrastructure level—you're about to write real object-oriented code for it, live, under time pressure. Every domain you leave in scope is a set of classes, fields, and methods you now have to actually design and defend. So the goal isn't just "what does this system do"—it's "what is the smallest, cleanest slice of this system I can model in code that still demonstrates I understand the problem."

## Purely Design What's Needed—Not Everything

Be ruthless about stripping any domain that doesn't serve the core objects you're about to design. If you're modeling a parking lot system, you probably don't need a full payment-processing subsystem with all its edge cases—you need enough of a `Ticket` and `Payment` concept to show the relationship exists, not a fully worked-out billing engine. The instinct to be "complete" is the trap here even more than it is in HLD, because completeness in LLD costs you actual lines of working code, not just a mention on a whiteboard.

## How to Phrase Them

Same as HLD: short, testable statements, not paragraphs.

- "A vehicle can enter and be assigned a parking spot."
- "A vehicle can exit and be charged based on duration."
- "The system supports multiple vehicle and spot types."

Keep the list short enough that every requirement on it maps to something you can actually point to in the class diagram a few minutes later.
