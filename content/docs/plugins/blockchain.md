---
title: Blockchains
date: 2024-02-17
weight: 1
---

### Overview
TravlrNode supports multiple blockchain platforms through a plugin-based architecture. Each blockchain implementation adheres to the `BlockchainPlugin` interface.

### Plugin Interface
```typescript
interface BlockchainPlugin {
    connect(): Promise<void>;
    disconnect(): Promise<void>;
    registerNode(did: string, isOrganization: boolean): Promise<string>;
    grantAccess(granterDID: string, granteeDID: string, dataKey: string, expirationTime: number): Promise<string>;
    revokeAccess(revokerDID: string, granteeDID: string, dataKey: string): Promise<string>;
    // ... additional methods
}
```

### Ethereum Implementation
The Ethereum plugin uses Web3.js for blockchain interactions and smart contracts for access control.

#### Key Features
- Smart contract-based access control
- Event monitoring
- Gas optimization
- Transaction management

#### Example Usage
```typescript
const ethereumPlugin = createBlockchainPlugin('ethereum', {
    rpcUrl: process.env.ETH_RPC_URL,
    privateKey: process.env.ETH_PRIVATE_KEY,
    contractAddress: process.env.ETH_CONTRACT_ADDRESS
});

await ethereumPlugin.connect();
```

### Sui Implementation
The Sui plugin leverages the Move programming language for efficient smart contract execution.

#### Key Features
- High throughput transactions
- Object-centric data model
- Native Move integration
- Parallel execution

#### Example Usage
```typescript
const suiPlugin = createBlockchainPlugin('sui', {
    rpcUrl: process.env.SUI_RPC_URL,
    privateKey: process.env.SUI_PRIVATE_KEY,
    moduleId: process.env.SUI_MODULE_ID
});
```
