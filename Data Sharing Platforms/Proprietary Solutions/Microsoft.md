# 🧩 Microsoft Excel Co‑Authoring Architecture: Synchronization Model

Microsoft Excel's real-time co-authoring—whether in Excel for the web or desktop via OneDrive/SharePoint—relies on a **centralized, server-mediated architecture**.

## ✏️ Synchronization Technique: **Operational Transformation (OT)**

Microsoft does not officially reveal the exact algorithm used for conflict resolution in Excel or Office 365. However, research and expert sources suggest that **Operational Transformation (OT)** is the main method behind Microsoft’s co-authoring system.

## 🧪 Supporting Evidence

* A peer-reviewed analysis comparing OT and CRDT explicitly states:

  > “OT has been successfully applied in many industrial real-time co-editors including Google Docs, Etherpad, and Microsoft co-authoring systems”

  [Real Differences between OT and CRDT for Co-Editors*, Sun et al. (2019)](https://arxiv.org/abs/1810.02137)

* Microsoft’s official documentation outlines a **centralized, autosave-based, versioned co-authoring model**, which is consistent with OT-style architecture
[Microsoft Excel Co-Authoring Support Page](https://support.microsoft.com/en-us/office/collaborate-on-excel-workbooks-at-the-same-time-with-co-authoring-7152aa8b-b791-414c-a3bb-3024e46fb104)

* A detailed engineering breakdown of Excel Web App's early real-time OT-like architecture  
  [Real-time co-authoring in the Excel Web App: Why and how we did it (Microsoft, 2013)](https://www.microsoft.com/en-us/microsoft-365/blog/2013/11/19/real-time-co-authoring-in-the-excel-web-app-why-and-how-we-did-it/)

## Architectural Features

| Feature                        | Description                                                                |
| ------------------------------ | -------------------------------------------------------------------------- |
| **Centralized Sync**           | Edits are routed through SharePoint/OneDrive cloud services                |
| **Sequential Operation Log**   | Central server applies operations                          |
| **Autosave + Lock-Free**       | Changes are auto-saved, and multiple users can edit without explicit locks |
| **Operational Transformation** | Likely used to resolve concurrent edits and maintain document consistency  |
