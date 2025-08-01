# Evolution of Real-Time Data Sharing Platforms

*(1980s → 2025)*

## 1. The Early Era: File-Based and Manual Sharing (1980s–1990s)

### Characteristics

- No real-time capabilities.
- Data shared via floppy disks, email attachments, or file servers.
- Spreadsheets like **Lotus 1-2-3** and early **Excel** were purely local.

### Key Technologies

- SMB/NFS file sharing
- Versioning done manually or via file naming (`budget_v2_final_FINAL.xls`)
- No conflict detection or merging
- No user access control or encryption

## 2. The Networked Desktop Era (Late 1990s–2005)

### Characteristics

- Shared drives and LAN-based collaboration tools emerged in offices.
- Microsoft Excel introduced **external links to other spreadsheets**, enabling formulas like `[filename]Sheet!A1` to reference data in other workbooks.
- Basic **read/write file locking** helped prevent simultaneous editing conflicts. Users had to take turns editing shared files.
- Users had to manually open linked files to ensure data was up to date, since Excel couldn't recalculate values from closed external workbooks.

### Key Technologies

- Windows file sharing using **SMB protocol** (e.g., `\\server\folder\file.xlsx`)
- Early versions of **SharePoint** (with rudimentary Excel Services)
- Client-server sync via **polling or periodic refresh**, not live syncing. polling means the client asks the server for updates at intervals.
- **Security** based on Windows ACLs or basic password protection

### Limitations

- No real-time collaboration or simultaneous editing
- External links were fragile across systems or renamed files
- No cloud-based access; file sharing required local network or VPN

## 3. The Cloud-Based Syncing Era (2006–2012)

### Characteristics

- The shift to cloud-based collaboration tools begins.
- **Google Docs (2006)** introduces **real-time multi-user document editing**.
- Microsoft launches **Excel Web App** (later Excel Online).

### Key Technologies

- **Operational Transformation (OT)** to resolve concurrent edits
- AJAX + HTTP polling → then **WebSockets** for push-based updates
  - In early collaborative tools (like early Google Docs), the browser would poll the server every few seconds using AJAX to check for updates.
  - With later upgrade, WebSockets allowed the server to push updates directly to the client in real-time, instead of waiting for the client to ask.
- Centralized servers host the "source of truth"
- SSL/TLS for in-transit security; encryption at rest on servers

### Limitations

- **Polling inefficiency**: Early AJAX-based polling caused high latency and unnecessary network traffic.
- **Centralized control**: All data stored and managed by a single provider. Server outages could disrupt all users.
- **Encryption at rest is limited**: It protects stored data from physical breaches, but not from the service provider, who typically holds the decryption keys and can access the data.
- **No peer-to-peer collaboration**: All sync routed through central servers, no direct device-to-device sharing.
- **Cross-platform inconsistencies**: Browser support varied; mobile and desktop integration was weak.

## 4. Engineering Collaboration and Modularity in Spreadsheets (2012–2018)

### Characteristics

- Rise of "end-user programming" in spreadsheets
- Start of research into **cell-level sharing**, e.g., Maresca's *Spreadsheet Space* (2016)
  - Publishing a specific cell or range as a first-class, secure, shareable object
  - The ability to control who can see/edit that cell or view, even if they can’t access the full spreadsheet
  - Supporting event-driven or real-time sync of that cell, not just static reference
  - Decoupling the data reference from file paths or storage locations
  - Formal models of spreadsheet computation (e.g., views, overlays)
- Partial integration with external data sources (SQL, APIs)
- Big companies focused on making real-time collaboration work at scale (support millions of users)

#### Limitations

- Still **centralized** in architecture and tied to specific ecosystems

## 5. Decentralization and Conflict-Free Collaboration (2018–Present)

### Characteristics

- Introduction of **CRDTs (Conflict-free Replicated Data Types)** and **peer-to-peer protocols** for real-time sync without a central server.
- Users can go **offline**, edit independently, and **auto-merge** later.

### Key Technologies

