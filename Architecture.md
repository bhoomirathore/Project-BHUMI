# B.H.U.M.I. --- Technical Architecture & System Workflow

**Project:** B.H.U.M.I. (Blockchain Hosted Unified Mutation
Infrastructure)\
**Document Type:** Technical Architecture and End-to-End Workflow\
------------------------------------------------------------------------

## 1. Document Purpose

This document combines the project's system architecture and operational
workflow into a single reference for development, review, testing, and
future implementation.

B.H.U.M.I. is designed as a government-oriented digital land transaction
and mutation platform. It uses a **hybrid Web2 + Web3 architecture**:

-   PostgreSQL handles application data and high-speed operational
    queries.
-   Secure off-chain storage handles sensitive documents and
    identity-related data.
-   SHA-256 hashes provide cryptographic fingerprints of verified
    documents and metadata.
-   A Solidity smart contract provides the blockchain trust,
    ownership-history, and audit layer.
-   Government-authorized users control final blockchain registration
    and mutation actions.

> **Important:** B.H.U.M.I. is a prototype/architectural model. It does
> not replace official government land-record systems or legal
> registration infrastructure. Production deployment would require
> government integration, legal approval, official identity/KYC
> infrastructure, payment providers, and an appropriate blockchain
> network.

------------------------------------------------------------------------

## 2. Project Overview

### 2.1 Full Form

**B.H.U.M.I. --- Blockchain Hosted Unified Mutation Infrastructure**

### 2.2 Core Idea

The system connects property registration, verification, ownership
transfer, and mutation into one auditable workflow.

The central architectural principle is:

> **Keep operational and sensitive data off-chain; store cryptographic
> proofs and finalized ownership events on-chain.**

This avoids placing large documents, personal information, or frequently
queried application data directly on the blockchain.

### 2.3 What the Blockchain Does

The blockchain acts as a **trust and audit layer**, rather than as the
primary application database.

It can maintain:

-   Property ID
-   Current owner blockchain identity
-   Document hash
-   Owner-photo hash, where applicable
-   Metadata hash
-   Registration timestamp
-   Last transfer timestamp
-   Property status
-   Ownership-transfer history
-   Transaction/event references

### 2.4 What Remains Off-Chain

The following remain in conventional application infrastructure:

-   User profiles
-   Login/session information
-   KYC information
-   Aadhaar/identity information
-   Phone and address information
-   Property metadata used for application queries
-   Land records and Khasra information
-   Uploaded documents
-   Photos
-   Application records
-   Appointments
-   Payment records
-   Verification records
-   Audit logs
-   Generated e-Registry PDFs

------------------------------------------------------------------------

## 3. Architectural Principles

### 3.1 Hybrid Architecture

B.H.U.M.I. separates the application layer from the blockchain trust
layer.

``` text
Application / Operational Layer
        |
        +-- React frontend
        +-- Node.js / Express backend
        +-- PostgreSQL
        +-- Secure document storage
        +-- Payment gateway
        +-- KYC / verification services
        |
        v
Cryptographic Trust Layer
        |
        +-- SHA-256 hashes
        +-- Authorized Registrar
        +-- LandRegistry smart contract
        +-- Blockchain ledger
```

### 3.2 Backend-Controlled Blockchain Transactions

The frontend does not directly write registry data to the blockchain.

The intended flow is:

``` text
Citizen / Government User
        |
        v
React Frontend
        |
        v
Node.js + Express Backend
        |
        v
Authentication + RBAC + Verification
        |
        v
SHA-256 Hash Generation
        |
        v
Authorized Registrar
        |
        v
Registrar Wallet
        |
        v
LandRegistry Smart Contract
        |
        v
Blockchain
```

This keeps blockchain authorization under controlled government/backend
workflows.

### 3.3 Separation of Concerns

The system is divided into:

1.  Presentation layer
2.  Application/API layer
3.  Core business services
4.  Data and storage layer
5.  Verification layer
6.  Hashing and blockchain layer
7.  External integrations
8.  Reporting and monitoring

------------------------------------------------------------------------

# 4. System Architecture

## 4.1 High-Level Architecture Diagram

The repository should keep the architecture diagram at:

`docs/assets/bhumi-system-architecture.png`

A Mermaid version is also maintained below so the architecture remains
editable in source control.

