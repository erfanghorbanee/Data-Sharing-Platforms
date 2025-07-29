# OT

**Operational Transformation (OT)** is an algorithmic technique for supporting **real-time collaboration** where **multiple users edit shared content simultaneously**.  
It ensures that **all users eventually see the same result**, even if they make **conflicting edits at the same time**.

## How it works

OT works by:

1. **Capturing edits as operations** (e.g., “insert ‘x’ at position 3”).
2. **Transforming operations** against each other when they arrive out of order — so they can be applied **in a way that preserves user intent**.

> **Example:**
>
> Initial text: `"Hello World"`
>
> - **User A** wants to insert `"X"` at position **6** (after `"Hello "`).
> - **User B** wants to delete the character at position **5** (the space).
>
> **If the delete is processed first:**
>
> - Text becomes `"HelloWorld"`
> - The insert must shift from position **6** to **5**, because removing the space shortens the string.
> - Result: `"HelloXWorld"`
>
> **If the insert is processed first:**
>
> - Text becomes `"Hello XWorld"`
> - The space is still at position **5**, so the delete can proceed **unchanged**.
> - Result: `"HelloXWorld"`

**Note:**

OT does not need to globally decide “which operation comes first”.

It only needs to:

- Detect when operations are concurrent
- Use transformation functions to merge them correctly, preserving user intent

## History and Use

- **Introduced in the late 1980s**, with academic work continuing into the early 2000s.
- **Popularized in real-world apps in the 2000s–2010s**.
- **Still in use**, but **less dominant** in new systems due to the rise of CRDTs.

### Used in

- **Google Docs** (since mid-2000s; hybrid approaches evolved over time)
- **Etherpad** (2008–present in some forks)
- **Microsoft Word Online** (partially, combined with locking and other sync models)

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
| Implementation     | Difficult for undo, branching     | Simpler for many data types  |
| Used in            | Google Docs, Word Online          | Yjs, Automerge               |
| Offline support    | Not natively                      | Yes, built-in                |
