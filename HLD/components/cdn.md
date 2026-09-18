# CDN (Content Delivery Network)

## What It Is

A CDN is a network of geographically distributed servers ("edge servers" or "points of presence") that cache and serve content physically close to the end user, instead of every request traveling all the way back to your origin server. It's essentially caching (see [Caching](caching.md)), but distributed across the planet by location rather than sitting in one place next to your database.

## Why It Matters

Physical distance directly costs latency—a request from Tokyo to a server in Virginia is going to be slow no matter how fast that server is, just because of the round-trip time. A CDN edge node in or near Tokyo can serve that same content in a fraction of the time. It also offloads a huge amount of traffic away from your origin servers, since popular content gets served entirely from the edge without ever reaching your infrastructure.

## What Belongs on a CDN

- **Static assets:** images, videos, CSS, JS bundles—content that doesn't change per-request and is safe to cache widely.
- **Semi-static content:** things that change occasionally but not per-request, like a rendered product page that only updates a few times a day.

Highly dynamic, per-user content (a personalized feed, an account balance) generally isn't a good CDN fit, since caching it wouldn't help—it's different for every request anyway.

## Push vs. Pull CDNs

- **Pull CDN:** the edge server fetches content from your origin the first time it's requested, then caches it for subsequent requests (like cache-aside, but geographically). Simple, and the default for most use cases.
- **Push CDN:** you proactively upload content to the CDN ahead of time, rather than waiting for a first request to trigger caching. Useful when you know exactly what needs to be available and want zero cold-start latency for the very first viewer.

## Cache Invalidation on a CDN

Same fundamental problem as any cache—when the origin content changes, stale copies can linger at edge nodes until their TTL expires. CDNs typically support an explicit "purge" or "invalidate" API call for when you need a change to propagate immediately instead of waiting out the TTL.

## Where This Shows Up in Streaming Systems

CDNs are central to how video streaming actually scales—see how Netflix/YouTube-style adaptive bitrate streaming and Twitch-style live ingest both lean on CDN edge distribution to avoid every viewer hitting origin servers directly.

## How to Bring This Up in an Interview

Bring up a CDN as soon as your system serves any meaningful amount of static or semi-static content to a geographically spread-out user base—images, video, or any "heavy" content that doesn't change per request. It's an easy, high-value addition that shows you're thinking about latency and origin load, not just correctness.