``` mermaid
flowchart TB

    subgraph USERS["Users"]
        C["Citizen"]
        LA["Local Authority"]
        HQ["Government HQ"]
    end

    subgraph FRONTEND["Frontend — React + Vite + Tailwind"]
        UI["Citizen Portal / Government Portal"]
    end

    subgraph BACKEND["Backend — Node.js + Express"]
        API["REST API"]
        AUTH["Authentication & RBAC"]
        LAND["Land Search"]
        APP["Application / Appointment"]
        DOC["Document Service"]
        PAY["Payment Service"]
        VERIFY["Verification Service"]
        MUT["Registry / Mutation Service"]
        HASH["Hashing Service"]
        BC["Blockchain Service"]
        AUDIT["Audit / Analytics / Notifications"]
    end

    subgraph DATA["Data & Storage"]
        DB[("PostgreSQL")]
        STORE[("IPFS / S3 / Secure Storage")]
        CACHE[("Redis — Optional")]
    end

    subgraph EXT["External Services"]
        PG["INR Payment Gateway"]
        MSG["Email / SMS / OTP"]
    end

    subgraph VERIFYL["Verification Layer"]
        KYC["KYC Verification"]
        DOCV["Document Verification"]
        PROP["Property Verification"]
    end

    subgraph CHAIN["Blockchain Layer"]
        SHA["SHA-256 Hashing"]
        REG["Authorized Registrar"]
        SC["LandRegistry.sol"]
        LEDGER[("Ethereum / EVM Testnet or Local Network")]
    end

    subgraph OUTPUT["Outputs"]
        CD["Citizen Dashboard"]
        GD["Government Dashboard"]
        HD["HQ Analytics / Audit"]
        PV["Public Verification"]
    end

    C --> UI
    LA --> UI
    HQ --> UI

    UI --> API
    API --> AUTH
    API --> LAND
    API --> APP
    API --> DOC
    API --> PAY
    API --> VERIFY
    API --> MUT
    API --> AUDIT

    LAND --> DB
    APP --> DB
    DOC --> STORE
    AUTH --> DB
    AUDIT --> DB
    CACHE -.-> API

    PAY --> PG
    APP --> MSG

    VERIFY --> KYC
    VERIFY --> DOCV
    VERIFY --> PROP

    MUT --> HASH
    HASH --> SHA
    SHA --> REG
    REG --> SC
    SC --> LEDGER

    BC --> SC
    LEDGER --> BC
    BC --> DB

    API --> CD
    API --> GD
    API --> HD
    API --> PV
```

------------------------------------------------------------------------

# 5. Technology Stack

  Layer              Technology                            Purpose
  ------------------ ------------------------------------- --------------------------------------
  Frontend           React.js                              Web application UI
  Build Tool         Vite                                  Frontend development/build
  Styling            Tailwind CSS                          UI styling
  Backend            Node.js + Express.js                  REST APIs and business logic
  Database           PostgreSQL                            Relational application and land data
  ORM                Prisma or equivalent                  Database access
  Smart Contract     Solidity                              Registry and ownership logic
  Blockchain         EVM-compatible network                Trust/audit layer
  Web3 Integration   Ethers.js                             Backend-to-contract interaction
  Hashing            SHA-256                               Document/data integrity
  File Storage       IPFS / S3 / secure storage            Document storage
  Authentication     JWT / secure sessions + OTP           Authentication
  Payments           INR payment gateway                   Registry/mutation fee processing
  Notifications      Email/SMS                             OTP, alerts, status notifications
  Local Blockchain   Hardhat/Ganache-style local network   MVP testing
  Deployment         Docker / AWS or equivalent            Application deployment
  CI/CD              Git-based pipeline                    Automated build/test/deployment

The source architecture specifically proposes Solidity with a local
Hardhat/Ganache-style network for the MVP, with strict role-based access
control rather than implementing a full government-grade Hyperledger
network during the prototype phase.

------------------------------------------------------------------------

# 6. System Users and Roles

## 6.1 Citizen

The citizen-facing portal supports:

-   Registration/login
-   OTP authentication
-   Land search using Khasra number
-   Viewing land/property details
-   Property verification
-   Registry appointment booking
-   Document upload
-   INR payment
-   Application tracking
-   Ownership-history viewing
-   e-Registry PDF download
-   Mutation/transfer initiation where applicable

