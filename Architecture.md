# TECHNICAL ARCHITECTURE DOCUMENT

## PROJECT B.H.U.M.I.

### Blockchain Hosted Unified Mutation Infrastructure

**Project Type:** College Minor Project
**Architecture Type:** Hybrid Web2 + Web3 Land Registry Platform
**Primary Domain:** Government / Land Registry / Property Management
**Version:** 1.0

---

# 1. Introduction

B.H.U.M.I. (Blockchain Hosted Unified Mutation Infrastructure) is a government-oriented hybrid Web2 + Web3 digital land registry platform designed to provide transparent, tamper-evident, and auditable property ownership records.

The platform combines conventional web technologies, relational/document databases, secure document storage, payment infrastructure, government verification workflows, cryptographic hashing, and blockchain technology.

The primary purpose of B.H.U.M.I. is **not to replace existing government land databases**. Instead, blockchain acts as an additional verification and audit layer for verified property records.

The system allows citizens to submit property applications, upload required documents, complete payments, and track application status. Government authorities can verify KYC information, property documents, and ownership details before an authorized Registrar records the verified property state on the blockchain.

---

# 2. Project Objectives

The major objectives of B.H.U.M.I. are:

1. To provide a digital platform for property registration and ownership management.
2. To simplify the interaction between citizens and government authorities.
3. To provide a structured property verification workflow.
4. To securely manage property and KYC documents.
5. To generate SHA-256 cryptographic hashes for verified documents and metadata.
6. To maintain tamper-evident property records using blockchain.
7. To maintain a transparent ownership history.
8. To provide an auditable record of important property transactions.
9. To allow ownership transfers through a controlled verification process.
10. To support INR-based payments without requiring citizens to own cryptocurrency.
11. To maintain sensitive personal information outside the blockchain.
12. To provide role-based access to citizens and authorized government officials.

---

# 5. High-Level Architecture

```text
                         B.H.U.M.I. PLATFORM
                                  |
                  ┌───────────────┴───────────────┐
                  |                               |
             CITIZEN                         GOVERNMENT
                  |                               |
             User Login                      Official Login
                  |                               |
                  ▼                               ▼
          Citizen Dashboard              Government Dashboard
                  |                               |
                  └───────────────┬───────────────┘
                                  |
                                  ▼
                         React Frontend
                                  |
                                  ▼
                      Node.js + Express Backend
                                  |
             ┌────────────────────┼────────────────────┐
             |                    |                    |
             ▼                    ▼                    ▼
        Database           Secure Storage       Payment Gateway
      PostgreSQL/           IPFS / S3              INR
        MongoDB
             |
             ▼
       Verification Layer
             |
      ┌──────┼───────┐
      |      |       |
     KYC   Document Property
   Verify   Verify   Verify
      |      |       |
      └──────┼───────┘
             |
             ▼
       SHA-256 Hashing
             |
             ▼
      Authorized Registrar
             |
             ▼
       ethers.js / Backend
             |
             ▼
      LandRegistry Smart Contract
             |
             ▼
          Blockchain
             |
      ┌──────┼──────────┐
      |      |          |
 Ownership  Document   Audit
 History     Proof     Trail
```

---

# 6. Architectural Principles

## 6.1 Hybrid Architecture

B.H.U.M.I. combines traditional Web2 infrastructure with blockchain technology.

Traditional infrastructure manages:

* User accounts
* Property information
* KYC
* Documents
* Applications
* Payments
* Verification workflows

Blockchain manages:

* Cryptographic proofs
* Ownership records
* Timestamps
* Ownership history
* Audit information

---

## 6.2 Off-Chain Sensitive Data

Sensitive information is stored outside the blockchain.

Examples include:

* Aadhaar information
* KYC documents
* Phone numbers
* Residential addresses
* Owner photographs
* Property documents
* Payment information
* Application records

---

## 6.3 Blockchain as a Proof Layer

Blockchain should be treated as a **verification and audit layer** rather than a replacement for the government's legal land database.

A blockchain record does not automatically establish the legal validity of a property document.

Government verification remains necessary before a property is recorded as verified.

---

# 7. User Roles

## 7.1 Citizen

The Citizen can:

