# Database Sharding

## What It Is

Sharding (also called partitioning) splits a single logical database's data across multiple physical machines, where each machine (shard) holds only a subset of the total data. This is different from replication (see [Database Replication](database-replication.md))—replication copies the *same* data onto multiple machines; sharding splits *different* data across multiple machines. You typically use both together: each shard is itself replicated for durability.

## Why You Need It

Replication alone scales reads, but it doesn't help once your *total data volume* or *write throughput* outgrows what a single leader machine can hold or handle. At some point, one machine's disk, memory, or write capacity is just not enough, no matter how good the hardware—that's when you need to split the data itself across multiple machines.

## Sharding Strategies

- **Range-based sharding:** each shard holds a contiguous range of key values (e.g., user IDs 1-1,000,000 on shard A, 1,000,001-2,000,000 on shard B). Easy to reason about and supports efficient range queries, but can create "hot shards" if traffic isn't evenly distributed across the key range (e.g., all your newest, most-active users land on the same shard).
- **Hash-based sharding:** hash the key and use the hash to decide the shard, spreading data more evenly and avoiding hot spots from sequential keys. Loses easy range-query support, since consecutive keys are now scattered across shards.
- **Consistent hashing:** a specific, better way to do hash-based sharding, where adding or removing a shard only requires moving a small fraction of data instead of a massive rehash. (See [Consistent Hashing](consistent-hashing.md)—this is the direct real-world application of that concept.)
- **Directory-based sharding:** keep an explicit lookup table mapping keys (or key ranges) to shards. Most flexible—you can rebalance manually and precisely—but that lookup table itself becomes a critical piece of infrastructure that needs to be fast and highly available.

## The Real Cost: Cross-Shard Operations

Once data is split across shards, anything that needs to touch data spanning multiple shards gets much harder:

- **Joins across shards** are expensive or unsupported—this is a big part of why sharded systems lean toward denormalized data models.
- **Transactions across shards** need distributed transaction protocols (like two-phase commit) or need to be avoided by design, since they're slow and add real complexity.
- **Aggregate queries** ("count all users") need to query every shard and combine results, instead of a single fast query.

## Choosing a Shard Key

The shard key is the single most consequential decision here. A bad shard key creates a hot shard (too much traffic concentrated on one shard) or forces cross-shard queries for your most common access pattern. The right shard key is usually whatever field your queries filter on most often—if you almost always query "give me all orders for user X," sharding by user ID keeps that query on a single shard.

## How to Bring This Up in an Interview

Bring sharding up once your storage or write-throughput estimate (from your non-functional requirements scope calculation) clearly exceeds what a single machine can handle. Naming your shard key explicitly, and explaining why it matches your dominant access pattern, is what separates a real answer from just saying "we'll shard it."