## 6.2 Local Authority

The local authority handles operational verification and
registry/mutation processing.

Responsibilities include:

-   Secure login
-   Viewing pending applications
-   Appointment management
-   Document verification
-   e-KYC verification
-   Property verification
-   Registry processing
-   Mutation processing
-   Approve/reject decisions
-   Initiating authorized registry workflows
-   Viewing blockchain transaction status

## 6.3 Authorized Registrar

The Authorized Registrar is responsible for the final blockchain
authorization step.

Responsibilities include:

-   Reviewing an authorized transaction
-   Signing the blockchain transaction
-   Sending the transaction through the Registrar wallet
-   Confirming the transaction
-   Ensuring the final registry event is recorded

## 6.4 Government HQ

Government HQ is primarily an oversight and monitoring layer.

It can provide:

-   District/office analytics
-   Registration reports
-   Mutation reports
-   Revenue/payment reporting
-   Turnaround-time reporting
-   Pending backlog monitoring
-   Stalled application monitoring
-   Workload monitoring
-   Repeated rejection analysis
-   Hash mismatch alerts
-   SLA breach alerts
-   Audit information

Approval decisions remain part of the operational authority/registrar
workflow rather than HQ analytics.

## 6.5 Admin

The administrative layer manages system configuration and controlled
access.

Typical responsibilities:

-   User/role administration
-   System configuration
-   Access management
-   Monitoring
-   Operational maintenance

------------------------------------------------------------------------

# 7. Citizen Portal Workflow

## 7.1 Citizen Registration/Login

``` text
Citizen
  |
  v
Register / Login
  |
  v
OTP Verification
  |
  v
Citizen Dashboard
```

The authentication system should use secure session/JWT handling and
role-based authorization.

## 7.2 Land Search by Khasra Number

``` text
Citizen enters Khasra Number
          |
          v
Backend Land Search API
          |
          v
PostgreSQL Land Records
          |
          v
Property / Land Details
          |
          v
Verification
```

The search result can contain:

-   Khasra number
-   Property ID
-   District
-   Tehsil
-   Village
-   Area
-   Land type
-   Current recorded status
-   Relevant registration information

If an on-chain record exists, the backend can compare the relevant
stored hash/reference with the blockchain record.

## 7.3 Land Verification

``` text
Land Search
    |
    v
View Land Details
    |
    v
Verify Property Information
    |
    +----> On-chain record exists?
    |          |
    |          +--> Yes: compare relevant hash/reference
    |          |
    |          +--> No: show off-chain registry status
    |
    v
Continue to appointment / application
```

A matching cryptographic fingerprint provides evidence that the
referenced data/document has not changed relative to the committed
blockchain record.

------------------------------------------------------------------------

# 8. Registry Appointment Workflow

``` text
Select Property
      |
      v
Select Office / Appointment Slot
      |
      v
Upload Required Documents
      |
      v
Make INR Payment
      |
      v
Application ID Generated
      |
      v
Pending Verification
      |
      v
Local Authority Review
      |
      +---- Rejected ---> Resubmission
      |
      +---- Approved --> Registry Processing
```

The application and appointment information remains in PostgreSQL.

Payment credentials and sensitive payment information are handled
through the payment gateway rather than stored directly in the
blockchain.

------------------------------------------------------------------------

# 9. Property Registration Workflow

## 9.1 Complete Flow

``` text
Citizen Login
      |
      v
Search Land by Khasra
      |
      v
Verify Land Details
      |
      v
Book Registry Appointment
      |
      v
Upload Documents + KYC
      |
      v
Make INR Payment
      |
      v
Application Created
      |
      v
Local Authority Verification
      |
      +---- Rejected --> Resubmit / Correct
      |
      +---- Approved
              |
              v
        Registry Processing
              |
              v
        Generate SHA-256 Hashes
              |
              v
        Authorized Registrar
              |
              v
        Smart Contract
              |
              v
           Blockchain
              |
              v
       Registration Confirmed
              |
              v
       Ownership History
              |
              v
        e-Registry PDF
```

## 9.2 Verification Before Blockchain Submission

The backend should validate:

-   User authorization
-   Required documents
-   KYC status
-   Property existence
-   Property status
-   Ownership information
-   Relevant restrictions/encumbrances where supported
-   Duplicate/conflicting claims where supported
-   Document integrity
-   Payment status
-   Application status

