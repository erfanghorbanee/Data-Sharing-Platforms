# CRDT

**CRDT = Conflict-free Replicated Data Type**

A **CRDT** is a data structure that lets **multiple users** update shared data **at the same time**, even while offline or on separate machines, **without causing conflicts** when they sync up later.

CRDTs are especially useful for **real-time collaborative editing**, like Google Docs or shared spreadsheets.

## Why are CRDTs important?

Because in real-time systems:

- **Users can edit the same data simultaneously**.
- Data updates must eventually reach **consistency** (everyone sees the same state).
- Traditional locks or version control don’t scale well for fluid, real-time editing.

CRDTs **guarantee** that all changes:

- **Merge automatically**
- **Don’t overwrite each other**
- **Always lead to the same final result** no matter the order of updates

## A Simple Example (Counters)

Imagine a counter that three users can increment independently.

With a CRDT:

- Alice adds +2
- Bob adds +3
- Charlie adds +1

Each person’s increment is tracked **separately**, and then all updates are **merged**:  
Final result = 2 + 3 + 1 = **6**, no matter the order they synced in.

## CRDT vs Traditional Sync

| Model | Sync Behavior | Handles Conflicts? | Used in |
| --- | --- | --- | --- |
| CRDT | Merges automatically | ✅ Yes, by design | Yjs, Automerge, Matrix |
| OT (Operational Transformation) | Uses transform rules | ✅ Yes | Google Docs |
| Central sync (like Spreadsheet Space) | Triggers updates via events | ❌ No — assumes one writer at a time | Spreadsheet Space (Maresca) |

## Real-World Tools That Use CRDTs

- **Yjs** – CRDT library for collaborative apps (used in many open-source editors)
- **Automerge** – A popular CRDT library in JavaScript
- **Matrix protocol** – A decentralized messaging protocol with CRDT-based state sync
- **Logux / Replicache** – Systems with CRDT-style offline-first sync
