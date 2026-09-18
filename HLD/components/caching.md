# Caching

## What It Is

A cache is a fast, usually in-memory store that holds a copy of data so you don't have to go back to a slower source (typically a database) every time that data is requested. The entire point is trading a bit of staleness risk for a large speed and load reduction win.

## Caching Strategies

- **Cache-aside (lazy loading):** the application checks the cache first; on a miss, it reads from the database, then writes the result into the cache for next time. Simple and the most common default—but the first request for any given key always hits the database (a "cold" cache).
- **Write-through:** every write goes to the cache and the database at the same time, synchronously. Keeps the cache always fresh, at the cost of every write being a bit slower (two writes instead of one).
- **Write-back (write-behind):** writes go to the cache immediately and are flushed to the database asynchronously, later. Fast writes, but risks data loss if the cache fails before the flush happens.
- **Read-through:** similar to cache-aside, but the cache itself (not the application) is responsible for loading from the database on a miss—common in managed caching layers.

## Eviction Policies

A cache has limited memory, so when it's full, something has to get kicked out to make room for something new:

- **LRU (Least Recently Used):** evict whatever hasn't been accessed in the longest time. The most common default, and usually the right first answer in an interview.
- **LFU (Least Frequently Used):** evict whatever has been accessed the fewest times overall. Better than LRU when access frequency matters more than recency, but more expensive to track.
- **FIFO:** evict whatever was added first, regardless of how often it's used. Simple, but ignores actual usage patterns.
- **TTL (Time To Live):** entries expire automatically after a fixed duration, independent of memory pressure—useful for data that's only valid for a limited window (like a session token).

## Where Caching Actually Lives

- **Client-side:** in the browser or app, closest to the user, zero network cost when it hits.
- **CDN:** caches static/semi-static content geographically close to users. (See [CDN](cdn.md).)
- **Application-level / distributed cache:** a shared cache layer like Redis or Memcached, sitting between your services and your database.
- **Database-level:** the database's own internal query/buffer cache.

## The Real Risk: Cache Invalidation

Caching's hardest problem isn't storing data fast—it's knowing when cached data is no longer valid and needs to be refreshed or removed. Common approaches: a short TTL so staleness self-heals, explicit invalidation when the underlying data changes (the write path deletes or updates the relevant cache key), or just accepting some bounded staleness because the data doesn't need to be perfectly fresh (see eventual consistency in [CAP Theorem & Consistency Models](cap-theorem.md)).

## Cache Stampede / Thundering Herd

If a popular cache key expires and a huge number of concurrent requests all miss at once, they can all hit the database simultaneously, spiking load right when the database least wants it. Mitigations include locking so only one request repopulates the cache while others wait, or staggering TTLs so keys don't all expire at exactly the same moment.

## How to Bring This Up in an Interview

Caching is almost always worth mentioning once you've established the database is the bottleneck for reads. State which strategy and eviction policy you'd use and why, and be ready to talk about invalidation—that's usually the follow-up question that separates a surface-level answer from a real one.