Only an authorized workflow should proceed to the final blockchain
transaction.

------------------------------------------------------------------------

# 10. Verification Architecture

The verification process is divided into three major areas.

## 10.1 KYC Verification

Validates the identity information required by the workflow.

Sensitive KYC information remains off-chain.

## 10.2 Document Verification

The system validates uploaded property documents and stores the actual
files in secure off-chain storage.

A SHA-256 fingerprint is generated for the document.

``` text
Document
   |
   v
Secure Storage
   |
   v
SHA-256
   |
   v
Document Hash
   |
   v
Blockchain Record
```

The blockchain stores the hash, not the actual PDF/document.

## 10.3 Property Verification

Property verification checks the land record and relevant property
information before registration or transfer.

Where field verification is required, it is part of the government
operational workflow.

------------------------------------------------------------------------

# 11. Hashing Architecture

SHA-256 is used to create a deterministic cryptographic fingerprint.

For example:

``` text
Uploaded Sale Deed
       |
       v
SHA-256
       |
       v
0x / bytes32-style document fingerprint
```

The same document contents produce the same hash. If the contents are
changed, the resulting hash changes.

The project should treat the hash as an **integrity proof**, not as the
document itself.

------------------------------------------------------------------------

# 12. Blockchain Architecture

## 12.1 Smart Contract

The central smart contract is:

`contracts/src/LandRegistry.sol`

A representative property structure is:

``` solidity
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

## 12.2 Property Status

The architecture uses the following lifecycle states:

``` text
Pending
Verified
Active
Frozen
Disputed
```

The operational system can additionally maintain application-level
states such as approved/rejected/resubmission where needed.

## 12.3 Core Contract Operations

The planned contract interface includes operations such as:

``` text
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

## 12.4 Contract Events

Important events include:

``` text
PropertyRegistered
OwnershipTransferred
PropertyFrozen
PropertyUnfrozen
PropertyDisputed
PropertyDisputeResolved
```

The backend should listen for relevant blockchain events so the
PostgreSQL state can be synchronized after confirmed blockchain
activity.

------------------------------------------------------------------------

# 13. Mutation / Ownership Transfer Workflow

Mutation is one of the core B.H.U.M.I. workflows.

``` text
Current Owner
     |
     v
Initiate Transfer / Mutation
     |
     v
New Owner e-KYC
     |
     v
Upload Mutation Documents
     |
     v
Pay Mutation Fee
     |
     v
Local Authority Verification
     |
     +---- Rejected --> Resubmission
     |
     +---- Approved
             |
             v
      Update PostgreSQL
             |
             v
      Generate New Hashes
             |
             v
      Authorized Registrar
             |
             v
      Smart Contract
             |
             v
      Ownership Transfer
             |
             v
      Blockchain Event
             |
             v
      Backend Event Listener
             |
             v
      PostgreSQL Synchronization
             |
             v
      Ownership History Updated
             |
             v
      Updated e-Registry PDF
```

The backend event listener is important because PostgreSQL and the
blockchain are separate state systems. The application should not assume
that a submitted transaction is equivalent to a confirmed transaction.

------------------------------------------------------------------------

# 14. Dual-State Synchronization

B.H.U.M.I. maintains two important states:

1.  PostgreSQL application state
2.  Blockchain ledger state

A failure can occur if the blockchain transaction succeeds but the
application database is not updated.

Therefore:

``` text
Smart Contract
     |
     v
Blockchain Event
     |
     v
Backend Event Listener
     |
     v
Validate / Process Event
     |
     v
Update PostgreSQL
```

The source architecture specifically identifies this dual-state
synchronization problem as a major engineering challenge.

The application should track:

-   Transaction hash
-   Contract address
-   Network/chain ID
-   Transaction status
-   Confirmation state
-   Event data
-   Timestamp
-   Related property/application ID

------------------------------------------------------------------------

# 15. e-Registry PDF Workflow

The e-Registry PDF is generated off-chain after the relevant
registration workflow reaches its final state.

``` text
Registration Confirmed
        |
        v
Collect Verified Property Data
        |
        v
Collect Registration Metadata
        |
        v
Include Document Hash
        |
        v
Include Blockchain Transaction Reference
        |
        v
Generate e-Registry PDF
        |
        v
Store / Serve Securely
        |
        v
Citizen Download
```

