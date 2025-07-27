# Evolution of Real-Time Data Sharing Platforms

*(1980s → 2025)*

* * *

## 1. The Early Era: File-Based and Manual Sharing (1980s–1990s)

### Characteristics

- No real-time capabilities.
- Data shared via floppy disks, email attachments, or file servers.
- Spreadsheets like **Lotus 1-2-3** and early **Excel** were purely local.

### Technologies

- SMB/NFS file sharing
- Versioning done manually or via file naming (`budget_v2_final_FINAL.xls`)
- No conflict detection or merging
- No user access control or encryption

## 2. The Networked Desktop Era (Late 1990s–2005)

### Characteristics

- Shared drives and LAN-based collaboration tools emerged.
- Microsoft Excel added **external links to other spreadsheets** (e.g., `[filename]Sheet!A1`).
- Basic **read/write file locking**, but no multi-user editing.

### Technologies

- Windows file sharing (SMB), SharePoint (basic Excel Services)
- Client-server sync logic (e.g., via polling)
- No true real-time — just **periodic refresh**
- Security = Windows ACLs or password protection (weak)

## 3. The Cloud-Based Syncing Era (2006–2012)

### Characteristics

- The shift to cloud-based collaboration tools begins.
- **Google Docs (2006)** introduces **real-time multi-user document editing**.
- Microsoft launches **Excel Web App** (later Excel Online).

### Key Technologies

- **Operational Transformation (OT)**:

  - Used in **Google Docs** to resolve concurrent edits.
  - Reorders operations to maintain consistency.
- AJAX + HTTP polling → then **WebSockets** for push-based updates

- Centralized servers host the "source of truth"

- SSL/TLS for in-transit security; encryption at rest on servers

- Still limited cross-platform or decentralized support

## 4. The Collaborative Model Formalized (2012–2018)

### Characteristics

- Rise of "end-user programming" in spreadsheets
- Recognition that spreadsheets are **programs**, not just static data
- Start of research into **fine-grained, cell-level sharing**, e.g., Maresca's *Spreadsheet Space* (2016)

### Key Innovations

- Formal models of spreadsheet computation (e.g., views, overlays)
- Experimental platforms for structured collaboration
- Partial integration with external data sources (SQL, APIs)
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

* * *

## 6. Future Directions (2025+)

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
