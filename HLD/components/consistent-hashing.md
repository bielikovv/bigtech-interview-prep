# Consistent Hashing

## The Problem It Solves

A load balancer, a cache client, and a sharded database's routing layer all face the same problem: given N servers, which one should handle this specific key or request? The naive fix is `server = hash(key) % N`. It works fine—until N changes. Add or remove a single server, and `% N` now points almost every key at a different server, all at once. For a cache, that's a near-total cache wipe. For a sharded database, that's a massive, unnecessary data migration. Consistent hashing exists to make adding or removing a node cheap instead of catastrophic.

## The Simple Picture

Before the mechanics, here's the intuition: picture a Colosseum — a giant circular arena, divided all the way around into slices. Every slice is a range of hash values, and each server plants itself at one specific point on that circle. A request's key also lands at some point on the circle, and whichever server is the next one you hit walking clockwise from that point is the one that handles it.

That's the whole trick. Because each server only "owns" the arc directly behind it, adding a new server only steals a slice of arc from its one neighbor, and removing a server only hands its arc back to the next one over. Nothing on the rest of the circle even notices. That's exactly why this fixes the `% N` problem: instead of one change reshuffling the entire ring, it only ever disturbs the small neighborhood right around it.

## How It Works

1. Imagine a circle (a hash ring) representing the full range of hash output values, from 0 to some max value, wrapping back around to 0.
2. Each server gets hashed onto a position on this ring (e.g., `hash(server_id)`).
3. Each key also gets hashed onto the same ring (`hash(key)`).
4. A key belongs to whichever server is the *next one clockwise* on the ring from the key's position.

When you add a new server, it only takes over the keys between itself and the previous server on the ring—everything else stays exactly where it was. When you remove a server, only its keys need to move, to the next server clockwise. Instead of remapping everything, you remap roughly `1/N` of the keys.

## Virtual Nodes: Fixing Uneven Distribution

With only one point per server on the ring, you can easily end up with an uneven distribution—one server might own a huge arc of the ring while another owns a tiny sliver, especially with a small number of servers. The fix is **virtual nodes**: each physical server gets hashed onto the ring multiple times (say, 100-200 virtual positions per server), spreading its ownership more evenly around the ring. This also makes rebalancing after adding/removing a node smoother, since the load shed or gained is spread across many small arcs instead of one big one.

## Where This Actually Shows Up

- **Distributed caches** (like a sharded Redis or Memcached setup): which cache node holds a given key.
- **Sharded databases**: which shard a given row/document lives on.
- **Load balancers with session affinity**: routing a given client consistently to the same backend server.
- **Distributed hash tables (DHTs)** in peer-to-peer systems.

## How to Bring This Up in an Interview

Consistent hashing is the answer whenever you're asked "what happens when you add or remove a node?" for any sharded or partitioned component. Mentioning virtual nodes specifically is a good signal that you understand the technique beyond the surface level—it shows you know the naive version has a real, fixable flaw.
