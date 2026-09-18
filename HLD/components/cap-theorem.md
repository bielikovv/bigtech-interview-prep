# CAP Theorem & Consistency Models

## The Core Idea

Any distributed data system—meaning data that lives on more than one machine—can't simultaneously guarantee all three of the following when a network partition happens:

- **Consistency (C):** every read gets the most recent write, or an error. Every node in the system agrees on the current value.
- **Availability (A):** every request gets a (non-error) response, even if it's not guaranteed to be the most recent value.
- **Partition Tolerance (P):** the system keeps working even when network failures split it into groups of nodes that can't talk to each other.

The theorem says: when a partition actually happens, you have to pick between C and A. You can't have both. This is *only* a statement about behavior during a partition—when the network is healthy, a well-designed system can give you both consistency and availability.

## Why Partition Tolerance Isn't Really a Choice

In practice, any real distributed system spread across multiple machines or data centers has to assume partitions will happen eventually—networks fail, cables get cut, data centers lose connectivity. So P is basically a given, not something you trade away. The actual decision you're making when you design a system is: **when a partition happens, do I favor Consistency or Availability?**

- **CP (Consistency + Partition Tolerance):** during a partition, the system refuses to serve some requests rather than risk returning stale or conflicting data. Good fit for systems where wrong data is worse than no data—banking, inventory counts, anything financial.
- **AP (Availability + Partition Tolerance):** during a partition, the system keeps responding to every request, even if that means different nodes might briefly disagree about the current value. Good fit for systems where staying up matters more than perfect accuracy—social media feeds, like counts, presence indicators ("user is online").

## Consistency Models, Beyond the Binary

"Consistency" isn't actually binary in practice—there's a spectrum:

- **Strong consistency:** every read reflects the latest write, everywhere, immediately. Expensive to maintain across distributed nodes, since it usually requires coordination (consensus protocols, locking, or routing all writes/reads through a single leader).
- **Eventual consistency:** after a write, all nodes will *eventually* converge on the same value, but there's a window where different nodes might return different (stale) answers. Much cheaper and more available, at the cost of that temporary inconsistency window.
- **Causal consistency** (in between): operations that are causally related (a comment that replies to a post) are seen in the correct order by everyone, but unrelated operations can be seen in different orders by different nodes.

## How to Use This in an Interview

When you get to non-functional requirements, state explicitly which side of the CAP tradeoff the system needs, and why:

- "This is a social feed, so eventual consistency is fine—if a like count is a few seconds stale, nobody notices."
- "This is a payments system, so we need strong consistency—an account balance can't be wrong even briefly."

Don't just say "CAP theorem" and move on—naming the actual tradeoff and tying it to a concrete piece of your system is what shows real understanding.
