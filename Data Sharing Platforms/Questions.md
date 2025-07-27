# Questions

## 1. is SpreadSheetSpace the same company as DocSpace s.r.l ?

It appears that **SpreadSheetSpace** and **DocSpace S.r.l.** are effectively the *same entity*—DocSpace S.r.l. is the Italian company behind the SpreadSheetSpace product. SpreadSheetSpace is a suite of Excel add‑ins (“Link”, “Sync”, “Co‑Edit”) developed and marketed by DocSpace S.r.l., based in Genova, Italy

&nbsp;

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
