# OT

**Operational Transformation (OT)** is an algorithmic technique for supporting **real-time collaboration** where **multiple users edit shared content simultaneously**.

It ensures that **all users eventually see the same result**, even if they make **conflicting edits at the same time**.

## How it works

OT works by:

1. **Capturing edits as operations** (e.g., “insert ‘x’ at position 3”).
2. **Transforming operations** against each other when they arrive out of order — so they can be applied **in a way that preserves intention**.

> Example:
>
> * User A inserts “a” at position 5.
> * User B deletes character at position 4.
> * These edits are transformed based on their relative positions so both actions can coexist without corrupting the document.

## Used in:

* **Google Docs**
* Etherpad (early versions)
* Microsoft Word Online (hybrid with locking)

## Pros and Cons

| Strengths                       | Weaknesses                                         |
| ------------------------------- | -------------------------------------------------- |
| Real-time collaboration         | Complex to implement correctly                     |
| Proven in large-scale apps      | Hard to extend beyond text (e.g., structured data) |
| Supports intention preservation | Central server usually required to order ops       |

## OT vs CRDT

| Feature            | OT                                | CRDT                         |
| ------------------ | --------------------------------- | ---------------------------- |
| Ordering required? | Yes, via a central server         | No, order-independent        |
| Easy to implement? | ❌ Hard (esp. for undo, branching) | ✅ Easier for many data types |
| Used in            | Google Docs, Word                 | Yjs, Automerge               |
| Offline-friendly?  | 🚫 Not natively                   | ✅ Yes                        |