The PDF can contain:

-   Property ID
-   Khasra number
-   Owner information appropriate for the document
-   Property details
-   Registration information
-   Document hash
-   Blockchain transaction reference
-   Current status
-   Relevant timestamps

------------------------------------------------------------------------

# 16. Property Lifecycle

``` text
                 +----------------+
                 |    Pending     |
                 +-------+--------+
                         |
                         v
                 +----------------+
                 |    Verified    |
                 +-------+--------+
                         |
                         v
                 +----------------+
                 |     Active     |
                 +---+--------+---+
                     |        |
             Freeze  |        | Dispute
                     v        v
                 +------+  +---------+
                 |Frozen|  |Disputed |
                 +--+---+  +----+----+
                    |           |
                    +----+------+
                         |
                         v
                       Active
```

Ownership transfer/mutation occurs as a controlled transition of an
active property.

------------------------------------------------------------------------

# 17. Data Storage Strategy

  -----------------------------------------------------------------------
  Data                    Storage                 On-Chain Representation
  ----------------------- ----------------------- -----------------------
  User profile            PostgreSQL              None

  KYC/identity data       Secure off-chain        Hash/reference only
                          storage                 where required

  Owner photo             Secure off-chain        Photo hash where
                          storage                 required

  Land/Khasra records     PostgreSQL              Selected property
                                                  references/hashes

  Property metadata       PostgreSQL              Metadata hash

  Sale deed / property    IPFS/S3/secure storage  Document hash
  documents                                       

  Applications            PostgreSQL              None

  Appointments            PostgreSQL              None

  Payment records         PostgreSQL/payment      None
                          provider                

  Audit logs              PostgreSQL              Blockchain tx/event
                                                  references

  e-Registry PDF          Off-chain               Transaction/hash
                                                  references

  Property ID             PostgreSQL + blockchain Property ID

  Current owner           Application +           Owner address
  blockchain identity     blockchain              

  Ownership history       PostgreSQL projection + Ownership events
                          blockchain              

  Registration timestamp  PostgreSQL + blockchain Timestamp

  Property status         PostgreSQL + blockchain Contract status
                          where applicable        
  -----------------------------------------------------------------------

------------------------------------------------------------------------

# 18. Database Architecture

The relational database should support the application's operational
workflow.

A logical schema includes:

``` text
users
roles
user_roles

properties
property_owners
transfers
transfer_status_history

documents
blockchain_transactions
blockchain_events

applications
appointments
kyc_records
verification_records
payments
mutations

audit_logs
notifications

registry_records
analytics / reporting data
alerts
```

## 18.1 Core Relationships

``` text
User
 |
 +---- Applications
 |
 +---- KYC
 |
 +---- Roles
 |
 +---- Audit Logs
 |
 +---- Notifications

Property
 |
 +---- Property Owner(s)
 |
 +---- Documents
 |
 +---- Applications
 |
 +---- Transfers
 |
 +---- Mutations
 |
 +---- Blockchain Transactions
 |
 +---- Blockchain Events
 |
 +---- Registry Records
```

The exact physical schema can evolve during implementation, but
relational links between property, owner, application, transaction, and
mutation data should remain explicit.

------------------------------------------------------------------------

# 19. API Architecture

The backend is organized around REST-style modules.

Recommended API groups:

``` text
/api/auth
/api/users
/api/land
/api/properties
/api/applications
/api/appointments
/api/kyc
/api/documents
/api/verification
/api/payments
/api/registry
/api/mutations
/api/ownership
/api/blockchain
/api/analytics
/api/audit
```

Examples:

``` text
POST   /api/auth/login
POST   /api/auth/verify-otp

GET    /api/land/search?khasraNumber=...
GET    /api/properties/:propertyId

POST   /api/applications
GET    /api/applications/:id

POST   /api/appointments
GET    /api/appointments/:id

POST   /api/documents
POST   /api/verification/:applicationId

POST   /api/payments
GET    /api/payments/:id

POST   /api/registry/:applicationId/approve
POST   /api/mutations
GET    /api/ownership/:propertyId/history

GET    /api/blockchain/transactions/:propertyId
GET    /api/analytics/*
GET    /api/audit/*
```

All endpoints must be protected according to role and jurisdiction.

------------------------------------------------------------------------

# 20. Security Architecture

