# Questions

## 1. is SpreadSheetSpace the same company as DocSpace s.r.l ?

It appears that **SpreadSheetSpace** and **DocSpace S.r.l.** are effectively the *same entity*—DocSpace S.r.l. is the Italian company behind the SpreadSheetSpace product. SpreadSheetSpace is a suite of Excel add‑ins (“Link”, “Sync”, “Co‑Edit”) developed and marketed by DocSpace S.r.l., based in Genova, Italy

## 2. Has Spreadsheet Space Improved Since 2016?

Yes, the **Spreadsheet Space** architecture has seen some updates since 2016, particularly in terms of **availability**, **federation**, and **privacy controls**. However, it still lacks key capabilities expected in modern real-time data sharing platforms, such as **conflict-free real-time collaboration**, **offline editing with merge**, and **peer-to-peer decentralization**.

### Improvements Since 2016

#### 1. **Federated Control Plane**

- The control logic (event manager, sharing manager) is now **distributed** across multiple domains using **RAFT consensus** or **WideGroups**, instead of relying on a single server.

#### 2. **Private View Servers**

- Enterprises and users can run their own always-on **view servers**, improving availability and reducing reliance on central infrastructure.

#### 3. **Stronger Encryption and Privacy**

- Adoption of **PKI (Public Key Infrastructure)** for managing keys and authentication.
- Control-plane components do **not store or process decrypted spreadsheet content**, supporting a **zero-knowledge architecture**.

### Limitations That Remain

#### 1. **No Conflict-Free Real-Time Collaboration**

- Still uses **event-based unidirectional sync** (view → image).
- Does **not support concurrent editing** or **merge resolution** (unlike CRDT or OT-based systems).

#### 2. **No Offline Editing or Convergence**

- Does **not support offline-first workflows**.
- Users cannot make edits offline and later merge them safely.

#### 3. **Spreadsheet-Centric Design**

- The system is still built around traditional spreadsheet models (cells, formulas), with limited generalization to structured or streaming data types.

### Conclusion

While **Spreadsheet Space** has evolved in terms of decentralization and privacy infrastructure, it still does **not meet modern expectations** for real-time, multi-user, conflict-free collaboration. It remains more suited for structured data sharing in **coordinated workflows**, not fluid, decentralized editing environments.

### Reference

University of Genoa PhD Thesis (2021):  
**"Real-Time Collaboration in the Spreadsheet Space"**  
Available at: <https://iris.unige.it/retrieve/e268c4cb-f929-a6b7-e053-3a05fe0adea1/phdunige_3490323.pdf>

## 3. Are CRDT, OT, and other modern models still using events under the hood?

Yes — **CRDT-based**, **OT-based**, and even newer synchronization models are still **event-driven at the transport layer**. What differentiates them is **how they handle concurrent updates**, not whether they use events.

- **All models rely on events** to propagate changes (e.g., via WebSockets, WebRTC, or Matrix).
- **CRDTs** merge updates deterministically using logic embedded in the data structure itself.
- **OT systems** use transformation rules to ensure consistency, often requiring a centralized server.
- **Naïve event-push systems** (like early Spreadsheet Space) do not handle conflicts and assume one writer at a time.

> In modern systems, **event delivery and conflict resolution are separated**. Event-based transport is universal — the key difference is whether the system has a **merge-aware layer (like CRDT or OT)** on top of those events.

## 4. Why Do CRDTs Have Limited Undo/Redo (No Clear History)?

### 1. No Clear Global History

CRDTs do **not maintain a global total order of operations**, only **partial causal orderings** (e.g., using vector clocks or Lamport timestamps).

- Undo/redo mechanisms usually depend on a **linear, user-visible history** (e.g., "undo last delete").
- CRDTs allow concurrent edits and **merge** them, which makes it unclear what should be undone — and in what order.

### 2. Operations Are Not Reversible

CRDT operations are:

- **Commutative** (order doesn’t matter),
- **Idempotent** (safe to reapply),
- **Associative** (grouping doesn’t matter),

But they are **not inherently reversible**:

