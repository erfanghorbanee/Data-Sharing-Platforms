# CRDT

A **CRDT (Conflict-free Replicated Data Type)** is a data structure that lets **multiple users** update shared data **at the same time**, even while offline or on separate machines, **without causing conflicts** when they sync up later.

CRDTs are especially useful for **real-time collaborative editing**, like Google Docs or shared spreadsheets.

## Why are CRDTs important?

Because in real-time systems:

* **Users can edit the same data simultaneously**
* Data updates must eventually reach **consistency** (everyone sees the same state)
* Traditional locks or version control don’t scale well for fluid, real-time editing

CRDTs **guarantee** that all changes:

* **Merge automatically**
* **Don’t overwrite each other**
* **Always lead to the same final result** no matter the order of updates

## A Simple Example (Counters)

Imagine a counter that three users can increment independently.

With a CRDT:

* Alice adds +2
* Bob adds +3
* Charlie adds +1

Each person’s increment is tracked **separately**, and then all updates are **merged**:
Final result = 2 + 3 + 1 = **6**, no matter the order they synced in.

This is an example of a **G-Counter (Grow-only Counter)**, one of the simplest CRDT types.

## But How Does CRDT *Actually* Work?

At the core, CRDTs ensure **conflict-free merging** by following these principles:

* **Each user replica stores its own state**
* Updates are **designed to be commutative**, **associative**, and **idempotent**
* When replicas sync, they **merge states or operations** in a way that guarantees convergence

There are two main types of CRDTs:

### 1. **State-based CRDTs (CvRDTs)**

* Each replica periodically **sends its entire state** to others.
* When received, the states are **merged** using a **merge function** (like taking the max for counters).
* Requires more bandwidth but simpler logic.

**Example:**
If replica A has counter = 3 and B has counter = 5, the merge is `max(3, 5) = 5`.

### 2. **Operation-based CRDTs (CmRDTs)**

* Instead of sending full state, each update is sent as an **operation** (like “insert 'a' at position 2”).
* Operations must be **commutative** and **causally ordered**.
* More efficient than state-based, but requires a reliable way to deliver operations (e.g. messaging queue or log).

## CRDTs for Text Editing (How it Works for Documents)

Text is tricky because:

* Inserting or deleting characters affects positions.
* Concurrent edits must resolve deterministically.

### CRDT-based text editors (like Yjs or Automerge) often:

1. **Assign unique IDs to each character or element**, such as `(client_id, counter)` tuples.
2. **Track inserts/deletes relative to these IDs**, not absolute positions.
3. Use a **linked list or tree structure** internally to preserve order.
4. When syncing:

   * Insertions/deletions are **merged using ID-based rules**.
   * The same final string results no matter the sync order.

**Example:**

* Alice inserts `"A"` at position 1 → gets ID `(alice, 1)`
* Bob inserts `"B"` at position 1 (concurrently) → gets ID `(bob, 1)`

The final order might be `"AB"` or `"BA"`, but **all devices will agree**, because the tie is broken by comparing the unique IDs deterministically (e.g. by client ID).

## CRDT vs Traditional Sync

| Model                               | Sync Behavior                      | Handles Conflicts?                  | Used in                     |
| ----------------------------------- | ---------------------------------- | ----------------------------------- | --------------------------- |
| **CRDT**                            | Merges automatically               | ✅ Yes, by design                    | Yjs, Automerge, Matrix      |
| **OT (Operational Transformation)** | Uses transform rules               | ✅ Yes                               | Google Docs                 |
| **Central sync (event-based)**      | Pushes state from a central server | ❌ No — assumes one writer at a time | Spreadsheet Space |

## Real-World Tools That Use CRDTs

* **Yjs** – High-performance CRDT library for collaborative apps, supports rich data types
* **Automerge** – JavaScript CRDT library focused on local-first apps and JSON-like structures
* **Matrix Protocol** – Decentralized chat system where room state is CRDT-like
* **Logux / Replicache** – Offline-friendly systems with CRDT-style operational sync
* **Ink & Causal Trees** – CRDT research projects focused on rich text and collaborative editing

## Pros and Cons of CRDTs

| Pros                                             | Cons                                                |
| ------------------------------------------------ | --------------------------------------------------- |
| Full offline support                             | Hard to implement (especially for sequences/text)   |
| Peer-to-peer friendly (no central server needed) | Metadata can grow large (especially in state-based) |
| Automatic conflict resolution                    | Limited undo/redo (no clear history)                |
| Deterministic convergence                        | More complex than traditional data types            |

## Final Takeaway

> **CRDTs let apps work offline, sync automatically, and merge without human intervention.**
> They're hard to build but ideal for decentralized, resilient, collaborative experiences.
