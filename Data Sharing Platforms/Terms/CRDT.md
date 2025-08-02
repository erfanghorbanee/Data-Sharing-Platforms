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

### CRDT-based text editors (like Yjs or Automerge) often

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

This means:

* Every device (user’s app, browser, etc.) applies **the same rule** to decide the order.
* For example: “if two elements are concurrent, order them by comparing their IDs lexicographically.”

So:

* If `(alice, 1)` < `(bob, 1)` alphabetically (or numerically), then all devices will insert `"A"` before `"B"` → result: `"AB"`
* This is a **deterministic rule**, meaning **every device independently makes the same decision**, even if the edits arrived in different order due to network delay.

**Why is this important?**

In distributed, real-time collaboration:

* Devices often receive edits **out of order**.
* CRDTs use these unique IDs and ordering rules to ensure **eventual consistency** — every replica (device) converges to the same document state **without needing a central server** or lock.

## CRDT vs OT vs Naïve Event-Based Sync

In collaborative systems, all synchronization approaches are event-driven at some level. The key difference lies in **how concurrent operations are handled** and **where conflict resolution happens**.

| Model                               | Conflict Handling                 | Merge Logic Location     | Assumes Central Server? | Used In                    |
|------------------------------------|----------------------------------|---------------------------|--------------------------|----------------------------|
| **CRDT**                            | ✅ Automatic conflict resolution | In the data structure     | ❌ (can be decentralized) | Yjs, Automerge, Matrix     |
| **OT (Operational Transformation)** | ✅ via transform functions       | In client/server logic    | ✅ Yes (usually central)  | Google Docs                |
| **Naïve event-push**                | ❌ Overwrites possible           | None                      | ✅ Yes                   | Spreadsheet Space (2016)   |

### Key Takeaways

* **All models use events** to propagate changes between users.
  * Yjs uses WebSockets or WebRTC to send CRDT updates.
  * Google Docs uses event queues to deliver OT operations and reconcile them.
* **Naïve event-based systems** treat edits as push notifications with no built-in merge handling.
* **OT** systems transform incoming edits to maintain consistency, often requiring a central server for sequencing.
* **CRDTs** embed merge logic directly into the data structures, making them suitable for decentralized or peer-to-peer use cases.

## Real-World Tools That Use CRDTs

### **1. Yjs**

**What it is**:
Yjs is a high-performance CRDT library written in JavaScript, designed for **real-time collaboration** in web applications.

**How it works**:

* Uses **Y-CRDT**, a custom CRDT implementation optimized for performance and compactness.
* Synchronizes shared documents like **text, arrays, maps, and XML trees** across peers in real time.
* Supports **offline editing**, automatic conflict resolution, and **fine-grained updates**.

**Key Features**:

* Works with WebRTC, WebSockets, or any custom transport.
* Efficient binary encoding (smaller payloads).
* Plugins for rich editors: **ProseMirror, CodeMirror, Slate.js**.

**Limitations**:

* Undo/redo must be implemented manually or via plugins.
* Needs external storage/backends for persistence.

### **2. Automerge**

**🔧 What it is**:
Automerge is a CRDT library designed for **local-first software**. It’s written in JavaScript and also has Rust bindings.

**How it works**:

* Uses **JSON-like data structures**: objects, arrays, maps, etc.
* Maintains a full **history of operations**, allowing **merging** without conflicts even after offline edits.
* Data is stored and synced as **changesets** (like Git commits).

**Key Features**:

* Optimized for local-first apps (edits first, syncs later).
* Supports time-travel/debugging via the full change history.
* Automatic merging of concurrent changes.

**Limitations**:

* **Larger memory footprint** due to retained history.
* Not as performant as Yjs in terms of document size and sync speed.
* Still evolving; some advanced features (like garbage collection) are experimental.

### **3. Matrix Protocol**

**What it is**:
Matrix is an open standard for **decentralized messaging and VoIP**, used in apps like **Element**.