- An operation like `add("X")` may not retain enough metadata to correctly define `remove("X")` as an inverse.
- No general guarantee exists that inverse operations will cleanly revert previous states, especially across replicas.

### 3. Limited Intent Preservation

Undo in collaborative apps often means **reverting the last action *you* performed**.

- CRDTs don’t track **who performed which operation**, unless explicitly extended.
- Without **user attribution** or session-specific metadata, it’s hard to implement **intent-based undo**.

### 4. Workarounds Require Explicit History

Some approaches to approximate undo/redo:

- **Operational logging**: Keep a separate log of user actions (like in Operational Transformation-based systems) — but this adds overhead and partially defeats CRDTs’ simplicity.

- **CRDT + version vectors**: You can snapshot state and allow undo/redo by replaying or rolling back — but this still lacks fine-grained, user-intent-preserving control.

- **Session-based undo**: Some CRDT implementations support per-session or per-user undo stacks — but this requires auxiliary metadata and custom logic.

## 5. Can the Fediverse be used for spreadsheet or table sharing?

Is it technically feasible to extend the Fediverse—particularly the ActivityPub protocol—for decentralized spreadsheet or tabular data sharing? What would be required to achieve near-real-time collaboration, including synchronization, access control, and conflict resolution? Have any prototypes or implementations been attempted, and what are the limitations or trade-offs of adapting a social-network-oriented protocol for structured collaborative data editing?

In principle, yes—the Fediverse could be extended to support spreadsheet or table sharing, but no widely adopted implementation currently exists. The core Fediverse protocol, **ActivityPub**, is flexible and allows for custom object types and activity definitions. This means it is technically possible to define spreadsheet-like data models (e.g., `Spreadsheet`, `SheetCell`, `SheetEdit`) and corresponding actions (`UpdateCell`, `AddRow`, etc.) for communication between actors (users or services).

However, ActivityPub is inherently designed for **eventual consistency**, not real-time collaboration. It is optimized for social media-like workloads—posts, likes, follows—not for high-frequency data updates such as those generated by spreadsheet edits. There is also **no built-in conflict resolution**, unlike Operational Transforms (OT) or Conflict-Free Replicated Data Types (CRDTs) used in collaborative editors like Google Sheets or CodeMirror-based tools.

As of 2025, there are **no known mainstream Fediverse projects that support real-time table or spreadsheet collaboration**. Adjacent efforts include:

- **ForgeFed**: Extends ActivityPub for federated Git forges (e.g., Gitea) but focuses on source code and issue tracking, not structured document editing.
- **Fission**, **IPFS**, and **Solid**: Explore decentralized identity and storage, with potential for collaborative apps, but these are not ActivityPub-based or spreadsheet-specific.
- **Cabal** and **Hypercore**: Offer peer-to-peer synchronization with CRDTs, suggesting a more suitable foundation for real-time collaboration, albeit outside the Fediverse.
- **Nextcloud Office** (Collabora Online): Supports collaborative editing but is centralized and does not use ActivityPub.

To build a spreadsheet tool on the Fediverse, a viable architecture might include:

- **ActivityPub for identity, permissions, and change notifications**;
- **CRDTs (e.g., Automerge, Yjs)** for local-first, conflict-free data merging;
- **WebRTC**, **WebSockets**, or **IPFS** for high-performance, real-time peer-to-peer sync;
- A UI layer that represents spreadsheet data and applies remote patches in real time;
- **End-to-end encryption** for protecting data in transit and at rest.

**Trade-offs** include:

| Aspect | Limitation |
|--------|------------|
| Real-time sync | ActivityPub is not optimized for low-latency updates. |
| Data conflict resolution | Would require additional logic (e.g. CRDT layer). |
| Storage and hosting | Not standardized in AP; would need integration with external storage (e.g., IPFS, Solid pods). |
| Access control | AP supports basic visibility settings, but fine-grained ACLs are missing. |
| Adoption | New object types and interaction models would need community and tool support. |

In summary, while the Fediverse provides a promising social and architectural foundation for decentralized collaboration, adapting it for structured, real-time table editing would require substantial protocol extensions and auxiliary systems. No such platform currently exists, but the concept is viable and represents an open area for future research or prototyping.
