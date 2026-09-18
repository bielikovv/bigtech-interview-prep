# Non-Functional Requirements

## There's No Single "Right" Way to Do This

Everyone approaches non-functional requirements a little differently, and it always feels slightly different from interview to interview. There's no universal pattern you have to follow. What I'm sharing here is just my own approach—the one that's worked for me as a practical best-practice checklist, not a rule handed down from anywhere official.

## 1. Define the Scope

Before anything else, pin down the scale of the system: daily active users (DAU), requests per second (RPS), and roughly how much data your databases will need to hold.

**How to estimate storage:** figure out the size of the smallest meaningful object that gets stored for this system (a tweet, a message, a video's metadata—whatever the core unit is), then multiply by a reasonable assumption of how many of those objects get created per user, and by your estimated user count. Don't forget peak hours—pad your load and storage estimates by 2-3x on top of your baseline average, since real traffic never arrives evenly.

Don't be afraid of big numbers. Some systems genuinely operate at massive scale—10 million DAU, tens of thousands of requests per second—and that's just correct for those systems. Other systems will have much smaller, humbler numbers, and that's correct too. The point isn't to hit some impressive figure; it's to have an approximate, defensible scope before you start designing around it.

## 2. Define Consistency

Once scope is set, decide what consistency model the system actually needs: **eventual consistency** or **strong consistency**.

- **Eventual consistency** fits systems where slightly stale data is acceptable—social network feeds, like/view counters, and similar cases where a few seconds of lag doesn't hurt anyone.
- **Strong consistency** is for systems where correctness can't slip even briefly—financial transactions being the classic example.

This is where the CAP theorem comes in, in short: a distributed system can't guarantee all three of Consistency, Availability, and Partition tolerance at once—only two. In practice, partition tolerance is usually a given for any real distributed system, so the actual tradeoff you're making is between consistency and availability. (A deeper breakdown of CAP theorem and the different consistency models will live elsewhere in this repo once it's ready—not yet.)

## 3. Define Availability

Most services need to prioritize high availability, so state that explicitly. Where possible, define it in terms of "nines"—99.99% (four nines), 99.999% (five nines), 99.9999% (six nines), and so on. It's worth reading up on what each level actually represents, since the short version is: each additional nine is the amount of allowed downtime shrinking by roughly 10x.

## 4. Define Performance

State a target latency for how long a request should take to process, in milliseconds. This varies a lot depending on the system—there's no single number that fits everything. You build the intuition for what's reasonable by exposing yourself to enough different systems over time; it's not something you can shortcut with a formula.

## 5. Security

Security is the hardest of these to generalize, because it's extremely specific to the system you're designing. As a starting checklist, it's usually worth touching on: encrypting data in transit and at rest, protecting any sensitive/PII data, guarding against abuse at the infrastructure level (rate limiting, DDoS protection), and—if the domain calls for it—compliance requirements like GDPR. In most interviews this gets a brief mention rather than deep treatment, since scalability, availability, and consistency tend to be the higher-priority axes.
