---
title: Components
date: 2024-02-17
weight: 2
---

### 1. Blockchain Layer
The blockchain layer serves as the control plane for the entire system, managing access rights and verifying identities.

#### Supported Platforms
- **Ethereum**: Smart contract-based access control and identity management
- **Sui Move**: High-performance transaction processing
- **REST API**: Development and testing environment

#### Key Responsibilities
- Access control management
- Transaction verification
- Smart contract execution
- State management

### 2. Identity Management
Built on Veramo, the identity management system provides robust DID capabilities.

#### Features
- Multiple DID method support
- Verifiable credential issuance
- Identity verification
- Key management

#### Components
- DID Provider
- Credential Store
- Key Manager
- DID Resolver

### 3. P2P Networking
The P2P networking layer enables direct communication between nodes.

#### Capabilities
- Peer discovery
- Data sharing
- Real-time messaging
- Network resilience

#### Implementation
- GUN protocol integration
- Distributed hash table (DHT)
- NAT traversal
- Connection management

### 4. Data Storage
Flexible data storage system supporting multiple backend options.

#### Features
- Pluggable storage backends
- Encrypted data storage
- Access control integration
- Data versioning

#### Storage Options
- File-based storage
- Distributed storage
- Encrypted storage
- Temporary storage