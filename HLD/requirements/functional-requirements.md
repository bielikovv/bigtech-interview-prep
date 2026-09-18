# Functional Requirements

## Talk to Your Interviewer, But Own the Room

The most important skill in this phase isn't knowing every system inside and out—it's communication. Your interviewer expects you to drive: to make assumptions, state them out loud, and move forward with confidence instead of waiting to be told what to do. You're expected to have a rough idea of how the system you're designing actually works. If you genuinely don't know, ask—interviewers are generally happy to fill you in. But the default mode should be confident assumption - making, not asking permission for every decision.

## Two Sections: Functional Requirements & Out of Scope

Structure this part of the interview around exactly two sections:

- **Functional Requirements** — what the system actually needs to do.
- **Out of Scope** — what you're explicitly choosing not to design.

Out of Scope isn't an afterthought; it's where you park things like authentication, or any component that's complex enough to eat your entire interview if you let it. Calling these out explicitly shows the interviewer you understand the full problem space, even though you're deliberately not solving all of it right now.

## Keep It Short: This Is a 3-5 Minute Step

This whole phase should take maybe 3 minutes, 5 at the outside if the system is genuinely complicated and needs real care to scope properly. It's a warm-up, not the interview itself—if you find yourself spending 10+ minutes debating requirements, you're eating time you need for the actual design.

## Scope Discipline: Don't Design Everything

Be disciplined about what belongs in Functional Requirements. The instinct to list every feature the real-world product has is the trap—your job is to identify the core of the system, not recreate the whole product.

**Example:** if you're asked to design YouTube, the core is video upload and streaming. Don't start designing comments, likes, or the recommendation system—those are separate, complex systems in their own right and will eat time you need for the core problem. Only go there if you finish the core design with time to spare, or if the interviewer explicitly asks you to extend into one of those areas.

## How to Phrase Them

Write each requirement as a simple, testable statement—not a paragraph of prose:

- "User can upload a video."
- "User can watch a video with adaptive quality based on their network."
- "System can serve video content to millions of concurrent viewers."

Short, action-oriented statements like these keep the list scannable and make it obvious later which requirement each part of your design is actually serving.
