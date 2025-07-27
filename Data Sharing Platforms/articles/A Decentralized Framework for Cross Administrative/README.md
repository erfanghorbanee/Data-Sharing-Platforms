# Analysis of: "A Decentralized Framework for Cross Administrative Domain Data Sharing"

**Author**: Matteo Gambetta  
**Institution**: University of Genoa (PhD Thesis)  
**Year**: 2021  
**Advisor**: Prof. Massimo Maresca  
**Available at**: <https://iris.unige.it/retrieve/e268c4cb-f929-a6b7-e053-3a05fe0adea1/phdunige_3490323.pdf>

* * *

## Overview

This thesis presents an evolved version of the **Spreadsheet Space** platform, transforming it into a **decentralized architecture** that supports **cross-domain collaboration**, **fine-grained sharing**, and **privacy-preserving synchronization**. It introduces a framework that allows spreadsheet users and software platforms to **share data securely**, across organizational boundaries, without relying on a single central authority.

* * *

## Key Concepts and Components

### 1. **Spreadsheet Space**

- A middleware system for spreadsheet-based collaboration.
- Introduces **views** (published data) and **images** (local copies).
- Originally centralized, later extended to a **federated architecture**.

### 2. **Federated Spreadsheet Space**

- Each organization runs its own **Spreadsheet Space node**.
- Nodes are coordinated via a **decentralized control plane** using protocols like **RAFT**.
- Enables **autonomous administration**, but maintains global consistency.

### 3. **View Server**

- Stores **encrypted spreadsheet views**.
- Always-on, even when users are offline.
- Makes data accessible without exposing the underlying spreadsheet.

### 4. **Control Plane Services**

- Modular services such as:
  - **Event Manager**: Coordinates updates across nodes.
  - **Sharing Manager**: Manages access control and publication.
  - **Collaborator Manager**: Handles user roles and interactions.

### 5. **Cryptographic Infrastructure**

- Uses **Public Key Infrastructure (PKI)** to authenticate users and encrypt content.
- All data exchanges between nodes are **end-to-end encrypted**.
- Servers do **not see unencrypted data** (zero-knowledge architecture).

### 6. **WideGroups**

- A model to manage **groups of users or organizations** in collaboration.
- Each WideGroup has its own control plane, views, and access policies.
- Used to organize large-scale collaborations or federations.

### 7. **Operational Phases**

- **Exposure**: Publishing a view to a set of users or WideGroup.
- **Subscription**: Target users retrieve views and create synchronized images.
- **Update propagation**: Real-time event notifications manage view/image alignment.

* * *

## Technical Architecture

- **Hybrid model**: Decentralized control + centralized persistence.
- **Event-driven updates**: Similar to publish/subscribe.
- **Client-server architecture**, but clients operate independently.
- **Manual or automatic synchronization** modes.
- **Data-level access control** (cell/table-level granularity).

* * *

## Useful Terms for Your Thesis

| Term | Definition |
| --- | --- |
| **View** | A published, read-only projection of a spreadsheet element |
| **Image** | A local, auto-synced copy of a view |
| **WideGroup** | A group-based abstraction for multi-organization collaboration |
| **Event Manager** | A component that coordinates update notifications |
| **PKI** | Public key infrastructure used for identity and encryption |
| **Spreadsheet Overlay** | A structured collaboration pattern using forms and views |
| **Federated Control Plane** | Distributed control components managed across domains |
| **Zero-Knowledge Architecture** | Servers cannot decrypt user data; only endpoints hold keys |

* * *

## Relevance to Your Thesis

This work offers a **rich architectural framework** for real-time data sharing with:

- Emphasis on **security and decentralization**
- Modular design with **pluggable components**
- Alignment with modern concerns like **cross-domain access, fine-grained control, and encryption**

### Use It For

- Inspiration on designing **modular, privacy-preserving data platforms**
- A case study in evolving from centralized to **federated architectures**
- A contrast to modern **CRDT-based real-time editors**: this system does **not handle concurrent editing or conflict resolution**

* * *

## Limitations to Note

- No CRDT or OT integration → **does not support multi-user concurrent edits**
- Still based on a **push-update model** (event propagation), not peer-to-peer diff/merge
- Privacy model depends on **infrastructure trust** in always-on view servers
- Complexity of **setup and federation** may hinder adoption in lightweight scenarios

* * *

## Suggested Sections to Read in Detail

- Chapter 3: Federated Spreadsheet Space Design
- Chapter 4: Security Infrastructure and Cryptographic Services
- Chapter 5: Collaboration Models and Overlays
- Chapter 6: Implementation and Case Studies

* * *

## Conclusion

The thesis provides a strong architectural basis for building **secure, cross-domain, spreadsheet-based data sharing platforms**. While it does not address **true real-time collaborative editing**, it contributes valuable models for **federation, access control, and synchronization** — highly relevant to the design of open, secure data sharing platforms.