## 20.1 Authentication

Use:

-   Secure login
-   OTP where required
-   JWT or secure session mechanism
-   Strong password hashing where passwords are used
-   Session expiry
-   Secure cookies where applicable

## 20.2 Authorization

Use RBAC for:

``` text
Citizen
Local Authority
Authorized Registrar
Government HQ
Admin
```

Local government users should be restricted to the appropriate
jurisdiction wherever applicable.

## 20.3 Blockchain Security

-   Smart-contract role checks
-   Registrar-only final authorization
-   Backend-controlled transaction flow
-   No private keys in frontend code
-   Private keys stored in environment secrets/secrets manager
-   Transaction logging
-   Contract-address/network validation

## 20.4 Data Security

Sensitive information must remain off-chain.

Examples:

-   Identity documents
-   Aadhaar/KYC information
-   Phone numbers
-   Residential addresses
-   Private documents
-   Payment credentials
-   Private keys

## 20.5 Environment Secrets

Never commit real secrets to Git.

Use `.env` locally and a secure secrets manager in production.

------------------------------------------------------------------------

# 21. Environment Configuration

Example backend configuration:

``` env
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

Frontend configuration:

``` env
VITE_API_BASE_URL=http://localhost:5000/api
VITE_BLOCKCHAIN_CHAIN_ID=31337
VITE_LAND_REGISTRY_CONTRACT_ADDRESS=0x...
```

**Do not place private keys, payment secrets, or server-side credentials
in `VITE_*` variables.**

------------------------------------------------------------------------

# 22. Recommended Project Structure

``` text
BHUMI/
├── frontend/
│   ├── public/
│   └── src/
│       ├── components/
│       ├── pages/
│       │   ├── auth/
│       │   ├── citizen/
│       │   └── government/
│       │       ├── local/
│       │       └── hq/
│       ├── hooks/
│       ├── services/
│       ├── api/
│       ├── routes/
│       ├── types/
│       ├── utils/
│       └── App.tsx
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
│       │   ├── landSearch/
│       │   ├── appointment/
│       │   ├── kyc/
│       │   ├── documents/
│       │   ├── verification/
│       │   ├── registry/
│       │   ├── mutation/
│       │   ├── payments/
│       │   ├── hashing/
│       │   ├── blockchain/
│       │   ├── analytics/
│       │   └── audit/
│       └── utils/
│
├── contracts/
│   ├── src/
│   │   └── LandRegistry.sol
│   ├── script/
│   └── test/
│
├── docs/
│   ├── ARCHITECTURE_AND_WORKFLOW.md
│   └── assets/
│       └── bhumi-system-architecture.png
│
├── storage/
├── .env.example
├── docker-compose.yml
├── package.json
└── README.md
```

------------------------------------------------------------------------

# 23. End-to-End System Workflow

The complete system can be summarized as:

``` text
Citizen
  |
  v
Login / OTP
  |
  v
Search Khasra
  |
  v
View / Verify Land
  |
  v
Book Appointment
  |
  v
Upload Documents + KYC
  |
  v
INR Payment
  |
  v
Application Created
  |
  v
Local Authority Verification
  |
  +------ Rejected ------> Resubmit
  |
  +------ Approved
              |
              v
       Registry Processing
              |
              v
          SHA-256
              |
              v
      Authorized Registrar
              |
              v
       LandRegistry.sol
              |
              v
          Blockchain
              |
              v
       Confirmed Event
              |
              v
      Backend Event Listener
              |
              v
     PostgreSQL Synchronization
              |
              v
      Ownership History Updated
              |
              v
       e-Registry PDF
              |
              v
       Citizen / Government
```

------------------------------------------------------------------------

# 24. Error and Failure Handling

The system should explicitly handle:

### Payment Failure

``` text
Payment Failed
   |
   v
Application remains unpaid/pending
   |
   v
Citizen retries payment
```

### Document Verification Failure

``` text
Invalid / incomplete document
   |
   v
Verification rejected
   |
   v
Citizen receives status/reason
   |
   v
Resubmission
```

### KYC Failure

``` text
KYC failed
   |
   v
Application cannot proceed to final approval
```

### Blockchain Transaction Failure

``` text
Registrar submits transaction
       |
       v
Transaction fails
       |
       v
Record failure + transaction/error information
       |
       v
