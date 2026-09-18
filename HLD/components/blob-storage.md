# Blob / Object Storage

## What It Is

Blob (or object) storage is where large, unstructured pieces of data live—videos, images, audio files, documents, backups—as opposed to structured rows/documents in a database. Think of services like Amazon S3 or Google Cloud Storage: you store an object under a key, and you get it back by that key. There's no querying by content, no joins, no schema—just "store this blob, give me this blob back by its ID."

## Why Not Just Store This in a Regular Database

Databases are built and optimized for relatively small, structured records that get queried, filtered, and joined. A video file can be gigabytes; storing that directly in a relational database's rows would be enormously wasteful and would wreck the performance of everything else sharing that database. Object storage is purpose-built for exactly this: cheap, durable, massively scalable storage for large binary blobs, with a much simpler access pattern.

## The Usual Pattern

Your database doesn't store the actual file—it stores a *reference* to it (a URL or object key pointing into blob storage), plus whatever metadata matters (upload time, owner, file size, processing status). The actual bytes live in the blob store. This keeps your database small and fast, and lets the blob store do what it's good at: storing huge amounts of large, mostly-immutable data cheaply.

## Durability

Object storage services typically replicate every object across multiple machines and often multiple physical locations automatically, which is why they can advertise extremely high durability guarantees (often stated as 99.999999999%, "eleven nines"). You generally don't need to build this yourself—it's part of what you're paying for by using a managed object storage service instead of rolling your own file storage.

## How This Connects to Other Components

- Objects are frequently served through a [CDN](cdn.md) rather than directly from the blob store, to get them physically closer to users and to offload repeat-read traffic.
- Uploading a large object is often handled asynchronously—the client uploads directly to the blob store (sometimes via a pre-signed URL, so the upload doesn't have to route through your application servers at all), and a [message queue](message-queues.md) event triggers any follow-up processing (like transcoding a video into multiple bitrates).

## How to Bring This Up in an Interview

Bring this up whenever the system involves large files—video, images, documents. Explicitly separating "the database stores metadata and a reference" from "the actual bytes live in blob storage" is the key distinction that shows you're not trying to jam large binary data into a relational or document database where it doesn't belong.