* Register/Login
* Search properties
* Submit property applications
* Upload documents
* Provide KYC information
* Make payments
* Track application status
* Request ownership transfers
* View ownership history

---

## 7.2 Government Official

Government officials can:

* Login securely
* View pending applications
* Verify KYC
* Verify property documents
* Verify property information
* Verify ownership
* Approve applications
* Reject applications
* Request document resubmission
* Initiate blockchain registration
* Review blockchain transactions

---

## 7.3 Authorized Registrar

The Authorized Registrar performs the blockchain registration step after successful government verification.

Responsibilities include:

* Reviewing verified application information
* Authorizing blockchain registration
* Signing the blockchain transaction
* Registering the verified property
* Approving ownership transfers on-chain

---

# 8. Technology Stack

## 8.1 Frontend

### React.js

React.js will be used to develop the citizen and government interfaces.

It will provide:

* Component-based development
* Reusable UI components
* Dashboard interfaces
* Forms
* Application tracking
* Government verification screens

### Vite

Vite will be used as the frontend build tool.

### Tailwind CSS

Tailwind CSS will be used for responsive and consistent UI development.

### Framer Motion

Framer Motion can be used for UI transitions and animations.

---

# 9. Backend Technology

## 9.1 Node.js

Node.js will be used as the backend runtime environment.

It is responsible for:

* API execution
* Authentication
* Application processing
* Verification workflows
* Payment verification
* Blockchain communication

## 9.2 Express.js

Express.js will provide the REST API layer.

Major backend responsibilities include:

* Authentication
* Authorization
* Property management
* Document management
* KYC processing
* Payment processing
* Hash generation
* Blockchain interaction
* Audit logging

---

# 10. Database Architecture

The database stores application-level information.

The workflow allows either PostgreSQL or MongoDB. For the implementation, the project can select one database rather than implementing both simultaneously.

### Recommended implementation

**PostgreSQL + Prisma**

PostgreSQL is suitable because the B.H.U.M.I. workflow contains strong relationships between:

* Users
* Properties
* Applications
* Documents
* Payments
* Transfers
* Verification records
* Ownership records
* Audit records

---

# 11. Database Components

The major database entities are:

```text
Users
  |
  ├── Applications
  ├── KYC Records
  └── Ownership Records

Properties
  |
  ├── Applications
  ├── Documents
  ├── Ownership History
  └── Transfers

Applications
  |
  ├── Documents
  ├── Verification
  ├── Payment
  └── Blockchain Registration

Transfers
  |
  ├── New Owner
  ├── Documents
  ├── Payment
  ├── Verification
  └── Blockchain Transaction
```

---

# 12. Database Schema

## 12.1 Users

| Field         | Description               |
| ------------- | ------------------------- |
| id            | Unique user identifier    |
| name          | User's name               |
| email         | User email                |
| phone         | User phone number         |
| password_hash | Encrypted/hashed password |
| role          | Citizen or official role  |
| status        | Account status            |
| created_at    | Account creation time     |
| updated_at    | Last update time          |

---

# 13. KYC Records

| Field               | Description                   |
| ------------------- | ----------------------------- |
| id                  | Unique KYC record             |
| user_id             | Associated user               |
| identity_type       | Type of identity document     |
| document_reference  | Secure reference to document  |
| verification_status | Pending / Verified / Rejected |
| verified_by         | Government official           |
| verified_at         | Verification timestamp        |

Sensitive identity information should remain off-chain.

---

# 14. Properties

| Field              | Description                            |
| ------------------ | -------------------------------------- |
| id                 | Internal property ID                   |
| property_id        | Public application property identifier |
| survey_number      | Survey number                          |
| district           | District                               |
| tehsil             | Tehsil                                 |
| village            | Village                                |
| area               | Property area                          |
| land_type          | Residential / Commercial / Other       |
| current_owner_id   | Current owner                          |
| status             | Property status                        |
| blockchain_tx_hash | Blockchain transaction reference       |
| created_at         | Creation time                          |
| updated_at         | Last update time                       |

---

# 15. Property Documents

