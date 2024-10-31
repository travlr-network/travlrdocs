---
title: API reference
date: 2024-02-17
weight: 100
---

### REST API

#### Node Management
```typescript
POST /register-node
{
    "did": string,
    "isOrganization": boolean
}

POST /grant-access
{
    "granterDID": string,
    "granteeDID": string,
    "dataKey": string,
    "expirationTime": number
}
```

#### Data Management
```typescript
POST /store-data
{
    "did": string,
    "dataKey": string,
    "data": any
}

GET /get-data/:dataKey
Authorization: Bearer <token>
```

### WebSocket Events

#### Connection Events
- `peer:connect`: New peer connection
- `peer:disconnect`: Peer disconnection
- `data:sync`: Data synchronization

#### Data Events
- `data:update`: Data update notification
- `access:granted`: Access grant notification
- `access:revoked`: Access revocation notification

## Configuration

### Environment Variables
```env
# Blockchain configuration
BLOCKCHAIN_TYPE=ethereum|sui|rest
ETH_RPC_URL=<ethereum_rpc_url>
ETH_PRIVATE_KEY=<private_key>
ETH_CONTRACT_ADDRESS=<contract_address>

# P2P configuration
P2P_TYPE=gun
P2P_PEERS=<comma_separated_peer_list>

# Storage configuration
DATA_STORE_TYPE=file
DATA_STORE_DIR=./data
```

### Plugin Configuration
```typescript
const config = {
    blockchain: {
        type: 'ethereum',
        rpcUrl: process.env.ETH_RPC_URL,
        // ... additional config
    },
    p2p: {
        type: 'gun',
        peers: process.env.P2P_PEERS?.split(','),
        // ... additional config
    },
    storage: {
        type: 'file',
        dataDir: process.env.DATA_STORE_DIR,
        // ... additional config
    }
};
```

## Error Handling

### Error Types
```typescript
enum ErrorType {
    BLOCKCHAIN_ERROR = 'BLOCKCHAIN_ERROR',
    P2P_ERROR = 'P2P_ERROR',
    STORAGE_ERROR = 'STORAGE_ERROR',
    DID_ERROR = 'DID_ERROR'
}
```

### Error Handling Example
```typescript
try {
    await blockchainPlugin.connect();
} catch (error) {
    if (error instanceof BlockchainError) {
        // Handle blockchain-specific error
    }
    // Handle general error
}
```