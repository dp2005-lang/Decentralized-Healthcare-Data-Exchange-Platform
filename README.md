# Decentralized-Healthcare-Data-Exchange-Platform
A blockchain-based healthcare data exchange platform enabling patient-controlled consent, secure record sharing, hash-based integrity verification, role-based access control, off-chain storage, and immutable audit trails using synthetic healthcare data.
# Decentralized Healthcare Data Exchange Platform

## Overview

The **Decentralized Healthcare Data Exchange Platform** is a blockchain-based healthcare data-sharing prototype designed to provide **patient-controlled consent, role-based access control, record integrity verification, and an immutable audit trail**.

The project uses blockchain primarily for storing **record hashes, metadata, permissions, consent information, and audit events**, while actual healthcare documents remain off-chain. This approach avoids placing sensitive medical information directly on a public blockchain.

> **Important:** This project uses only synthetic/dummy healthcare data and is intended for educational purposes.

---

## Problem Statement

Healthcare records are often distributed across hospitals, laboratories, doctors, insurance systems, and patient portals. Centralized systems can create challenges involving:

* Data silos
* Limited interoperability
* Unauthorized access
* Lack of transparent consent management
* Difficulty verifying record integrity
* Duplicate records and testing
* Limited visibility into data-access history

This project demonstrates how blockchain can provide a transparent and auditable mechanism for managing access permissions and verifying healthcare-record integrity.

---

## Objectives

The main objectives are to:

* Implement blockchain-based healthcare record management.
* Give patients control over record access.
* Implement patient-to-doctor consent management.
* Provide role-based access control.
* Store medical-record hashes on-chain.
* Keep actual healthcare documents off-chain.
* Detect document tampering through hash verification.
* Generate an immutable audit trail.
* Demonstrate the complete workflow using synthetic data.

---

## Core Workflow

```text
Hospital / Patient Creates Record
              ↓
       Record Stored Off-Chain
              ↓
       SHA-256 Hash Generated
              ↓
 Hash + Metadata Stored On-Chain
              ↓
       Doctor Requests Access
              ↓
       Patient Grants Consent
              ↓
 Smart Contract Verifies Permission
              ↓
      Authorized Access Granted
              ↓
       Patient Revokes Access
              ↓
 Future Unauthorized Access Denied
```

This follows the project's intended architecture of separating sensitive medical documents from blockchain-stored references, hashes, metadata, and permissions.

---

## Actors

| Actor                 | Responsibilities                                         |
| --------------------- | -------------------------------------------------------- |
| **Patient**           | Owns records, grants/revokes access and controls consent |
| **Doctor / Provider** | Requests and accesses authorized records                 |
| **Hospital**          | Registers verified medical-record metadata               |
| **Admin**             | Optionally verifies and manages participants             |

The patient remains the primary authority for granting and revoking access to their records.

---

## Architecture

```text
                   ┌─────────────────────┐
                   │      PATIENT        │
                   │ Consent Controller  │
                   └──────────┬──────────┘
                              │
                              ↓
┌──────────────┐     ┌─────────────────────────┐     ┌──────────────────┐
│   HOSPITAL   │────→│   BLOCKCHAIN NETWORK    │←────│ DOCTOR / PROVIDER│
└──────────────┘     │                         │     └──────────────────┘
                     │ Role Registry           │
                     │ Medical Record Registry │
                     │ Consent Manager         │
                     │ Audit Events            │
                     └────────────┬────────────┘
                                  │
                                  ↓
                     ┌─────────────────────────┐
                     │    OFF-CHAIN STORAGE    │
                     │                         │
                     │ Synthetic JSON / PDF    │
                     │ Encrypted Records       │
                     │ IPFS / Local Storage    │
                     └─────────────────────────┘
```

The architecture separates the blockchain layer, frontend dashboards, and off-chain storage layer.

---

## On-Chain vs Off-Chain Data

### Stored On-Chain

The blockchain stores limited metadata such as:

* Record ID
* Patient wallet address
* Record type
* File hash
* Timestamp
* Storage reference
* Access permissions
* Consent records
* Audit events

### Stored Off-Chain

The following should remain off-chain:

* Complete medical reports
* Diagnoses
* Personal identification information
* Confidential medical documents
* Large healthcare files

This minimizes unnecessary exposure of sensitive information on a public blockchain.

---

## Medical Record Model

A medical record can contain:

```text
recordId
patient
createdBy
recordType
fileHash
storageReference
timestamp
active
```

Supported example record types include:

* Prescription
* Lab Report
* Imaging
* Discharge Summary
* Vaccination Record
* General Medical Record

---

## Consent Management

The platform implements a patient-controlled consent workflow.

### Grant Access

```text
Patient → grantAccess() → Doctor
```