Do not mark registration as blockchain-confirmed
```

### Hash Mismatch

``` text
Stored document
      |
      v
Generate current SHA-256
      |
      v
Compare with committed hash
      |
      +---- Match ----> Integrity verified
      |
      +---- Mismatch -> Flag for investigation
```

### Database/Blockchain Desynchronization

Use transaction/event tracking and blockchain event listeners to
reconcile PostgreSQL with confirmed blockchain events.

------------------------------------------------------------------------

# 25. Testing Strategy

## 25.1 Unit Tests

Test:

-   Authentication
-   RBAC
-   Land search
-   Application creation
-   Appointment handling
-   Document hashing
-   Verification logic
-   Payment state handling
-   Mutation logic

## 25.2 Smart Contract Tests

Test:

-   Property registration
-   Ownership transfer
-   Role restrictions
-   Freeze/unfreeze
-   Dispute handling
-   Invalid property IDs
-   Unauthorized calls
-   Event emission

## 25.3 Integration Tests

Test:

``` text
React
  -> Express API
  -> PostgreSQL
  -> Document Storage
  -> Hashing
  -> Smart Contract
  -> Blockchain Event
  -> PostgreSQL Synchronization
```

## 25.4 Failure Tests

Explicitly test:

-   Payment failure
-   KYC failure
-   Invalid document
-   Rejected application
-   Blockchain transaction failure
-   Hash mismatch
-   Duplicate/conflicting ownership attempt
-   Backend restart during pending blockchain confirmation
-   Blockchain event synchronization

------------------------------------------------------------------------

# 26. Deployment Architecture

## 26.1 MVP / Development

The project can be developed using:

``` text
React + Vite
       |
Node.js + Express
       |
PostgreSQL
       |
Local Hardhat/Ganache-style EVM Network
```

This avoids real blockchain gas costs during development.

## 26.2 Prototype Testnet

A public EVM testnet such as Sepolia may be used for prototype
demonstration where appropriate.

The Registrar wallet should be funded with test ETH, and its private key
must remain server-side.

## 26.3 Production Direction

A production deployment would require:

-   Official government land-record integration
-   Legal and policy approval
-   Government identity/KYC integration
-   Production payment integration
-   Secure key management
-   High-availability infrastructure
-   Monitoring and alerting
-   Backup and disaster recovery
-   Appropriate permissioned/public blockchain architecture
-   Formal security auditing
-   Data protection and retention controls

------------------------------------------------------------------------

# 27. Git and Repository Workflow

The project should follow a branch-based workflow.

``` text
main
 |
 +-- feature/auth
 +-- feature/land-search
 +-- feature/registry
 +-- feature/mutation
 +-- feature/smart-contract
 +-- feature/frontend-citizen
 +-- feature/government-dashboard