| Field               | Description                    |
| ------------------- | ------------------------------ |
| id                  | Document ID                    |
| property_id         | Associated property            |
| document_type       | Document category              |
| storage_reference   | Secure file location           |
| sha256_hash         | Cryptographic hash             |
| verification_status | Verification result            |
| uploaded_by         | User who uploaded document     |
| verified_by         | Official who verified document |
| created_at          | Upload timestamp               |

---

# 16. Applications

| Field            | Description                       |
| ---------------- | --------------------------------- |
| id               | Application ID                    |
| applicant_id     | Citizen who submitted application |
| property_id      | Related property                  |
| application_type | Registration / Transfer           |
| status           | Current application status        |
| submitted_at     | Submission timestamp              |
| reviewed_at      | Review timestamp                  |
| approved_by      | Government official               |
| rejection_reason | Reason for rejection              |

---

# 17. Payments

| Field             | Description                   |
| ----------------- | ----------------------------- |
| id                | Payment ID                    |
| application_id    | Related application           |
| amount            | Amount in INR                 |
| gateway_reference | Payment gateway reference     |
| status            | Pending / Successful / Failed |
| payment_method    | Selected payment method       |
| created_at        | Payment timestamp             |

Citizens interact with the payment system using INR. They are not required to purchase cryptocurrency.

---

# 18. Verification Records

| Field           | Description                  |
| --------------- | ---------------------------- |
| id              | Verification ID              |
| application_id  | Related application          |
| kyc_status      | KYC verification result      |
| document_status | Document verification result |
| property_status | Property verification result |
| verified_by     | Government official          |
| remarks         | Verification remarks         |
| verified_at     | Verification timestamp       |

---

# 19. Ownership History

| Field              | Description            |
| ------------------ | ---------------------- |
| id                 | Record ID              |
| property_id        | Property               |
| previous_owner_id  | Previous owner         |
| new_owner_id       | New owner              |
| transfer_id        | Associated transfer    |
| blockchain_tx_hash | Blockchain transaction |
| transferred_at     | Transfer timestamp     |

---

# 20. Audit Logs

| Field       | Description            |
| ----------- | ---------------------- |
| id          | Audit record           |
| user_id     | User performing action |
| action      | Action performed       |
| entity_type | Related entity         |
| entity_id   | Related record         |
| timestamp   | Action time            |
| metadata    | Additional information |

---

# 21. Document Storage Architecture

B.H.U.M.I. does not store complete sensitive documents directly on blockchain.

The process is:

```text
Document Upload
      |
      ▼
File Validation
      |
      ▼
Secure Storage
      |
      ▼
SHA-256 Hash Generation
      |
      ▼
Government Verification
      |
      ▼
Hash Recorded On-Chain
```

Possible storage systems include:

* IPFS
* Amazon S3
* Secure local storage for prototype development

---

# 22. Document Hashing

Each verified document can be processed using SHA-256.

Example:

```text
Original Document
       |
       ▼
    SHA-256
       |
       ▼
Document Hash
       |
       ▼
Blockchain
```

If the document changes later, its SHA-256 hash will also change.

Therefore, the system can compare the current document hash with the blockchain-recorded hash.

---

# 23. Owner Photo Hash

The owner photograph can also be hashed.

The original photograph remains off-chain.

```text
Owner Photograph
       |
       ▼
    SHA-256
       |
       ▼
Photo Hash
       |
       ▼
Blockchain
```

The blockchain does not store the complete photograph.

---

# 24. Metadata Hash

Relevant verified property metadata can also be converted into a cryptographic hash.

For example:

```text
Property Metadata
      |
      ▼
Canonical Data Format
      |
      ▼
SHA-256
      |
      ▼
Metadata Hash
      |
      ▼
Blockchain
```

This provides a mechanism for later verification of the recorded metadata.

---

# 25. Data Storage Strategy

| Data                     | Storage                   |
| ------------------------ | ------------------------- |
| User Profile             | Off-chain Database        |
| Aadhaar/KYC Information  | Secure Off-chain Storage  |
| Owner Photograph         | Secure Off-chain Storage  |
| Property Documents       | Secure Off-chain Storage  |
| Property Metadata        | Database                  |
| Payment Information      | Database/Payment Provider |
| Application Data         | Database                  |
| Property ID              | Blockchain                |
| Owner Blockchain Address | Blockchain                |
| Document Hash            | Blockchain                |
| Photo Hash               | Blockchain                |
| Metadata Hash            | Blockchain                |
| Registration Timestamp   | Blockchain                |
| Ownership History        | Blockchain                |
| Registry Status          | Blockchain                |

