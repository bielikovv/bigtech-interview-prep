# Message Queues & Pub/Sub

## What It Is

A message queue (or pub/sub system) lets one part of a system send a message to another part without calling it directly and waiting for a response. Instead of Service A calling Service B synchronously and blocking until B replies, A drops a message onto a queue and moves on; B picks up the message whenever it's ready to process it.

## Why You Need One

- **Decoupling:** the sender doesn't need to know anything about the receiver, or even whether it's currently running—it just needs the queue to exist.
- **Absorbing load spikes:** if a sudden burst of requests comes in, they queue up instead of overwhelming the downstream service directly. The queue smooths bursty input into a steady, manageable rate of processing.
- **Reliability:** if the downstream service crashes, messages usually stay safely in the queue until it's back up, instead of being lost.
- **Async work:** anything that doesn't need to happen before you respond to the user (sending a confirmation email, generating a thumbnail, updating analytics) is a natural fit for "queue it and move on" instead of making the user wait for it.

## Queue (Point-to-Point) vs. Pub/Sub (Publish-Subscribe)

- **Queue:** each message is consumed by exactly one consumer. Good for distributing work across a pool of workers, where you want each task done once.
- **Pub/Sub:** a publisher sends a message to a topic, and every subscriber to that topic gets a copy. Good when multiple, independent parts of the system all need to react to the same event (e.g., "a video finished uploading" might need to trigger transcoding, a notification, and an analytics update, all independently).

## Delivery Guarantees

- **At-most-once:** a message might be lost, but never delivered twice. Rarely what you actually want.
- **At-least-once:** a message is guaranteed to be delivered, but might be delivered more than once (e.g., if a consumer crashes after processing but before acknowledging). This is the common default, which means consumers need to be **idempotent**—processing the same message twice should be safe and not double-apply an effect.
- **Exactly-once:** the ideal, but genuinely hard and expensive to guarantee end-to-end in a distributed system; often approximated by combining at-least-once delivery with idempotent consumers, rather than a true guarantee from the queue itself.

## Ordering

Some queues guarantee strict ordering (messages are processed in the exact order they were sent), often at the cost of throughput (you can't parallelize processing without risking out-of-order handling). Others sacrifice strict ordering for much higher throughput by allowing parallel consumers. Whether you need ordering depends entirely on the use case—payment events probably need it; independent notification events probably don't.

## How to Bring This Up in an Interview

Bring up a queue whenever part of the workflow doesn't need to block the user's response, or when one event needs to fan out to multiple independent downstream effects. Naming the delivery guarantee you need and explicitly calling out that consumers must be idempotent under at-least-once delivery is a strong signal of real understanding, not just "we'll add a queue here."
