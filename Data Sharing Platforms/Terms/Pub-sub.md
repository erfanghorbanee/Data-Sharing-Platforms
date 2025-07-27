# Pub-sub

**Publish-subscribe (pub-sub)** is a **messaging pattern** where **producers (publishers)** send messages **without knowing who receives them**, and **consumers (subscribers)** get messages **without knowing who sent them**.

It’s a core pattern in **event-driven architectures**, used in real-time data sharing, microservices, and collaborative systems.

## Components

* **Publisher**: Sends messages/events (e.g., "cell A1 updated to 42").
* **Subscriber**: Registers interest in certain messages (e.g., “notify me when cell A1 changes”).
* **Broker**: Middleman that delivers messages from publishers to subscribers.

## Used in

* Spreadsheet Space (via central event manager)
* Google Firebase Realtime Database
* Apache Kafka, MQTT, Redis PubSub
* WebSocket-based collaboration tools

## Example

In a spreadsheet sync system:

* Spreadsheet A *publishes* an update to cell A1.
* Any spreadsheet *subscribed* to A1 gets notified and updates its own image/view of A1.

## Pros and Cons

| Strengths                        | Weaknesses                                   |
| -------------------------------- | -------------------------------------------- |
| Decouples senders from receivers | Requires a broker or middleware              |
| Scalable for many subscribers    | Complex error handling                       |
| Fits real-time sync well         | Security & delivery guarantees can be tricky |

## Why it matters

Pub-sub is foundational to any **event-driven synchronization system**:

* CRDTs can use pub-sub to **broadcast operations**
* OT systems use pub-sub to **send and order edits**
* Systems like **Spreadsheet Space** rely on a centralized pub-sub mechanism for **event propagation**

If you want to build a **modular or federated** data sharing platform, understanding pub-sub is crucial.