---

# 26. Smart Contract Architecture

The primary smart contract is:

```text
LandRegistry.sol
```

The contract represents the blockchain-side property registry state.

A property can contain:

```solidity
struct Property {

    bytes32 propertyId;

    address currentOwner;

    bytes32 documentHash;

    bytes32 ownerPhotoHash;

    bytes32 metadataHash;

    uint256 registeredAt;

    uint256 lastTransferAt;

    PropertyStatus status;

    bool exists;
}
```

---

# 27. Property Status

The smart contract can use:

```solidity
enum PropertyStatus {

    Pending,

    Verified,

    Active,

    Frozen,

    Disputed
}
```

These states correspond to the property lifecycle defined by the project workflow.

---

# 28. Smart Contract Functions

The contract may contain functions such as:

```text
registerProperty()
transferOwnership()
freezeProperty()
unfreezeProperty()
markDisputed()
resolveDispute()
getProperty()
getCurrentOwner()
getOwnershipHistory()
```

Access to sensitive state-changing functions should be restricted to authorized accounts.

---

# 29. Smart Contract Events

Important blockchain events may include:

```text
PropertyRegistered
OwnershipTransferred
PropertyFrozen
PropertyUnfrozen
PropertyDisputed
PropertyDisputeResolved
```

Events provide an auditable history of important state changes.

---

# 30. Blockchain Authorization

Only an authorized Registrar should be able to perform final blockchain registration.

```text
Government Verification
          |
          ▼
Verification Successful
          |
          ▼
Hash Generation
          |
          ▼
Authorized Registrar
          |
          ▼
Smart Contract
          |
          ▼
Blockchain
```

The Registrar's blockchain identity is controlled using an authorized wallet.

---

# 31. Backend-First Blockchain Architecture

The frontend should not directly perform the core registry blockchain transaction.

The architecture follows:

```text
Citizen / Government
        |
        ▼
Frontend
        |
        ▼
Backend
        |
        ▼
Verification
        |
        ▼
Hash Generation
        |
        ▼
Authorized Registrar
        |
        ▼
Smart Contract
        |
        ▼
Blockchain
```

This keeps the blockchain transaction workflow under backend and authorized government control.

---

# 32. Complete Property Registration Workflow

The property registration process is:

```text
Citizen
   |
   ▼
Login / OTP
   |
   ▼
Citizen Dashboard
   |
   ▼
Submit Property Application
   |
   ▼
Upload Documents
   |
   ▼
Backend
   |
   ▼
Application ID Created
   |
   ▼
Pending Verification
   |
   ▼
Government Dashboard
   |
   ├── KYC Verification
   |
   ├── Document Verification
   |
   └── Property Verification
   |
   ▼
Verification Decision
   |
   ├───────────────┐
   |               |
 Reject          Approve
   |               |
   ▼               ▼
Application      Property
Rejected         Verified
                   |
                   ▼
             SHA-256 Hashing
                   |
                   ▼
          Authorized Registrar
                   |
                   ▼
             Smart Contract
                   |
                   ▼
               Blockchain
                   |
                   ▼
          Property Registered
                   |
                   ▼
           Ownership History
```

---

# 33. Government Verification Workflow

```text
Government Official Login
          |
          ▼
Pending Applications
          |
          ▼
Open Application
          |
     ┌────┼────┐
     ▼    ▼    ▼
   KYC  Docs Property
 Verify Verify Verify
     |    |    |
     └────┼────┘
          |
          ▼
   All Checks Passed?
       /       \
     No         Yes
     |           |
     ▼           ▼
Reject /      Approve
Resubmit      Application
                 |
                 ▼
            Hash Generation
                 |
                 ▼
         Authorized Registrar
                 |
                 ▼
              Blockchain
```

---

# 34. INR Payment Architecture

B.H.U.M.I. provides an INR-based payment experience.

