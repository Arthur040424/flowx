# FlowX - Decisions Log

A running log of significant technical decisions made during development, including the reasoning behind each choice. This exists so that every decision can be defended in a technical interview or team discussion.

---

## 001 - Why Redis alongside PostgreSQL

**Decision:** Use Redis as an in-memory store alongside PostgreSQL.

**Reasoning:** PostgreSQL stores data on disk and is optimised for permanent, structured data with transaction guarantees. Redis stores data in memory and is optimised for speed. For job queues and real-time pub/sub messaging, latency matters more than permanence - a job queue entry does not need to survive forever, it needs to be picked up fast. Redis handles this at a fraction of the latency of a disk-based query.

**Alternative considered:** Storing job queues directly in PostgreSQL. Rejected because polling a database for new jobs is inefficient at scale and adds unnecessary load to the primary database.

## 003 - Why BullMQ for job queues

**Decision:** Use BullMQ as the job queue library.

**Reasoning:** BullMQ provides atomic job locking (preventing two workers from picking up the same job), automatic retries with configurable backoff, priority queues, and a dead-letter queue for permanently failed jobs. Building this from scratch on top of raw Redis would require significant engineering efforts and introduce risk of subtle concurrency bugs.

**Alternative considered:** Building a custom queue on raw Redis.
Rejected because BullMQ solves known hard problems(atomic locking, retry logic that are not worth re-implementing

---

## 003 - Why Websockets over polling

**Decision:** Use WebSockets for real-time execution monitoring

**Reasoning:** HTTP polling requires the client to repeatedly ask the server for updates on a timer, resulting in unnecessary requests and artificial delay between an event occurring and the client seeing it. WebSockets establish a persistent bidirectional connection to the server can push updates instantly when they occur, with no polling overhead.

**Alternative considered:** HTTP long-polling or Server-Sent Events.
WebSockets were chosen because FlowX requires bidirectional communication - not just server-to-client updates.

---

## 004 - Why UUID over auto-incrementing integers for IDs

**Decision:** Use UUID as the primary key type for all tables.

**Reasoning:** Auto0-incrementing integers expose sequential record counts in URLs, making it trivial to enumerate records and infer system volume. UUIDs are randomly generated and statistically impossible to guess, improving security and preventing accidental data exposure through URL manipulation.

---

## 005 - Why JSONB for step configuration

**Decision:** Store step configuration as JSONB rather than individual columns.x

**Reasoning:** Different step types (HTTP, SQL, transform) require completely different configuration fields. Creating individual columns for every possible configuration would result in a sparse table with mostly null values. JSONB allows each step type to store exactly the fields it needs, while still being queryable by PostgreSQL when needed.
