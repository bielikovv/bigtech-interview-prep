# SQL vs. NoSQL Databases

## The Actual Question Behind the Question

"SQL or NoSQL" isn't really about which is objectively better—it's about which set of tradeoffs fits the data you're storing and the access patterns you need. Both are correct answers in different systems.

## SQL (Relational)

Data lives in tables with a fixed schema, and relationships between tables are modeled explicitly (foreign keys) and queried with joins.

**Strengths:**
- Strong consistency and ACID transactions (Atomicity, Consistency, Isolation, Durability)—the guarantees you want when correctness can't slip, like financial data.
- Complex queries across related data (joins) are a first-class feature.
- A fixed schema catches data integrity issues early, since the database itself enforces structure.

**Weaknesses:**
- Scaling horizontally (across many machines) is harder—relational databases were built assuming mostly-vertical scaling, and joins across shards are painful. (See [Database Sharding](database-sharding.md).)
- Schema changes on a huge table can be slow and disruptive.

## NoSQL

An umbrella term for several genuinely different data models, not one thing:

- **Key-value stores** (e.g., DynamoDB, Redis as a store): simplest model, just a key mapped to a value. Extremely fast lookups, no query flexibility beyond the key.
- **Document stores** (e.g., MongoDB): store semi-structured documents (JSON-like), with a flexible schema per document. Good fit when your data's shape naturally varies or nests.
- **Column-family stores** (e.g., Cassandra): optimized for very high write throughput and horizontal scale, at the cost of weaker consistency and query flexibility.
- **Graph databases** (e.g., Neo4j): optimized for traversing relationships (friend-of-a-friend style queries)—a genuinely different access pattern than the others.

**Strengths (general, varies by type):**
- Built from the ground up for horizontal scaling.
- Flexible or schema-less data models adapt easily to changing requirements.
- Often favor availability and eventual consistency over strict consistency (see [CAP Theorem](cap-theorem.md)), which is exactly the tradeoff a lot of large-scale systems want.

**Weaknesses:**
- Weaker consistency guarantees by default (though some support tunable consistency).
- Joins across collections/tables are either unsupported or expensive, so you often denormalize data instead.

## How to Actually Decide

Ask: does this data have complex relationships that need to be queried together, and does it need strong transactional guarantees? That points to SQL. Does this data need to scale to a massive, unpredictable volume, with a flexible or simple shape, where eventual consistency is acceptable? That points to NoSQL. Most real, large systems end up using both—SQL for the core transactional data (accounts, orders), NoSQL for the high-volume, loosely-structured data (activity logs, session data, feeds).

## How to Bring This Up in an Interview

Don't just declare "I'll use NoSQL because it scales better" without justification—that's a red flag, not a strength. Tie the choice directly to a specific access pattern or consistency requirement from your functional/non-functional requirements. "Users need strongly consistent account balances, so this table is SQL. The activity feed just needs to scale and can tolerate eventual consistency, so that's a document store" is the level of specificity to aim for.