Citizens do not need to understand cryptocurrency.

The citizen sees:

```text
Property Registration / Transfer Fee

Amount: ₹25,000

[ Pay Now ]
```

The payment gateway handles the INR payment.

After successful payment:

```text
Citizen Pays INR
       |
       ▼
Payment Gateway
       |
       ▼
Backend Confirms Payment
       |
       ▼
Application Continues
```

---

# 35. Blockchain Gas Architecture

Citizens should not be required to:

```text
Buy ETH
   ↓
Connect MetaMask
   ↓
Pay Gas
   ↓
Register Property
```

Instead:

```text
Citizen Pays INR
       |
       ▼
Backend Confirms Payment
       |
       ▼
Backend / Authorized Wallet
       |
       ▼
Pays Blockchain Gas
       |
       ▼
Smart Contract
       |
       ▼
Blockchain
```

This provides a Web2-style user experience while blockchain operates as the underlying infrastructure.

---

# 36. Ownership Transfer Workflow

After a property has been registered, ownership can be transferred through a controlled process.

```text
Current Owner
      |
      ▼
Initiate Transfer
      |
      ▼
Transfer Request
      |
      ▼
New Owner KYC
      |
      ▼
Upload Transfer Documents
      |
      ▼
Pay Transfer Fee
      |
      ▼
Government Verification
      |
      ▼
Approved?
    /     \
  No       Yes
  |         |
  ▼         ▼
Reject   Generate Hashes
            |
            ▼
     Authorized Registrar
            |
            ▼
       Smart Contract
            |
            ▼
      Owner Updated
            |
            ▼
    Ownership History
```

---

# 37. Property Lifecycle

The B.H.U.M.I. property lifecycle is:

```text
Pending
   |
   ├── Rejected
   |
   ▼
Verified
   |
   ▼
Active
   |
   ├── Disputed
   |
   └── Frozen
```

Ownership transfer can occur while the property is active.

A simplified state model is:

```text
                 ┌──────────────┐
                 │    Pending   │
                 └──────┬───────┘
                        |
              Government Approval
                        |
                        ▼
                 ┌──────────────┐
                 │   Verified   │
                 └──────┬───────┘
                        |
                Blockchain Record
                        |
                        ▼
                 ┌──────────────┐
          ┌──────│    Active    │──────┐
          |      └──────────────┘      |
          |                             |
          ▼                             ▼
     ┌──────────┐                  ┌──────────┐
     │ Disputed │                  │  Frozen  │
     └────┬─────┘                  └────┬─────┘
          |                             |
          └───────────► Active ◄────────┘
```

---

# 38. Blockchain Record Example

A blockchain property record may contain:

| Field            | Example              |
| ---------------- | -------------------- |
| Property ID      | PROP-MP-BPL-001      |
| Current Owner    | `0x1234...ABCD`      |
| Document Hash    | `8e7d...a93f`        |
| Owner Photo Hash | `91ab...73cd`        |
| Metadata Hash    | `a821...9d72`        |
| Registered At    | Blockchain Timestamp |
| Status           | ACTIVE               |

---

# 39. Off-Chain Property Record Example

```json
{
  "propertyId": "PROP-MP-BPL-001",
  "surveyNumber": "123/4",
  "district": "Bhopal",
  "tehsil": "Huzur",
  "village": "Example Village",
  "area": "1500 sq.ft",
  "landType": "Residential",
  "status": "VERIFIED"
}
```

The complete property information remains in the application database.

---

# 40. Security Architecture

## 40.1 Authentication

The platform should support:

* Secure login
* JWT or secure session authentication
* OTP-based authentication where applicable
* Government official authentication
* Password hashing

---

## 40.2 Authorization

Role-based access control should be implemented.

Example:

| Role                | Access                                  |
| ------------------- | --------------------------------------- |
| Citizen             | Own applications and property requests  |
| Government Official | Verification and application processing |
| Registrar           | Blockchain registration                 |
| Admin               | System administration                   |

---

# 41. Blockchain Security

The blockchain architecture should follow:

* Authorized Registrar wallet
* Backend-controlled blockchain transactions
* Smart contract access control
* No private keys in frontend
* Secure environment variables
* Transaction logging
* Event monitoring
* Validation before blockchain submission