**How it works**:

* Matrix rooms are replicated across servers.
* The **room state** (e.g. user membership, permissions) is **CRDT-like** — it supports merging without coordination.
* Uses **eventual consistency** and **conflict resolution rules** to converge on a single state.

**Key Features**:

* Fully decentralized.
* End-to-end encrypted messaging.
* Extensible protocol (can carry JSON events of any kind).

**Limitations**:

* Not a pure CRDT system — it's a **hybrid** model.
* History is append-only; can't easily "undo" state changes globally.
* Some conflicts (e.g. simultaneous kicks) require custom resolution logic.

### **4. Logux / Replicache**

These are **synchronization frameworks** that use CRDT-inspired ideas for offline and collaborative apps.

#### **Logux**

* Uses **logs of actions** (like Redux actions) that are synced across clients.
* Inspired by CRDTs but doesn't use a formal CRDT model.
* Allows **optimistic UI updates** and rollback on conflict.

#### **Replicache**

* Targets **high-performance, offline-first apps**, especially for mobile/web.
* Syncs changes from the local cache to a server in **batches**.
* Uses a **custom merge algorithm** — not pure CRDT but follows similar principles (conflict-free, mergeable).

**💡 Key Features**:

* Fast and responsive UX even with no network.
* Fine-grained control over sync and merge logic.

**Limitations**:

* More developer involvement needed to define how merges work.
* Not standardized like Yjs/Automerge — more opinionated tooling.

### **5. Ink & Causal Trees**

**What they are**:
These are **research CRDT projects** focused on **collaborative rich-text editing**.

**How they work**:

* Use **causally ordered insertions and deletions** to represent a shared text buffer.
* Every character or operation is given a unique ID based on **causal history**.
* Designed to avoid the overhead of traditional Operational Transformation (OT).

**Key Concepts**:

* **Causal Trees**: Each edit is a node in a tree of edits, preserving causality.
* Can handle **concurrent inserts** at the same position without conflict.

**Limitations**:

* Still experimental; not production-ready.
* Complexity grows with document size (without garbage collection).
* Less mature tooling compared to Yjs or Automerge.

### Summary Comparison Table

| Tool/Project         | CRDT Type         | Key Use Case                  | Notable Strengths                 | Limitations                            |
| -------------------- | ----------------- | ----------------------------- | --------------------------------- | -------------------------------------- |
| **Yjs**              | Y-CRDT            | Rich collaborative editors    | High performance, modular         | Needs manual undo/persistence          |
| **Automerge**        | JSON-CRDT         | Local-first apps              | Full history, automatic merging   | Large memory footprint                 |
| **Matrix**           | Hybrid CRDT       | Decentralized messaging       | Decentralized, encrypted          | Not a pure CRDT, custom conflict logic |
| **Logux**            | Log-based sync    | Offline-first frontends       | Redux-compatible, optimistic UI   | Merge logic is manual                  |
| **Replicache**       | Custom sync model | High-performance offline apps | Fast sync, batching               | Proprietary, no CRDT standard          |
| **Ink/Causal Trees** | Research CRDT     | Rich text editing             | Precise causality, no OT required | Experimental, complex GC               |

## Pros and Cons of CRDTs

| Pros                                             | Cons                                                |
| ------------------------------------------------ | --------------------------------------------------- |
| Full offline support                             | Hard to implement (especially for sequences/text)   |
| Peer-to-peer friendly (no central server needed) | Metadata can grow large (especially in state-based) |
| Automatic conflict resolution                    | Limited undo/redo (no clear history)                |
| Deterministic convergence                        | More complex than traditional data types            |

## Final Takeaway

* In CRDT systems, the merge algorithm is built into the structure itself — there's no need for external coordination to resolve conflicts.
* Ideal for decentralized and collaborative experiences.
* Even CRDT-based tools don’t "replace events" — they wrap conflict resolution logic around events.
