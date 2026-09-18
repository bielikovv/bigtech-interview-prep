# Big Tech Interview Prep: The Full Roadmap

## Why This Repo Exists

Earlier this year (January 2026), I actually got a shot at Google for a Software Engineer III role (L4, if we're being precise about the leveling). I made it through the behavioral round and into the first technical phone screen—and got knocked out right there. At the time I was convinced my prep was enough. I'd been putting in about 3 hours a day for 20 days straight, which felt like real effort, and I'd picked up the foundations. Turns out that was nowhere near enough, especially for Google.

Then in March 2026, Amazon reached out with an interview invite. I turned it down—I knew I wasn't actually ready, so instead of risking another loss, I just let it go.

That one kept nagging at me, and by June 2026 I'd had enough of half-preparing. I decided I genuinely wanted this—Google, or any of the other big names—and that meant actually stepping up my game across the board until I was good enough to clear an interview at one of those companies.

I know cracking Big Tech isn't just about grinding algorithms. It's DSA, plus high-level design, plus low-level design. On top of that, I've heard from other people going through Big Tech interviews that some companies have started introducing AI-assisted rounds—things like debugging alongside an AI tool, or coding sessions where you're expected to work with AI rather than against it. I haven't actually sat through one of those myself yet, but it's likely worth covering here too once I have something real to say about it. This repo is where I'm documenting the whole climb, broken out by area.

## What This Repo Doesn't Cover

I'm not going to get into resume-writing here. The baseline assumption is that you already have real engineering experience, and that experience is reflected honestly on your resume. From what I've seen, Big Tech companies don't really care much about your formal education — they care far more about what you've actually built and the tools you're genuinely fluent in.

The one thing that does matter on the application side is making sure your resume passes ATS (Applicant Tracking System) screening — if it doesn't, you get auto-rejected before a human ever looks at it. Beyond that, be ready to get rejected constantly; that's just the nature of the process. The single most effective thing I've found for actually landing interview invites is to obsessively monitor company careers pages for new openings and apply the moment they go up. I built a small personal parser that scans career sites and pings me on Telegram the second a relevant role appears — it didn't take long to build and it's been genuinely useful. Building something similar for yourself is worth the couple of hours it takes.

## A Note on Where I Actually Stand

Everything in this repo is just my own experience, shared because it feels right to me — not a proven formula. I haven't gone through a final interview loop that ended in an offer yet, so treat this as one person's honest progress report, not a guarantee.

That said, the jump in my algorithmic ability since summer 2026 has been real. Back then, I was constantly stuck on breaking down medium-difficulty problems. I thought "pattern recognition" meant spotting the broad algorithm family—"oh, this is DP," "oh, this is binary search." That's true, but it's only half the picture. The part that actually matters is recognizing the specific variation and sub-variation of that pattern, and knowing exactly how to implement it. That's the skill that makes you strong—once you can pin down the variation, you already know how to solve it.

At the start of that summer, I couldn't do any of that reliably. I was lost on time complexity, didn't know what the constraints were actually telling me about the expected complexity, and regularly blew past 30 minutes on problems I should've solved fast. After the grind described in the DSA section, I can now identify the correct variation roughly 95% of the time, even on hard problems. I can read the constraints and immediately know what time complexity is expected, without wasting time chasing the wrong approach. And for interviews specifically, I know how to walk through brute force first, then the optimized version, and estimate time/space complexity for both without hesitating.

One caveat on that 95%: I don't have a strong math background. I never finished university — I left after the first semester — so I don't have any deep formal grounding in the mathematical side of algorithms. That 95% is specifically about pure DSA ability, the kind of algorithmic thinking that actually shows up in Big Tech interviews — not deep combinatorics, number theory, or the more theoretical corners of computer science. Those are still blind spots for me. The number is honest about what it's measuring: raw problem-solving ability on interview-style DSA, not everything that could technically fall under "algorithms."

I hope this repo brings you the same kind of jump.

## How This Repo Is Structured

- **`/DSA`** — Data Structures & Algorithms. **Fully built out.** Every topic folder has its own `Roadmap.md` (the mental models and pattern breakdowns) and `tasks.md` (the actual problem list I worked through, with links and difficulty). See [DSA/Roadmap.md](DSA/Roadmap.md) for the full story of how I approached this piece.
- **`/HLD`** — High-Level Design. **Fully built out**, aside from one deliberate gap: `/diagrams` is intentionally on hold (see [HLD/Roadmap.md](HLD/Roadmap.md) for why). Requirements, components, and API design are all filled in.
- **`/LLD`** — Low-Level Design. **Fully built out**, aside from `/examples`, which is still empty—I want real worked problems (parking lot, elevator, that classic set) in there before calling it done. See [LLD/Roadmap.md](LLD/Roadmap.md).
- **`/AI`** — AI-assisted interview rounds (AI debugging, AI-paired coding sessions). Not started yet—I'm still investigating what actually shows up in these rounds at Big Tech companies before writing anything down here.

DSA, HLD, and LLD are all filled in now, with the two noted gaps above (HLD diagrams, LLD examples) left open on purpose rather than by neglect. AI is the one section still genuinely unstarted.

Feel free to follow along, use the DSA roadmap and task lists as a reference, and check back as the other sections come online.