```

Recommended process:

1.  Pull the latest `main`.
2.  Create a feature branch.
3.  Make the required changes.
4.  Test locally.
5.  Review the changes.
6.  Commit with a meaningful message.
7.  Push the branch.
8.  Open a pull request.
9.  Review and merge into `main`.

Do not push feature work directly to `main`.

------------------------------------------------------------------------

# 28. MVP Implementation Priorities

The source architecture identifies the core MVP workflow as the
priority.

### Layer 1 --- Backend and Database

-   PostgreSQL schema
-   Node.js/Express APIs
-   Authentication
-   Basic CRUD
-   Property and owner relationships

### Layer 2 --- Smart Contract

-   `LandRegistry.sol`
-   Role-based access control
-   Property registration
-   Ownership transfer
-   Event emission
-   Local testnet deployment
-   Contract tests

### Layer 3 --- Dashboards

-   Citizen portal
-   Government/Registrar dashboard
-   Application status
-   Verification workflow

### Layer 4 --- Integration

-   Ethers.js
-   Backend-to-contract communication
-   Blockchain event listener
-   PostgreSQL synchronization

The project should first demonstrate a successful end-to-end property
registration/transfer and mutation event before expanding into advanced
features.

------------------------------------------------------------------------

# 29. Future Scope

Potential future extensions include:

-   GIS/map integration
-   AI/OCR-assisted document processing
-   Duplicate-property detection
-   Fraud/anomaly detection
-   Mobile application
-   Government API interoperability
-   Digital identity integration
-   SMS/email notifications
-   Multi-state deployment
-   Rural/offline-friendly workflows
-   Broader institutional interoperability
-   Advanced analytics

These features should remain secondary to the core MVP workflow.

------------------------------------------------------------------------

# 30. Known Engineering Challenges

## 30.1 Dual-State Synchronization

PostgreSQL and blockchain must remain consistent.

## 30.2 Smart Contract Immutability

Contract logic must be tested thoroughly before deployment because
deployed smart-contract logic is difficult to change.

## 30.3 Asynchronous Blockchain UX

Blockchain confirmation may take time.

The frontend should display:

``` text
Pending
Processing
Transaction Submitted
Confirmation Waiting
Confirmed
Failed
```

The UI should expose the transaction reference where appropriate rather
than immediately claiming completion after submission.

## 30.4 Key Management

Registrar/private keys must never be exposed to the browser or committed
to the repository.

## 30.5 Scope Control

AI, advanced GIS, drone mapping, and other advanced features should not
distract from the MVP until the core registration → transfer → mutation
flow is functioning.

------------------------------------------------------------------------

# 31. Example Property Record

A representative application-level property record:

``` json
{
  "propertyId": "PROP-MP-BPL-001",
  "khasraNumber": "123/4",
  "district": "Bhopal",
  "tehsil": "Huzur",
  "village": "Example Village",
  "area": "1500 sq.ft",
  "landType": "Residential",
  "status": "VERIFIED"
}
```

A corresponding blockchain-oriented record can contain:

``` text
Property ID
Current Owner
Document Hash
Owner Photo Hash
Metadata Hash
Registered At
Last Transfer At
Status
```

------------------------------------------------------------------------

# 32. Architectural Boundaries

The following boundaries are important:

### Boundary 1 --- Sensitive Data

Sensitive personal and identity data stays off-chain.

### Boundary 2 --- Blockchain Writes

Only authorized backend/Registrar workflows perform final blockchain
writes.

### Boundary 3 --- Application Database

PostgreSQL is the operational system used for application queries,
workflow state, and user-facing operations.

### Boundary 4 --- Blockchain Ledger

The blockchain is used for finalized cryptographic proof, ownership
state/history, and auditability.

### Boundary 5 --- Legal Record

The prototype must not be represented as a replacement for legally
authoritative government land records.

------------------------------------------------------------------------

# 33. Final Architecture Summary

B.H.U.M.I. combines a conventional government-oriented web application
with a blockchain trust layer.

``` text
                    B.H.U.M.I.
                        |
        +---------------+---------------+
        |                               |
   Web Application                 Government Portal
        |                               |
        +---------------+---------------+
                        |
                 Node.js / Express
                        |
        +---------------+---------------+
        |               |               |
   PostgreSQL     Secure Storage    External Services
        |               |               |
        +---------------+---------------+
                        |
                 Verification Layer
                        |
                    SHA-256
                        |
              Authorized Registrar
                        |
                 LandRegistry.sol
                        |
                   Blockchain
                        |
              Immutable Audit Trail
                        |
        +---------------+---------------+
        |               |               |
     Citizen       Government        HQ Analytics
     Access          Access             / Audit
```

The architecture is intentionally hybrid:

-   **Web2 handles speed, workflow, sensitive information, and
    operational records.**
-   **Cryptographic hashing provides document integrity evidence.**
-   **Web3 provides finalized ownership events and a tamper-evident
    audit layer.**
-   **Government authorization remains part of the registration and
    mutation process.**
-   **The backend synchronizes confirmed blockchain events back into the
    application database.**

This separation keeps the prototype practical while preserving the
central B.H.U.M.I. objective: a unified, transparent, auditable
property-registration and mutation workflow.

------------------------------------------------------------------------

## 34. Reference Documents

This architecture consolidates the project's existing architecture and
workflow materials. The repository should treat this file as the single
high-level technical reference, while detailed implementation notes may
be maintained in separate documents as the project grows.

### Suggested `docs/` structure

``` text
docs/
├── ARCHITECTURE_AND_WORKFLOW.md
├── SYSTEM_ARCHITECTURE_DIAGRAM.md
├── api/
├── database/
├── blockchain/
├── workflows/
└── decisions/
```

------------------------------------------------------------------------

**B.H.U.M.I. --- Blockchain Hosted Unified Mutation Infrastructure**\
*Technical Architecture & System Workflow*