The patient explicitly authorizes a doctor to access a particular record.

### Revoke Access

```text
Patient → revokeAccess() → Doctor
```

After revocation, future access attempts should fail.

### Permission Verification

```text
Doctor Request
      ↓
Smart Contract
      ↓
Has Permission?
   ↙       ↘
 YES       NO
 ↓          ↓
ALLOW      DENY
```

Every important consent action is recorded through blockchain events.

---

## Hash-Based Integrity Verification

The project uses hashing to detect modifications to off-chain files.

Example:

```text
Original File
     ↓
SHA-256
     ↓
9e8e8d4051e00940...
     ↓
Stored / Compared
```

If the document is modified:

```text
Modified File
     ↓
SHA-256
     ↓
Different Hash
     ↓
TAMPERING DETECTED
```

Therefore, the blockchain does not need to store the complete medical document to verify its integrity.

---

## Smart Contract

The project architecture includes a Solidity `ConsentRegistry` contract supporting:

* Participant registration
* Patient-owned asset registration
* Access requests
* Consent granting
* Consent revocation
* Consent expiry
* Access verification
* Blockchain events

The reference implementation uses Solidity `^0.8.20`.

### Major Events

```solidity
ParticipantRegistered
AssetRegistered
AccessRequested
ConsentGranted
ConsentRevoked
ConsentExpired
```

These events provide an auditable record of important state changes.

---

## Technology Stack

### Blockchain

* Solidity
* Ethereum-compatible blockchain
* Hardhat
* Remix IDE
* Ethers.js

### Frontend

* React
* Ethers.js
* MetaMask

### Storage

* Local off-chain storage
* IPFS concept/simulation
* Encrypted files

### Data

* JSON
* Synthetic healthcare records
* SHA-256 hashes
* FHIR-compatible concepts

The recommended student stack is Solidity + Hardhat + Ethers.js + MetaMask + React with local blockchain/testnet and simulated IPFS storage.

---

## Project Structure

```text
Decentralized-Healthcare-Data-Exchange/
│
├── contracts/
│   └── HealthcareDataExchange.sol
│
├── scripts/
│   └── deploy.js
│
├── test/
│   └── HealthcareDataExchange.test.js
│
├── frontend/
│   ├── src/
│   └── components/
│
├── sample_records/
│
├── hashes/
│
├── screenshots/
│
├── reports/
│
├── docs/
│
├── README.md
├── hardhat.config.js
├── package.json
└── .gitignore
```

---

## Installation

### 1. Clone the Repository

```bash
git clone https://github.com/YOUR_USERNAME/Decentralized-Healthcare-Data-Exchange.git
cd Decentralized-Healthcare-Data-Exchange
```

### 2. Initialize Node Project

```bash
npm init -y
```

### 3. Install Hardhat

```bash
npm install --save-dev hardhat
```

For the recommended setup:

```bash
npm install --save-dev hardhat @nomicfoundation/hardhat-toolbox
```

### 4. Initialize Hardhat

```bash
npx hardhat
```

### 5. Compile

```bash
npx hardhat compile
```

---

## Local Blockchain

Start a local blockchain with:

```bash
npx hardhat node
```

This provides local accounts that can be used to simulate:

```text
Account 1 → Admin
Account 2 → Patient
Account 3 → Doctor
Account 4 → Hospital
```

No real cryptocurrency is required for the educational local simulation.

---

## Testing

Run:

```bash
npx hardhat test
```

Important test scenarios include:

1. Patient registration
2. Doctor registration
3. Hospital registration
4. Unauthorized role registration
5. Hospital record registration
6. Invalid patient address
7. Unauthorized record creation
8. Doctor access without permission
9. Patient grants permission
10. Authorized doctor access
11. Patient revokes permission
12. Doctor access after revocation
13. Invalid record ID
14. Hash verification
15. Event emission

The project specifically recommends testing registration, authorization, consent, revocation, record integrity, and event generation.

---

## Remix Simulation

The project can also be demonstrated using Remix IDE.

### Demonstration Accounts

```text
Account 1 → Patient
Account 2 → Doctor
Account 3 → Hospital
```

### Demo Workflow

```text
Deploy Contract
      ↓
Register Patient
      ↓
Register Doctor
      ↓
Register Hospital
      ↓
Hospital Adds Dummy Record
      ↓
Doctor Attempts Access
      ↓
ACCESS DENIED
      ↓
Patient Grants Access
      ↓
Doctor Accesses Record
      ↓
ACCESS ALLOWED
      ↓
Patient Revokes Access
      ↓
Doctor Attempts Access Again
      ↓
ACCESS DENIED
```

This is the intended Remix demonstration flow described in the project specification.

---

## Synthetic Data

This repository must use **dummy/synthetic healthcare information only**.