---

# 42. Sensitive Data Protection

The following information must **not** be stored directly on a public blockchain:

* Aadhaar number
* Aadhaar document
* Full owner photograph
* Phone number
* Residential address
* Personal KYC information
* Complete property documents
* Payment credentials

Instead, only appropriate cryptographic proofs and registry information should be stored on-chain.

---

# 43. Project Folder Structure

```text
BHUMI/
│
├── frontend/
│   ├── public/
│   └── src/
│       ├── assets/
│       ├── components/
│       │   ├── ui/
│       │   ├── forms/
│       │   ├── property/
│       │   ├── transfer/
│       │   ├── documents/
│       │   └── blockchain/
│       │
│       ├── pages/
│       │   ├── auth/
│       │   ├── citizen/
│       │   ├── government/
│       │   └── registrar/
│       │
│       ├── layouts/
│       ├── hooks/
│       ├── services/
│       ├── api/
│       ├── routes/
│       ├── types/
│       ├── utils/
│       ├── constants/
│       ├── App.tsx
│       └── main.tsx
│
├── backend/
│   └── src/
│       ├── config/
│       ├── controllers/
│       ├── routes/
│       ├── models/
│       ├── middleware/
│       ├── services/
│       │   ├── auth/
│       │   ├── kyc/
│       │   ├── property/
│       │   ├── documents/
│       │   ├── payments/
│       │   ├── verification/
│       │   ├── hashing/
│       │   ├── blockchain/
│       │   └── audit/
│       │
│       ├── utils/
│       ├── jobs/
│       ├── app.ts
│       └── server.ts
│
├── contracts/
│   ├── src/
│   │   └── LandRegistry.sol
│   ├── script/
│   │   └── deploy.ts
│   ├── test/
│   │   ├── LandRegistry.test.ts
│   │   ├── authorization.test.ts
│   │   └── transfer.test.ts
│   └── hardhat.config.ts
│
├── docs/
│   └── architecture/
│
├── storage/
│   └── documents/
│
├── .env.example
├── docker-compose.yml
├── package.json
└── README.md
```

---

# 44. Environment Variables

## Backend

```env
NODE_ENV=development

PORT=5000

DATABASE_URL=postgresql://postgres:password@localhost:5432/bhumi

JWT_SECRET=CHANGE_THIS_TO_A_LONG_RANDOM_SECRET

JWT_EXPIRES_IN=1d

CORS_ORIGIN=http://localhost:5173

STORAGE_PATH=./storage/documents

DOCUMENT_MAX_SIZE_MB=10

ENCRYPTION_KEY=CHANGE_THIS_TO_A_SECURE_KEY

BLOCKCHAIN_RPC_URL=http://127.0.0.1:8545

BLOCKCHAIN_CHAIN_ID=31337

LAND_REGISTRY_CONTRACT_ADDRESS=0x...

REGISTRAR_WALLET_ADDRESS=0x...

REGISTRAR_PRIVATE_KEY=DO_NOT_COMMIT_THIS

PAYMENT_GATEWAY_KEY=CHANGE_THIS

PAYMENT_GATEWAY_SECRET=CHANGE_THIS

LOG_LEVEL=info
```

---

# 45. Frontend Environment Variables

```env
VITE_API_BASE_URL=http://localhost:5000/api

VITE_BLOCKCHAIN_CHAIN_ID=31337

VITE_LAND_REGISTRY_CONTRACT_ADDRESS=0x...
```

Private credentials must never be placed inside frontend environment variables.

The following must remain backend-only:

```text
REGISTRAR_PRIVATE_KEY
JWT_SECRET
ENCRYPTION_KEY
DATABASE_PASSWORD
PAYMENT_GATEWAY_SECRET
```

---

# 46. API Architecture

The backend REST API can be divided into:

```text
/api/auth
/api/users
/api/properties
/api/applications
/api/kyc
/api/documents
/api/verification
/api/payments
/api/transfers
/api/ownership
/api/blockchain
/api/audit
```

Example endpoints:

