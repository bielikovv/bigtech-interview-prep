# Database Replication

## What It Is

Replication means keeping copies of the same data on multiple database nodes, instead of a single machine holding the only copy. It's the foundation for both durability (surviving a machine failure without losing data) and scaling read traffic beyond what one machine can handle.

## Leader-Follower (Primary-Replica) Replication

The most common setup: one node (the leader/primary) accepts all writes; one or more other nodes (followers/replicas) receive a continuous copy of those writes and serve read traffic.

- **Writes** go to the leader only.
- **Reads** can be served from any replica, spreading read load across multiple machines.
- If the leader fails, one of the followers gets promoted to take its place (failover).

This is the default answer to "how do we scale reads" for most systems, since read traffic is usually much higher than write traffic.

## Synchronous vs. Asynchronous Replication

- **Synchronous:** the leader waits for at least one replica to confirm it received the write before telling the client the write succeeded. Safer (no data loss if the leader dies right after), but slower, since every write waits on a network round trip to a replica.
- **Asynchronous:** the leader confirms the write immediately and replicates to followers in the background. Faster writes, but there's a real risk of losing the most recent writes if the leader fails before replicating them.

Most systems use asynchronous replication by default and only pay for synchronous replication on the specific data where losing a write is actually unacceptable.

## Replication Lag

Because replication (especially asynchronous) isn't instant, a follower's data can be briefly behind the leader's. This means a client can write data, then immediately read it back from a lagging replica and not see their own write yet—a real, common bug source ("read-your-own-writes" problems). Fixes include routing a user's own reads to the leader right after they write, or accepting the lag as a known eventual-consistency tradeoff.

## Leader-Leader (Multi-Leader) Replication

Multiple nodes can each accept writes, and they replicate changes to each other. This helps with write availability across multiple regions/data centers, but introduces write conflicts—what happens when two leaders accept different writes to the same record at nearly the same time? This needs an explicit conflict resolution strategy (last-write-wins, merging, or app-specific logic), which is real added complexity.

## Why This Matters Beyond Just Scaling Reads

Replication is also your durability story: if a node's disk dies, you haven't lost the data, because a replica has it. Any real production database design needs replication for this reason alone, even before considering read-scaling.

## How to Bring This Up in an Interview

Bring this up as soon as you've picked a database and need to explain how it survives a node failure and how it scales reads. Mentioning replication lag specifically, and how you'd handle read-your-own-writes, is a good way to show depth beyond "just add replicas."
