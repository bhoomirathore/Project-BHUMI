# B.H.U.M.I. – Product Requirements Document (PRD)

## 1. Product Overview

### Product Name
B.H.U.M.I. (Blockchain Hosted Unified Mutation Infrastructure)

### Product Vision
B.H.U.M.I. helps bridge the gap between traditional government land registries and decentralized trust layers. It provides a hybrid Web2 + Web3 platform to create tamper-evident property records and automates the notoriously delayed mutation (*Namankan*) workflow. 

The goal is to provide citizens with a seamless, Web2-like experience (using INR payments) while leveraging a permissioned EVM smart contract in the background to ensure absolute cryptographic auditability.

---

## 2. Problem Statement

Existing digital land registry systems (like NGDRS or state portals) have digitized paperwork but face three major architectural and operational challenges:

1. **The "God Mode" Database Vulnerability:** Existing platforms rely exclusively on centralized databases (e.g., PostgreSQL/Oracle). A compromised admin account can silently alter ownership records without leaving a mathematically verifiable audit trail.
2. **Fragmented Workflows (Registry vs. Mutation):** Legal registration (Sub-Registrar Office) and land record mutation (Revenue Office) are disjointed. Manual file transfers between departments cause massive pendency and corruption.
3. **Document Tampering:** Physical and digital documents can be swapped or forged post-registration.

B.H.U.M.I. bridges this gap by adding a blockchain-backed trust layer that immutably records authorized transactions and uses smart contract events to instantly trigger the downstream mutation in the application database.

---

## 3. Target Audience

### Primary Audience

#### Citizens (Buyers & Sellers)
Everyday users who want to safely transfer property ownership without dealing with Patwari bottlenecks or understanding cryptocurrency wallets.

#### Government Officials (Registrars)
Authorized state officials who need a secure dashboard to verify KYC, validate documents, and digitally sign legal ownership transfers on a blockchain ledger.

### User Characteristics
* **Citizens:** Non-technical, prefer INR (fiat) payments, mobile-first users, require simple tracking dashboards.
* **Registrars:** Require high-security portals, will use Web3 wallets (e.g., MetaMask) tied to their official identity to sign transactions, need clear verification workflows.

---

## 4. Product Goals

B.H.U.M.I. should help users and the system:

### Verify (Integrity, not just Data)
Ensure that every property document's exact state is locked via a `SHA-256` hash on the blockchain.

### Automate
Eliminate the manual Patwari approval by making the Smart Contract's `TransferFinalized` event the direct trigger for database mutation.

### Audit
Maintain an append-only, tamper-evident history of who owned what and when, accessible to public auditors.

---

## 5. Success Metrics

### User Engagement & Efficiency
* **Transaction Time:** Reduction in time from Registry to Mutation (Target: Instantaneous upon blockchain finalization).
* **Assessment Completion:** 100% success rate in citizens completing the upload and INR payment flow.

### Technical Success (Crucial for MVP)
* **Reconciliation Accuracy:** 100% synchronization between the PostgreSQL application state and the Blockchain ledger state.
* **Smart Contract Security:** Zero authorization bugs (e.g., citizens failing to bypass Registrar-only functions).
* **Gas Abstraction:** 100% of blockchain gas fees successfully abstracted away from the citizen.

---

## 6. Core User Journey

### Step 1: Landing & Initiation (Citizen)
* Citizen logs into the B.H.U.M.I. portal via OTP/Standard Auth.
* Navigates to "My Properties" and initiates a transfer request.
* Enters the Buyer's ID and uploads the Sale Deed and KYC documents.
* Pays the registration fee in INR via a standard Payment Gateway.

### Step 2: Document Hashing (Backend)
* The Node.js backend receives the files and stores them in secure off-chain storage.
* The backend generates a strict `SHA-256` hash for the documents (e.g., `8e7d...a93f`).
* A pending application is created in the PostgreSQL database.

### Step 3: Verification & Signing (Registrar)
* Registrar logs into the Government Dashboard and reviews the pending application.
* Registrar connects their authorized Web3 Wallet (MetaMask).
* Upon clicking "Approve", the Registrar signs the transaction `[PropertyID, BuyerAddress, DocumentHash]`, pushing it to the Smart Contract.

