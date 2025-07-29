# Data Sharing Platforms – Knowledge Base

This repository contains a structured collection of concepts, summaries, and annotated research related to the design and evolution of real-time, secure, and decentralized data sharing platforms. It is organized to support thesis development and research workflows.

## 1. Evolution

- [Evolution of Real-Time Data Sharing Platforms](./Data%20Sharing%20Platforms/Evolution%20of%20Real-Time%20Data%20Sharing%20Platforms.md)
A historical and technical overview of how collaborative data systems evolved from file-based sharing to cloud platforms and decentralized CRDT-based architectures.

## 2. Core Technical Concepts

- [CRDT](./Data%20Sharing%20Platforms/Terms/CRDT.md)  
- [OT (Operational Transformation)](./Data%20Sharing%20Platforms/Terms/OT.md)  
- [Pub-sub (Publish-Subscribe Model)](./Data%20Sharing%20Platforms/Terms/Pub-sub.md)  
- [Eventual Consistency](./Data%20Sharing%20Platforms/Terms/Eventual%20Consistency.md)  
- [Zero-Knowledge Proofs](./Data%20Sharing%20Platforms/Terms/Zero-Knowledge%20Proofs.md)  

## 3. Articles and Critical Analyses

Detailed, critically reviewed summaries of key papers relevant to my thesis.

- [The Spreadsheet Space](./Data%20Sharing%20Platforms/articles/The%20Spreadsheet%20Space)  
- [A Decentralized Framework for Cross Administrative Domain Data Sharing](./Data%20Sharing%20Platforms/articles/A%20Decentralized%20Framework%20for%20Cross%20Administrative)  
PhD thesis by Gambetta, extending Spreadsheet Space into a federated, zero-knowledge architecture.

## 4. To-Do: Research and Comparison Planning

### Modern Articles to Read

- A recent research article on advanced real-time table-sharing using CRDT or decentralized frameworks (e.g., “PS‑CRDTs: CRDTs in highly volatile environments”)  
  [Frontiers in Blockchain](https://www.frontiersin.org/journals/blockchain/articles/10.3389/fbloc.2020.497985/full)  
- A case study of real-time collaborative table editing using modern distributed sync techniques  
  [MDPI IJGI](https://www.mdpi.com/2220-9964/13/12/441)

### Company Protocol and Architecture Research

#### Google (Docs / Sheets)

- Real-time sync using OT and microservice architecture  
  [Google Docs Architecture Analysis](https://sderay.com/google-docs-architecture-real-time-collaboration/)

#### Microsoft (Excel Online)

- Real-time co-authoring system integrated with SharePoint and OneDrive  
  [Excel Web App Co-Authoring](https://www.microsoft.com/en-us/microsoft-365/blog/2013/11/19/real-time-co-authoring-in-the-excel-web-app-why-and-how-we-did-it/)

#### Amazon (AWS)

- Amazon Redshift and AWS Data Exchange for secure large-scale table sharing  
  [Optimizing Data Sharing on AWS](https://www.cloudthat.com/resources/blog/optimizing-data-sharing-strategies-with-amazon-redshift-data-sharing/)

### Comparison

Compare the above companies' approaches in terms of:

- Sync model (e.g., OT vs event-driven vs CRDT)
- Infrastructure design (e.g., central, distributed, peer-to-peer)
- Encryption and access control
- Scalability and latency
- Domain applicability (e.g., enterprise, public cloud, collaborative tooling)

### Matrix Protocol for Table Data Sharing

- Peer-to-peer shared editing using Matrix with extensible structured data  
  [ResearchGate Article](https://www.researchgate.net/publication/310212186_Near_Real-Time_Peer-to-Peer_Shared_Editing_on_Extensible_Data_Types)  
- Protocol analysis of Matrix in real-time collaborative data systems  
  [ScienceDirect Paper](https://www.sciencedirect.com/science/article/pii/S2666281721000159)

### Blockchain in Table-Based Data Sharing

- IPFS and smart contract-based structured data publishing  
  [MDPI Applied Sciences](https://www.mdpi.com/2076-3417/14/16/6940)  
- User-controlled, auditable data-sharing over blockchain networks  
  [Frontiers in Blockchain](https://www.frontiersin.org/journals/blockchain/articles/10.3389/fbloc.2020.497985/full)

### LibreOffice: Real-Time Collaboration Status and Research Directions

Investigate how LibreOffice handles collaboration and data sharing:

- **Collabora Online** is the main pathway for real-time collaborative editing in LibreOffice. It provides web-based spreadsheet and document co-authoring using a client-server model and supports integration with distributed platforms like Nextcloud Global Scale.
  - Collabora enables collaborative spreadsheet editing, but depends on server-side infrastructure and is not natively integrated into LibreOffice desktop applications.

- **LibreOffice Desktop (Calc/Writer)** does **not currently support real-time collaboration**. However, there are **experimental proposals** to support **peer-to-peer collaboration** using networks like **Jami** and **DHTNet**.
  - These proposals aim to remove server dependence and provide privacy-preserving, offline-capable collaboration, but they are not yet production-ready.

#### Key Articles and Sources

- [Collaborative Editing – LibreOffice Wiki](https://wiki.documentfoundation.org/Collaborative_Editing)
- [Peer-to-Peer Collaboration Design Proposal (2024)](https://design.blog.documentfoundation.org/2024/07/17/peer-to-peer-collaboration-with-libreoffice/)
- [LibreOffice Online Overview](https://wiki.documentfoundation.org/Development/LibreOffice_Online)
- [Collabora + Nextcloud Scale Collaboration Case Study](https://www.collaboraonline.com/blog/nextcloud-and-collabora-scale-real-time-collaboration-to-hundreds-of-millions-of-users/)

#### Suggested Actions

- Evaluate the architecture of Collabora Online and how it compares to Google Docs, Excel Online, and CRDT-based systems.
- Analyze why LibreOffice has not integrated CRDT or OT models into its desktop suite.
- Track developments in the peer-to-peer collaboration initiative and consider how it could influence decentralized data sharing architectures.

This case will help identify the limitations of retrofitting real-time collaboration into large, traditional office suites and the potential role of open-source projects in pioneering decentralized collaboration.
