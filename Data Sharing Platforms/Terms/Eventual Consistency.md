# Eventual Consistency

**Eventual consistency** is a consistency model used in distributed systems where, in the absence of new updates, all replicas of a given piece of data will **converge to the same value** over time.

Unlike **strong consistency**, which guarantees that all reads return the most recent write, eventual consistency allows **temporary divergence** between replicas in exchange for better **availability and performance**.

## Key Characteristics

- **Temporary inconsistency is allowed**: Replicas might see different values for a short time.
- **Convergence guarantee**: If no new updates are made, all replicas will eventually agree.
- **Asynchronous replication**: Changes propagate across nodes over time.

## Example

In a multi-region database:

- A user in Europe updates their profile.
- A user in Asia queries that profile a second later but sees the old data.
- After replication completes, the user in Asia sees the updated profile.

## Use Cases

- Distributed databases (Amazon DynamoDB, Cassandra)
- Collaborative editors with CRDTs
- Offline-first apps with delayed sync

## Comparison

| Model                 | Guarantees                                  | Trade-offs                          |
|----------------------|---------------------------------------------|-------------------------------------|
| Strong consistency   | All reads see the latest write              | Lower availability, higher latency |
| Eventual consistency | All replicas eventually agree (no timeline) | High availability, temporary inconsistency |

## Relevance to Data Sharing Platforms

Eventual consistency is often used in **real-time collaboration**, especially when:

- Systems are **decentralized or distributed**.
- Users operate **offline** and sync later.
- **Low latency** is prioritized over strict correctness.

It is commonly paired with data types like **CRDTs**, which are designed to ensure convergence even when updates arrive out of order.