- **CRDTs** (used in [Yjs](https://yjs.dev/), [Automerge](https://automerge.org/))
- **IPFS** / **libp2p** for decentralized data exchange
- **Matrix protocol** (decentralized messaging with CRDT state sync)
- **End-to-end encryption** + **zero-knowledge architectures** (e.g., [CryptPad](https://cryptpad.org/))
- Real-time data sync libraries: [Replicache](https://replicache.dev/), [Logux](https://logux.io/)

### What's different

- No single point of failure
- No trust in the server — encryption is **client-side**
- Rich access control + identity mechanisms (e.g., DIDs, OAuth2, WebAuthn)

## Modern System Architectures

Most real-time collaborative tools today combine:

- **Event delivery** (e.g., via WebSockets, WebRTC, Matrix, or pub-sub)
- **Merge-aware sync logic** (CRDT or OT)
- **Optional central coordination** for user identity, presence, and document hosting

## Future Directions

### Likely Trends

- **Federated data sharing** (e.g., cross-org collaboration without shared cloud)
- Integration of **verifiable credentials**, auditability, and **provable access rights**
- **Composable data layers**: Data is shared through schemas, APIs, and linked views
- **Zero-trust platforms**: Total separation of compute and storage from access

### Research frontiers

- Privacy-preserving computation (e.g., **FHE**, **MPC**, **ZK-proofs**)
- Real-time + large-scale: syncing thousands/millions of users efficiently
- Real-time computation over **graph and streaming data**, not just tables

## Summary Table

| Era | Key Tech | Sync Model | Conflict Handling | Security | Architecture |
| --- | --- | --- | --- | --- | --- |
| 1980–1995 | File sharing | Manual | Manual | None | Local |
| 1995–2005 | SMB/SharePoint | Polling, locks | Locks | ACLs | LAN-based |
| 2006–2012 | Google Docs, OT | Server-push | OT  | SSL/TLS | Centralized cloud |
| 2012–2018 | Spreadsheet Space, Excel Online | Event-based | None | Basic E2EE | Semi-centralized |
| 2018–2024 | Yjs, IPFS, CRDTs | P2P / Push-Pull | Automatic (CRDT) | Client-side E2EE | Decentralized |
| 2025+ | FHE, Verifiable Sync | Federated / zero-trust | Cryptographic | Zero-knowledge | Composable / Distributed |

## References and Resources

This timeline and analysis is based on a combination of academic research papers, open-source project documentation, and historical documentation of key technologies. Below is a list of the primary resources and references used:

### Academic Sources

1. **Maresca, M. (2016)**  
   *The Spreadsheet Space: Eliminating the Boundaries of Data Cross-Referencing*  
   IEEE Computer, Sept. 2016.  
   DOI: 10.1109/MC.2016.289

2. **Gambetta, M. (2021)**  
   *A Decentralized Framework for Cross Administrative Domain Data Sharing*  
   PhD Thesis, University of Genoa.  
   [Link](https://iris.unige.it/retrieve/e268c4cb-f929-a6b7-e053-3a05fe0adea1/phdunige_3490323.pdf)

3. **Shapiro, M., et al. (2011)**  
   *A comprehensive study of CRDTs*  
   Research Report RR-7506, INRIA.  
   [Link](https://hal.inria.fr/inria-00555588)

4. **Sun, C., et al. (1998)**  
   *Operational Transformation for Collaborative Internet Editing*  
   Proc. CSCW '98.  
   DOI: 10.1145/289444.289469

### Open Source and Technical Documentation

5. **Yjs – CRDT Framework**  
   [https://yjs.dev](https://yjs.dev)

6. **Automerge – JSON CRDT Library**  
   [https://automerge.org](https://automerge.org)

7. **CryptPad – End-to-End Encrypted Collaboration**  
   [https://cryptpad.org](https://cryptpad.org)

8. **Replicache – Real-time Sync Engine**  
   [https://replicache.dev](https://replicache.dev)

9. **Logux – CRDT-Based Sync over WebSockets**  
    [https://logux.io](https://logux.io)

10. **IPFS and libp2p – Decentralized Web Protocols**  
    [https://ipfs.io](https://ipfs.io)  
    [https://libp2p.io](https://libp2p.io)

11. **Matrix Protocol – Decentralized Messaging**  
    [https://matrix.org](https://matrix.org)

### Security and Future Tech

12. **zk-SNARKs and Zero-Knowledge Proofs**  
    Zcash Foundation & Electric Coin Company  
    [https://z.cash/technology/](https://z.cash/technology/)

13. **Homomorphic Encryption and FHE Libraries**  
    Microsoft SEAL: [https://www.microsoft.com/en-us/research/project/microsoft-seal/](https://www.microsoft.com/en-us/research/project/microsoft-seal/)

14. **MPC & Verifiable Computation**  
    Tutorials from Crypto 101, Stanford CS251, and relevant literature from IACR.