### Step 4: Automated Mutation (System)
* The Smart Contract verifies the Registrar's role (RBAC) and records the transfer immutably.
* The Contract emits a `TransferFinalized` event.
* The Node.js Event Listener catches this event and automatically updates the `current_owner` in the PostgreSQL database.

### Step 5: Ledger View (Public/Auditor)
* User searches a Property ID.
* System displays the current owner (from DB) and the immutable chronological transfer history (from Blockchain).

---

## 7. Feature Prioritization

### Must-Have Features (MVP)

#### 1. Hybrid Storage Engine
* **Purpose:** Separate heavy data from blockchain state.
* **Requirements:** PostgreSQL for user profiles, UI state, and off-chain document storage.

#### 2. Document Cryptographic Hashing
* **Purpose:** Prove document integrity without leaking private data on-chain.
* **Requirements:** Node.js utility to generate `SHA-256` hashes for all uploaded PDFs/Images before smart contract interaction.

#### 3. Role-Controlled Smart Contract (EVM)
* **Purpose:** Handle the core state transition securely.
* **Requirements:** Minimal Solidity contract with `registerProperty()`, `transferOwnership()`, and strict `onlyRegistrar` modifiers. 

#### 4. Dual-State Reconciliation (Event Listener + Fallback)
* **Purpose:** Keep DB and Blockchain in sync.
* **Requirements:** WebSockets/Ethers.js listener to update DB upon smart contract events, PLUS a fallback API script to periodically compare and repair DB state against the ledger.

#### 5. Registrar Wallet Binding
* **Purpose:** Prevent unauthorized blockchain signatures.
* **Requirements:** Map the Registrar's internal `user_id` strictly to a predefined blockchain wallet address.

---

## 8. Nice-to-Have Features
*These will be considered strictly after MVP completion.*

* 5-Tier Role System (Adding Revenue Officers, Auditors, Buyers).
* E-KYC Aadhaar API integration.
* AI-based OCR for automated document pre-verification.
* Complex Smart Contract dispute/freeze mechanisms.
* Multi-node consortium deployment (Hyperledger/Polygon).

---

## 9. MVP Scope

**Version 1 will include:**
- [x] Citizen & Registrar Dashboards (React)
- [x] PostgreSQL Database Schema (Users, Properties, Transfers)
- [x] Node.js REST APIs + Hashing Engine
- [x] Solidity Smart Contract (Hardhat Local Testnet)
- [x] Registrar MetaMask Wallet Integration
- [x] Event Listener for Automated Mutation
- [x] Mock INR Payment Flow

*Everything else is excluded from the MVP.*

---

## 10. Non-Goals (Deliberately Not Building)

**To maintain focus and technical honesty, the following are strictly OUT of scope:**
* ❌ **Establishing Legal Ownership:** Blockchain does not guarantee legal ownership; it only records the *authorized transaction history*. The government remains the legal authority.
* ❌ **Preventing Fraudulent Uploads:** Blockchain ensures a document hasn't been altered *after* hashing. It cannot prevent a registrar from approving a physically fake document ("Garbage in, immutable garbage out").
* ❌ **Full Permissioned Network Setup:** We are simulating a permissioned environment using EVM RBAC, not deploying a full Hyperledger fabric network.
* ❌ **Cryptocurrency Payments:** Citizens will never interact with tokens, gas, or wallets. 

---

## 11. Design Principles

### Backend-First Security
Frontend applications must never communicate directly with the Smart Contract to write data. All data must pass through the Node.js backend for validation and hashing before being signed by an authorized entity.

### Fallback over Assumption
Never assume an emitted blockchain event successfully updated the database. Always design reconciliation APIs to handle Node crashes or missed blocks.

### Simplicity in Solidity
Keep the Smart Contract "boring". Do not put complex business logic, document storage, or string parsing in Solidity. The contract only handles authorization checks and state transitions.

---

## 12. Product Positioning

B.H.U.M.I. is not a replacement for government databases. 

B.H.U.M.I. is a **security and automation upgrade** that helps governments:
1. Make their existing land records tamper-evident.
2. Provide an unquestionable audit trail.
3. Automatically mutate land records the second a registry is legally approved, cutting out bureaucratic delays.