Example:

```json
{
    "patient_id": "P001",
    "record_type": "Lab Report",
    "date": "2026-08-26",
    "test": "Blood Test",
    "result": "Normal"
}
```

No real patient records should be uploaded to this repository.

---

## Security & Privacy

Healthcare information is highly sensitive. This project therefore follows a privacy-oriented design:

* Do not store complete medical records on a public blockchain.
* Store hashes rather than plaintext documents.
* Keep healthcare documents off-chain.
* Use encryption for real-world implementations.
* Use role-based access control.
* Require patient consent.
* Log important actions through events.
* Minimize personally identifiable information.
* Consider secure key management for production systems.

A blockchain provides transparency and auditability, but **blockchain itself does not automatically make healthcare data private**.

---

## Limitations

This project is an educational prototype and does not represent a production healthcare platform.

Limitations include:

* Synthetic data only
* Simulated off-chain/IPFS storage
* Simplified identity management
* Simplified encryption model
* No production hospital integration
* No complete regulatory compliance implementation
* Revocation cannot erase data already downloaded by an authorized user
* Key-management infrastructure is not production-ready

---

## Future Improvements

Possible future enhancements include:

* Encrypted IPFS storage
* Decentralized identity / DID
* Time-limited consent
* Purpose-bound permissions
* FHIR interoperability
* Multi-hospital integration
* Emergency access workflow
* Proxy re-encryption
* Hardware-backed key management
* Zero-knowledge proofs
* Production React dashboard
* Blockchain event indexing
* Secure audit analytics

The project's advanced design also identifies encrypted storage, FHIR mapping, decentralized identity, proxy re-encryption, and privacy-preserving technologies as future directions.

---

## GitHub Topics

Recommended repository topics:

```text
blockchain
solidity
ethereum
healthcare
healthtech
web3
smart-contract
hardhat
ethersjs
access-control
consent-management
ipfs
dapp
data-privacy
```

---

## Git Commit Strategy

Recommended commits:

```bash
git add .
git commit -m "Initialize healthcare blockchain project"

git commit -m "Add role-based user registry"

git commit -m "Implement medical record hash storage"

git commit -m "Add patient consent management"

git commit -m "Implement access revocation"

git commit -m "Add healthcare audit events"

git commit -m "Add Hardhat test suite"

git commit -m "Add Remix simulation proof"

git commit -m "Add off-chain record hash verification"

git commit -m "Complete README and documentation"
```

---

## Proof / Screenshots

Recommended screenshots for the project:

```text
01_Project_Folder.png
02_Solidity_Contract.png
03_Successful_Compilation.png
04_Contract_Deployment.png
05_Patient_Registration.png
06_Doctor_Registration.png
07_Hospital_Registration.png
08_Dummy_Record.png
09_File_Hash.png
10_Medical_Record_Added.png
11_Unauthorized_Access.png
12_Consent_Granted.png
13_Authorized_Access.png
14_Consent_Revoked.png
15_Access_Denied_After_Revocation.png
16_Event_Logs.png
17_Hardhat_Test_Results.png
18_Frontend_Dashboard.png
19_GitHub_Repository.png
20_README.png
```

These screenshots demonstrate the project's implementation and provide useful proof-of-work for GitHub and academic/project presentations.

---

## Industry Relevance

The concepts demonstrated by this project can be applied to:

* Hospital networks
* Diagnostic laboratories
* Insurance systems
* Telemedicine
* Electronic health records
* Clinical research
* Patient portals
* Healthcare interoperability
* Cross-network referrals
* Insurance pre-authorization
* Research data sharing

The project specifically identifies interoperability, verifiable consent, immutable audit trails, and controlled data sharing as major potential healthcare applications.

---

## Learning Outcomes

Through this project, the developer demonstrates knowledge of:

* Blockchain fundamentals
* Ethereum architecture
* Solidity
* Smart contracts
* Wallet addresses
* `msg.sender`
* Structs
* Mappings
* Enums
* Modifiers
* Events
* `require()`
* Hashing
* `keccak256`
* SHA-256
* Transaction concepts
* Access control
* Consent management
* Off-chain storage
* IPFS concepts
* Hardhat
* Ethers.js
* Web3 development

---

## Disclaimer

This project is an **educational blockchain prototype**.

It does not use real patient information and should not be deployed for actual healthcare-data processing without appropriate security engineering, encryption, identity management, regulatory review, compliance controls, and professional healthcare-system integration.

**Never upload real patient medical records to this educational repository.**

---

## Author

**Developed as a Blockchain Course Project**

**Project:** Decentralized Healthcare Data Exchange Platform

**Focus:** Blockchain • Healthcare • Web3 • Smart Contracts • Consent Management • Data Privacy • Hash Verification
