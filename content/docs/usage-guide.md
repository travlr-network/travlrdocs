---
title: Usage Guide
date: 2024-02-17
weight: 3
---

### Initializing TravlrNode

Initialize a new TravlrNode instance with basic configuration:

```typescript
import { runNode } from './runNode';

const initializeNode = async () => {
  const node = await runNode();
  return node;
};
```

### Basic Node Operations

#### 1. Creating a DID
```typescript
// Create a new DID for an individual
const createUserIdentity = async (veramo) => {
  const identifier = await veramo.createDID({
    isOrganization: false
  });
  console.log('Created DID:', identifier.did);
  return identifier;
};

// Create a new DID for an organization
const createOrgIdentity = async (veramo) => {
  const identifier = await veramo.createDID({
    isOrganization: true
  });
  return identifier;
};
```

#### 2. Managing Access Control
```typescript
// Grant access to data
const grantDataAccess = async (accessControl, granterDID, granteeDID, dataKey) => {
  const expirationTime = Date.now() + (24 * 60 * 60 * 1000); // 24 hours
  await accessControl.grantAccess(granterDID, granteeDID, dataKey, expirationTime);
};

// Revoke access
const revokeDataAccess = async (accessControl, revokerDID, granteeDID, dataKey) => {
  await accessControl.revokeAccess(revokerDID, granteeDID, dataKey);
};
```

## Data Management

### Storing Data

#### 1. Basic Data Storage
```typescript
// Store data with encryption
const storeData = async (dataStore, key, value) => {
  await dataStore.setData(key, {
    value,
    timestamp: Date.now(),
    metadata: {
      encrypted: true,
      version: '1.0'
    }
  });
};
```

#### 2. Retrieving Data
```typescript
// Fetch stored data
const getData = async (dataStore, key) => {
  const data = await dataStore.getData(key);
  return data;
};
```

### Data Sharing

#### 1. P2P Data Transfer
```typescript
// Send data to peer
const shareData = async (p2pNode, peerId, dataKey, data) => {
  await p2pNode.sendData(peerId, dataKey, data);
};

// Receive data from peer
const receiveData = async (p2pNode, dataKey) => {
  const data = await p2pNode.receiveData(dataKey);
  return data;
};
```

## Advanced Features

### Verifiable Credentials

#### 1. Issuing Credentials
```typescript
// Issue a travel credential
const issueTravelCredential = async (veramo, issuerDID, subjectDID, travelData) => {
  const credential = await veramo.issueVC(issuerDID, subjectDID, {
    type: ['VerifiableCredential', 'TravelCredential'],
    travelData
  });
  return credential;
};
```

#### 2. Verifying Credentials
```typescript
// Verify a travel credential
const verifyCredential = async (veramo, credential) => {
  const verification = await veramo.verifyVC(credential);
  return verification;
};
```

### Delegation Management

#### 1. Setting Up Delegation
```typescript
// Add delegation rights
const addDelegation = async (blockchain, fromDID, toDID, dataKey) => {
  await blockchain.addDelegation(fromDID, toDID, dataKey);
};

// Remove delegation rights
const removeDelegation = async (blockchain, fromDID, toDID, dataKey) => {
  await blockchain.removeDelegation(fromDID, toDID, dataKey);
};
```

## Integration Examples

### 1. Travel Service Provider Integration
```typescript
const setupTravelProvider = async () => {
  // Initialize node
  const node = await runNode();
  
  // Create organization DID
  const orgIdentity = await node.veramo.createDID({
    isOrganization: true
  });

  // Setup data storage
  const dataStore = node.dataStore;
  
  // Setup access control
  const accessControl = node.accessControl;
  
  return {
    node,
    orgIdentity,
    dataStore,
    accessControl
  };
};
```

### 2. Customer Data Management
```typescript
const manageCustomerData = async (node, orgDID, customerDID, customerData) => {
  // Store customer data
  const dataKey = `customer_${customerDID}`;
  await node.dataStore.setData(dataKey, customerData);
  
  // Grant temporary access
  const expirationTime = Date.now() + (7 * 24 * 60 * 60 * 1000); // 7 days
  await node.accessControl.grantAccess(
    orgDID,
    customerDID,
    dataKey,
    expirationTime
  );
};
```

## Best Practices

### 1. Security Guidelines

#### Key Management
```typescript
// Secure key storage
const secureKeyStorage = {
  storeKey: async (key, value) => {
    // Implement secure storage
  },
  retrieveKey: async (key) => {
    // Implement secure retrieval
  }
};
```

#### Access Control
```typescript
// Regular access review
const reviewAccess = async (accessControl, dataKey) => {
  const accessList = await accessControl.listAccess(dataKey);
  const now = Date.now();
  
  for (const access of accessList) {
    if (access.expirationTime < now) {
      await accessControl.revokeAccess(
        access.granterDID,
        access.granteeDID,
        dataKey
      );
    }
  }
};
```

### 2. Performance Optimization

#### Caching Strategy
```typescript
// Implement caching
const cache = new Map();

const getCachedData = async (dataStore, key) => {
  if (cache.has(key)) {
    return cache.get(key);
  }
  
  const data = await dataStore.getData(key);
  cache.set(key, data);
  return data;
};
```

### 3. Error Handling
```typescript
// Implement robust error handling
const safeOperation = async (operation) => {
  try {
    return await operation();
  } catch (error) {
    if (error instanceof BlockchainError) {
      // Handle blockchain errors
    } else if (error instanceof StorageError) {
      // Handle storage errors
    } else {
      // Handle other errors
    }
    throw error;
  }
};
```

## Monitoring and Maintenance

### 1. Health Checks
```typescript
const performHealthCheck = async (node) => {
  const status = {
    blockchain: await node.blockchain.getStatus(),
    p2p: await node.p2pNode.getStatus(),
    storage: await node.dataStore.getStatus(),
    did: await node.veramo.getStatus()
  };
  return status;
};
```

### 2. Performance Monitoring
```typescript
const monitorPerformance = async (node) => {
  const metrics = {
    activeConnections: await node.p2pNode.getPeerCount(),
    storageUsage: await node.dataStore.getUsage(),
    transactionCount: await node.blockchain.getTransactionCount(),
    lastBlockTime: await node.blockchain.getLastBlockTime()
  };
  return metrics;
};
```

## Troubleshooting

### Common Issues and Solutions
```typescript
const troubleshoot = async (node) => {
  // Check blockchain connection
  if (!await node.blockchain.isConnected()) {
    await node.blockchain.reconnect();
  }
  
  // Verify P2P connectivity
  if (await node.p2pNode.getPeerCount() === 0) {
    await node.p2pNode.reconnectToPeers();
  }
  
  // Validate storage access
  if (!await node.dataStore.isAccessible()) {
    await node.dataStore.repair();
  }
};
```