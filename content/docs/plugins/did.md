---
title: Decentralised Identity
date: 2024-02-17
weight: 2
---

### Overview
TravlrNode uses Veramo for DID management and verifiable credentials.

### Core Components

#### DID Provider
```typescript
class PublicBlockchainDIDProvider extends AbstractIdentifierProvider {
    async createIdentifier(
        options: MinimalImportableKey,
        context: IAgentContext<IKeyManager>
    ): Promise<Omit<IIdentifier, 'provider'>> {
        // Implementation details
    }
}
```

#### Verifiable Credentials
```typescript
class VerifiableCredentialStore {
    async generateVC(
        issuerDID: string, 
        subjectDID: string, 
        claims: any
    ): Promise<any> {
        // Implementation details
    }
}
```

### Key Features
- Multiple DID method support
- Credential issuance and verification
- Key management
- Identity resolution