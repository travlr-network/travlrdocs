---
title: Examples
date: 2024-02-17
weight: 101
---

## Basic Usage Examples

### 1. Node Initialization
```typescript
// Initialize a basic TravlrNode instance
import { runNode } from './runNode';

async function initializeBasicNode() {
  try {
    const node = await runNode();
    console.log('Node initialized successfully');
    return node;
  } catch (error) {
    console.error('Failed to initialize node:', error);
    throw error;
  }
}
```

### 2. Identity Management
```typescript
// Create and manage identities
async function identityManagementExample(node: any) {
  // Create individual identity
  const userIdentifier = await node.veramo.createDID({
    isOrganization: false
  });
  console.log('User DID created:', userIdentifier.did);

  // Create organization identity
  const orgIdentifier = await node.veramo.createDID({
    isOrganization: true
  });
  console.log('Organization DID created:', orgIdentifier.did);

  // Issue a verifiable credential
  const credential = await node.veramo.issueVC(
    orgIdentifier.did,
    userIdentifier.did,
    {
      type: ['VerifiableCredential', 'TravelCredential'],
      travelData: {
        bookingReference: 'BR123456',
        validUntil: new Date('2024-12-31').toISOString()
      }
    }
  );

  return { userIdentifier, orgIdentifier, credential };
}
```

## Integration Examples

### 1. Travel Service Provider Integration
```typescript
// Example of a travel service provider implementation
async function travelServiceExample() {
  const node = await runNode();
  
  // Initialize service provider identity
  const serviceProvider = await node.veramo.createDID({
    isOrganization: true
  });

  class TravelServiceProvider {
    private node: any;
    private did: string;

    constructor(node: any, did: string) {
      this.node = node;
      this.did = did;
    }

    async createBooking(customerDID: string, bookingData: any) {
      // Store booking data
      const bookingId = `booking_${Date.now()}`;
      await this.node.dataStore.setData(bookingId, {
        ...bookingData,
        customerDID,
        providerDID: this.did,
        timestamp: new Date().toISOString()
      });

      // Grant access to customer
      await this.node.accessControl.grantAccess(
        this.did,
        customerDID,
        bookingId,
        Date.now() + (90 * 24 * 60 * 60 * 1000) // 90 days
      );

      // Issue booking credential
      const credential = await this.node.veramo.issueVC(
        this.did,
        customerDID,
        {
          type: ['VerifiableCredential', 'BookingCredential'],
          bookingId,
          bookingDetails: bookingData
        }
      );

      return { bookingId, credential };
    }
  }

  return new TravelServiceProvider(node, serviceProvider.did);
}
```

### 2. Customer Data Management
```typescript
// Example of customer data management
async function customerDataExample() {
  const node = await runNode();

  class CustomerDataManager {
    private node: any;

    constructor(node: any) {
      this.node = node;
    }

    async createCustomerProfile(customerDID: string, profileData: any) {
      const profileKey = `profile_${customerDID}`;
      
      // Store encrypted profile data
      await this.node.dataStore.setData(profileKey, {
        ...profileData,
        lastUpdated: new Date().toISOString()
      });

      return profileKey;
    }

    async shareProfileWithProvider(
      customerDID: string,
      providerDID: string,
      profileKey: string
    ) {
      // Grant temporary access
      const expirationTime = Date.now() + (24 * 60 * 60 * 1000); // 24 hours
      await this.node.accessControl.grantAccess(
        customerDID,
        providerDID,
        profileKey,
        expirationTime
      );

      // Create sharing credential
      const credential = await this.node.veramo.issueVC(
        customerDID,
        providerDID,
        {
          type: ['VerifiableCredential', 'ProfileSharingCredential'],
          profileKey,
          expirationTime
        }
      );

      return credential;
    }
  }

  return new CustomerDataManager(node);
}
```

## Advanced Examples

### 1. Multi-Party Data Sharing
```typescript
// Example of complex data sharing between multiple parties
async function multiPartyDataSharingExample() {
  const node = await runNode();

  class TravelDataNetwork {
    private node: any;

    constructor(node: any) {
      this.node = node;
    }

    async setupDataSharing(
      airline: string,
      hotel: string,
      customer: string,
      bookingData: any
    ) {
      // Create shared booking record
      const bookingKey = `booking_${Date.now()}`;
      await this.node.dataStore.setData(bookingKey, bookingData);

      // Grant access to all parties
      const expirationTime = Date.now() + (7 * 24 * 60 * 60 * 1000); // 7 days
      
      await Promise.all([
        this.node.accessControl.grantAccess(airline, hotel, bookingKey, expirationTime),
        this.node.accessControl.grantAccess(airline, customer, bookingKey, expirationTime),
        this.node.accessControl.grantAccess(hotel, customer, bookingKey, expirationTime)
      ]);

      // Issue credentials to all parties
      const credentials = await Promise.all([
        this.node.veramo.issueVC(airline, hotel, {
          type: ['VerifiableCredential', 'BookingSharingCredential'],
          bookingKey
        }),
        this.node.veramo.issueVC(airline, customer, {
          type: ['VerifiableCredential', 'BookingSharingCredential'],
          bookingKey
        })
      ]);

      return { bookingKey, credentials };
    }
  }

  return new TravelDataNetwork(node);
}
```

### 2. Blockchain Integration
```typescript
// Example of blockchain-specific features
async function blockchainExample() {
  const node = await runNode();

  class BlockchainManager {
    private node: any;

    constructor(node: any) {
      this.node = node;
    }

    async registerTravelDocument(
      ownerDID: string,
      documentHash: string,
      metadata: any
    ) {
      // Register document on blockchain
      const transaction = await this.node.blockchain.registerDocument(
        ownerDID,
        documentHash,
        metadata
      );

      // Create verifiable credential
      const credential = await this.node.veramo.issueVC(
        ownerDID,
        ownerDID,
        {
          type: ['VerifiableCredential', 'DocumentRegistration'],
          documentHash,
          transactionHash: transaction.hash,
          timestamp: new Date().toISOString()
        }
      );

      return { transaction, credential };
    }

    async verifyDocument(documentHash: string) {
      const registration = await this.node.blockchain.getDocumentRegistration(
        documentHash
      );
      
      if (!registration) {
        throw new Error('Document not registered');
      }

      return registration;
    }
  }

  return new BlockchainManager(node);
}
```

### 3. P2P Data Synchronization
```typescript
// Example of P2P data synchronization
async function p2pSyncExample() {
  const node = await runNode();

  class P2PDataSync {
    private node: any;

    constructor(node: any) {
      this.node = node;
    }

    async syncTravelData(peerId: string, dataKey: string) {
      // Check access rights
      const hasAccess = await this.node.accessControl.hasAccess(
        peerId,
        dataKey
      );

      if (!hasAccess) {
        throw new Error('Access denied');
      }

      // Get data
      const data = await this.node.dataStore.getData(dataKey);

      // Send data to peer
      await this.node.p2pNode.sendData(peerId, dataKey, data);

      // Log sync event
      await this.node.dataStore.setData(`sync_${dataKey}_${Date.now()}`, {
        peerId,
        dataKey,
        timestamp: new Date().toISOString()
      });
    }

    async subscribeToPeerUpdates(peerId: string) {
      return this.node.p2pNode.subscribe(peerId, async (update: any) => {
        console.log('Received peer update:', update);
        // Handle update
      });
    }
  }

  return new P2PDataSync(node);
}