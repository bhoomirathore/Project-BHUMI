# B.H.U.M.I. — Product Requirements Document

## Blockchain Hosted Unified Mutation Infrastructure

**Project:** B.H.U.M.I.
**Document:** Product Requirements Document
**Version:** 1.1
**Status:** Locked for MVP — Phase 1

---

# 1. Document Purpose

This document defines the **product requirements** for B.H.U.M.I.

It specifies:

* the problem being addressed,
* the intended users,
* the product objective,
* functional requirements,
* user-facing workflows,
* MVP scope,
* product constraints,
* non-functional requirements,
* acceptance criteria,
* out-of-scope functionality,
* MVP success criteria.

Technical implementation details are defined separately in the **B.H.U.M.I. Technical Architecture Document**.

---

# 2. Product Overview

B.H.U.M.I. is a digital land transaction platform intended to connect property registration/transfer with the subsequent ownership mutation process.

The platform provides a digital workflow in which:

1. A citizen initiates a property transaction.
2. Required information and documents are submitted.
3. The transaction is verified by an authorized government official.
4. The authorized transaction is finalized.
5. The resulting ownership change is recorded in a tamper-evident history.
6. The ownership state becomes available through the system without requiring a separate manual mutation submission.

B.H.U.M.I. is intended as an enhancement/trust layer around existing digital land-record processes, not as a replacement for the legal or governmental land-record system.

---

# 3. Problem Statement

The project addresses two primary problems.

## 3.1 Record Integrity

Conventional centralized land-record systems depend heavily on controlled database access.

Unauthorized or incorrect modification of an operational record can create uncertainty regarding the integrity of historical ownership information.

B.H.U.M.I. aims to provide a tamper-evident history of finalized ownership transactions.

---

## 3.2 Registration–Mutation Disconnection

Property registration and subsequent mutation may involve separate administrative processes.

This can result in:

* duplicate data entry,
* manual follow-up,
* delays,
* fragmented records,
* unnecessary administrative effort.

B.H.U.M.I. aims to connect the finalized property transaction with the ownership mutation process.

---

## 2. Problem Statement (The "Why")
Despite advancements like NGDRS and state portals (e.g., Sampada 2.0), the ecosystem suffers from two critical, unaddressed loopholes:

1. **The "God Mode" Database Vulnerability:** Existing systems run on centralized relational databases (SQL/Oracle). A corrupt administrator or a compromised backend account can silently alter, overwrite, or delete ownership records without leaving a cryptographic trace.
2. **The Mutation (Namankan) Bottleneck:** Legal registration (at the Sub-Registrar Office) and land record mutation (at the Revenue Office/Patwari) are disjointed. The physical transfer of files between these departments causes massive delays, pendency, and opens the door for corruption.

---

## 3. The Proposed Solution (The "What")
B.H.U.M.I. does not replace existing e-registries; it upgrades them with a **Hybrid Web3 Trust Layer**.
* **Off-Chain (Speed & Privacy):** Traditional database (PostgreSQL) handles fast queries, user sessions, encrypted document storage, and fiat payments.
* **On-Chain (Immutability):** A permissioned Smart Contract (Solidity) stores the SHA-256 cryptographic hashes of property documents and records the immutable ownership transfer event.
* **The Magic Trigger:** The moment a Registrar clicks "Approve", the Smart Contract logs the transaction, which simultaneously triggers the backend to update the current owner in the database—executing Registry and Mutation in exactly **one step**.

---

## 4. Target Audience & User Personas

### Persona A: The Citizen (Buyer/Seller)
* **Needs:** Wants a simple, fast way to buy/sell land without dealing with Patwari bribes or understanding cryptocurrency.
* **Experience:** Pure Web2 interface. Logs in via standard auth, uploads KYC/Sale Deeds, pays fees in INR via standard payment gateways (Razorpay/UPI). **No MetaMask required.**

### Persona B: The Government Official (Registrar)
* **Needs:** Needs to verify documents and execute legal transfers securely.
* **Experience:** Uses an official dashboard to review applications. Signs transactions using authorized digital keys. 

### Persona C: The Public / Auditor
* **Needs:** Verify who actually owns a piece of land to prevent buying disputed property.
* **Experience:** Accesses a "Public Ledger" search bar. Enters a Property ID to see the full, immutable history of ownership transfers and status.

---

## 5. Core Features (MVP Scope)

| Feature Module | Description |
| :--- | :--- |
| **Two-Tier Authentication** | Separate dashboards and access controls for Citizens and Government Officials. |
| **Document Hashing Engine** | Backend service that generates a unique `SHA-256` hash for every uploaded KYC and property document to prevent tampering. |
| **Smart Contract RBAC** | Strict Role-Based Access Control in Solidity. Only authorized backend/Registrar wallets can write to the blockchain. Users cannot directly hit the contract. |
| **1-Click Mutation workflow** | Automated sync between the Smart Contract transaction success event and the PostgreSQL `Current Owner` field update. |
| **Fiat Payment Simulation** | Mocking a standard INR checkout flow for registry fees, proving citizens don't need to pay blockchain Gas fees directly. |

---

## 6. Out of Scope (Non-Goals for V1)
To ensure we deliver a flawless working prototype, the following features are strictly out of scope for the MVP:
* Complex Dispute/Freeze mechanisms in the Smart Contract.
* Real Government eKYC / Aadhaar API integration (we will use mock verification).
* Live banking APIs for actual E-Stamping.
* AI-based OCR for document reading.

---

## 7. Success Metrics
The MVP will be considered successful if it can demonstrate:
1. A citizen successfully submitting a transfer request.
2. A registrar approving it.
3. The system generating a SHA-256 hash, pushing it to a local blockchain testnet, and instantly reflecting the new owner on the public ledger interface without manual database editing.