```text
POST   /api/auth/register
POST   /api/auth/login

POST   /api/properties
GET    /api/properties/:id

POST   /api/applications
GET    /api/applications/:id

POST   /api/documents
GET    /api/documents/:id

POST   /api/verification/:id
POST   /api/applications/:id/approve
POST   /api/applications/:id/reject

POST   /api/payments/create
POST   /api/payments/verify

POST   /api/transfers
GET    /api/transfers/:id

POST   /api/blockchain/register
GET    /api/blockchain/:propertyId

GET    /api/audit/:propertyId
```

---

# 47. End-to-End System Workflow

The complete B.H.U.M.I. workflow is:

```text
                     CITIZEN
                        |
                        ▼
                  Login / OTP
                        |
                        ▼
                Citizen Dashboard
                        |
                        ▼
               Submit Property
                        |
                        ▼
                Upload Documents
                        |
                        ▼
                 Make INR Payment
                        |
                        ▼
                     BACKEND
                        |
                        ▼
               Create Application
                        |
                        ▼
              Government Dashboard
                        |
              ┌─────────┼─────────┐
              ▼         ▼         ▼
             KYC      Document   Property
          Verification Verification Verification
              └─────────┼─────────┘
                        |
                        ▼
               Verification Result
                    /         \
                  Reject      Approve
                    |           |
                    ▼           ▼
                Rejected    Generate Hashes
                                |
                                ▼
                         Authorized Registrar
                                |
                                ▼
                          LandRegistry.sol
                                |
                                ▼
                             Blockchain
                                |
                    ┌───────────┼───────────┐
                    ▼           ▼           ▼
                Ownership    Document      Audit
                  History      Proof        Trail
```

---

# 48. Ownership Transfer Workflow

```text
Current Owner
      |
      ▼
Transfer Request
      |
      ▼
New Owner KYC
      |
      ▼
Transfer Documents
      |
      ▼
INR Transfer Fee
      |
      ▼
Government Verification
      |
      ▼
Approval
      |
      ▼
Generate Hashes
      |
      ▼
Authorized Registrar
      |
      ▼
Smart Contract
      |
      ▼
Blockchain
      |
      ▼
Current Owner Updated
      |
      ▼
Ownership History Updated
```

---

# 49. Why Blockchain Is Used

A traditional database can be modified through authorized database operations.

B.H.U.M.I. adds a blockchain proof layer:

```text
Verified Record
      |
      ▼
SHA-256 Hash
      |
      ▼
Blockchain
      |
      ▼
Timestamp + Immutable History
```

Blockchain provides:

* Tamper-evident records
* Verifiable timestamps
* Ownership history
* Cryptographic proof
* Auditability
* Shared trust between authorized stakeholders

However, blockchain does not itself prove that a document is legally genuine.

Government verification remains an essential part of the system.

---

# 50. Error and Failure Handling

The system should handle cases such as:

### Payment Failure

```text
Payment Failed
      |
      ▼
Application remains incomplete
      |
      ▼
User can retry payment
```

### Verification Failure

```text
Verification Failed
      |
      ▼
Application Rejected / Resubmission Requested
```

### Blockchain Transaction Failure

```text
Blockchain Transaction Failed
      |
      ▼
Record Failure
      |
      ▼
Retry / Registrar Review
```

### Hash Mismatch

```text
Stored Document
      |
      ▼
Generate Current Hash
      |
      ▼
Compare With Blockchain Hash
      |
      ├── Match → Verified
      |
      └── Mismatch → Flag for Review
```

---

# 51. Testing Strategy

## 51.1 Unit Testing

Test individual:

* Authentication functions
* Hash generation
* Validation functions
* Property services
* Payment services
* Verification services

## 51.2 Smart Contract Testing

Test:

* Property registration
* Ownership transfer
* Registrar authorization
* Invalid transactions
* Property status changes
* Event generation

## 51.3 Integration Testing

Test complete flows:

```text
Citizen
   ↓
Application
   ↓
Payment
   ↓
Government Verification
   ↓
Hash Generation
   ↓
Registrar
   ↓
Blockchain
```

---

# 52. Deployment Architecture

For prototype development:

