# Google Docs Architecture: Real-Time Collaboration via OT vs. CRDTs

This article provides a concise breakdown of how Google Docs achieves real-time collaborative editing, focusing on the architectural decision to use **Operational Transform (OT)** rather than **Conflict-Free Replicated Data Types (CRDTs)**.

## Core Collaboration Challenges

- **Consistency**: All users must eventually see the same document.
- **Low latency**: Changes appear immediately.
- **Concurrency resolution**: Edits from multiple users must merge without overwriting.
- **Offline support**: Enables syncing after reconnecting.

## 1. Operational Transform (OT)

- **Used by Google Docs**: Edits are sent as operations to a central server.
- Server orders, transforms, and broadcasts operations to ensure consistent ordering.
- Example: If Alice inserts “Hello” at position 0 and Bob inserts “World” at position 5, OT transforms Bob’s operation to correct positions, producing “HelloWorld.”
  
**Pros:**

- Proven scalability at Google's scale.
- Efficient in bandwidth due to central coordination.

**Cons:**

- Requires a central server.
- Complex logic for handling concurrent edits.

## 2. CRDTs: The Alternative

- **Decentralized model**: Each user edits a local replica; changes merge later via mathematical merge functions.
- Suitable for peer-to-peer architectures and offline-first systems.

**Pros:**

- No central server needed.
- Full offline support; eventual consistency without conflict.

**Cons:**

- Higher resource usage (memory, bandwidth).
- Overhead increases with number of collaborators.

## 3. Feature Comparison

| Feature                | OT (Google Docs)                         | CRDTs (e.g. Notion, Figma)            |
|------------------------|-------------------------------------------|----------------------------------------|
| Consistency            | Strong, immediate                          | Eventual                               |
| Network dependency     | Central server                             | Peer-to-peer                          |
| Offline support        | Limited                                    | Robust                                |
| Scalability            | Excellent (centralized scaling)            | Better in decentralized environments  |
| Complexity             | Complex transform logic                    | Higher overhead per client            |

### Real-World Performance Comparisons

To compare the efficiency of OT vs. CRDTs, consider this scenario:

- **1000 concurrent users editing a document.**
- Each user makes **1 edit per second**.
- **Network latency = 100ms.**

#### Using OT

- The server processes **1000 transformations per second**.
- Network cost: **Minimal, only sends necessary transformations.**

#### Using CRDTs

- Each user syncs with **999 other users**.
- Bandwidth overhead: **Massive (O(n²) sync complexity)**.
- Memory usage: **Higher due to multiple stored versions.**

## 4. Why Google Uses OT

- **Immediate consistency** and predictability in collaborative text editing.
- Optimized for **highly structured document workflows**, like those in Docs and Sheets.

CRDTs are more attractive for **offline or decentralized use cases**, but at Google’s scale and operational model, OT remains more efficient.

## Source

S. Deray, *Google Docs Architecture: Real-Time Collaboration via OT vs CRDTs*, [sderay.com](https://sderay.com/google-docs-architecture-real-time-collaboration/), February 25, 2025