```text
Frontend
   |
   ▼
Vite Development Server
   |
   ▼
Node.js / Express
   |
   ├── PostgreSQL
   ├── Local File Storage
   └── Local Ethereum Network
          |
          ▼
        Hardhat
```

For future deployment:

```text
Users
  |
  ▼
Cloud Frontend
  |
  ▼
Cloud Backend
  |
  ├── Managed Database
  ├── Secure Object Storage
  ├── Payment Gateway
  └── Blockchain Network
```

---

# 53. Development Environment

The project can use:

* Node.js
* npm
* React
* Vite
* PostgreSQL
* Prisma
* Solidity
* Hardhat
* ethers.js
* Docker
* Git
* GitHub

For local blockchain development, Hardhat can provide an Ethereum-compatible development network.

---

# 54. Git Workflow

The project should use a controlled Git workflow.

```text
main
 |
 ├── feature/frontend
 ├── feature/backend
 ├── feature/blockchain
 ├── feature/payment
 ├── feature/verification
 └── feature/document-management
```

Changes should be developed in feature branches and merged into the main branch after review.

---

# 55. Future Scope

The current architecture can later be extended with:

* GIS-based land mapping
* AI-assisted document verification
* OCR for land documents
* Duplicate property detection
* Mobile application
* Government API integration
* Existing land-record system interoperability
* Digital identity integration
* Automated mutation workflows
* SMS and email notifications
* Government analytics dashboard
* Automated compliance checks
* Multi-state deployment
* Rural citizen support

These features are considered future extensions rather than mandatory components of the core architecture.

---

# 56. Social and Economic Impact

## Transparency

Authorized stakeholders can track verified property records and ownership history.

## Trust

Cryptographic hashes provide tamper-evident proof of recorded information.

## Efficiency

Digital workflows can reduce manual coordination between citizens and government authorities.

## Accessibility

Citizens interact with the platform using INR rather than directly handling cryptocurrency.

## Auditability

Blockchain provides a verifiable history of important registered transactions.

---

# 57. Important Design Limitations

B.H.U.M.I. is currently a **prototype and architectural model**.

A production deployment would require integration with:

* Actual government land-record systems
* Legal frameworks
* Government identity infrastructure
* Official KYC infrastructure
* Approved payment providers
* Data protection requirements
* Government authentication systems
* Appropriate blockchain infrastructure

The blockchain component should therefore be treated as a **verification and audit layer**, not as a replacement for legally recognized government land records.

---

# 58. Core Innovation

The core innovation of B.H.U.M.I. is the combination of:

```text
Government Verification
          +
Secure Off-Chain Storage
          +
Cryptographic Hashing
          +
Authorized Blockchain Registration
          +
INR-Based Payments
          +
Ownership History
          =
Transparent Hybrid Land Registry
```

---

# 59. Final Architecture Summary

B.H.U.M.I. follows a hybrid architecture in which the **application and government verification workflows remain primarily Web2-based**, while blockchain provides a trusted cryptographic proof and audit layer.

The complete system can be summarized as:

```text
                    B.H.U.M.I.
                        |
        ┌───────────────┴───────────────┐
        |                               |
     CITIZEN                         GOVERNMENT
        |                               |
        ▼                               ▼
  React Frontend                Government Dashboard
        |                               |
        └───────────────┬───────────────┘
                        |
                        ▼
               Node.js + Express
                        |
        ┌───────────────┼────────────────┐
        |               |                |
        ▼               ▼                ▼
    Database        File Storage    INR Payment
        |               |                |
        └───────────────┼────────────────┘
                        |
                        ▼
                  Verification
                        |
              ┌─────────┼─────────┐
              ▼         ▼         ▼
             KYC      Documents Property
              └─────────┼─────────┘
                        |
                        ▼
                   SHA-256
                        |
                        ▼
              Authorized Registrar
                        |
                        ▼
                LandRegistry.sol
                        |
                        ▼
                    Blockchain
                        |
        ┌───────────────┼───────────────┐
        ▼               ▼               ▼
   Ownership        Document          Audit
     History           Proof           Trail
```

**B.H.U.M.I. therefore provides a complete digital workflow from citizen application to government verification, payment, cryptographic proof generation, authorized blockchain registration, and long-term ownership history.**


