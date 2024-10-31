---
title: Getting started
date: 2024-02-17
weight: 3
slug: getting-started
---

## Prerequisites

### System Requirements
- Node.js (v14 or later)
- npm (v6 or later) or yarn
- Git
- 2GB RAM minimum
- 10GB available disk space

### Optional Requirements
- Docker & Docker Compose (for containerized deployment)
- Ethereum node access (for Ethereum blockchain integration)
- Sui node access (for Sui blockchain integration)

## Installation Steps

### 1. Clone the Repository
```bash
# Clone the repository
git clone https://github.com/travlr-network/travlrnode.git

# Navigate to project directory
cd travlrnode

# Install dependencies
npm install
```

### 2. Environment Setup

Create a `.env` file in the root directory:

```env
# Required Configuration
BLOCKCHAIN_TYPE=ethereum|sui|rest
API_PORT=3001
DATA_STORE_TYPE=file
DATA_STORE_DIR=./data
P2P_TYPE=gun

# Blockchain Configuration
## For Ethereum
ETH_RPC_URL=https://mainnet.infura.io/v3/YOUR_INFURA_PROJECT_ID
ETH_PRIVATE_KEY=your_ethereum_private_key
ETH_CONTRACT_ADDRESS=0x1234567890123456789012345678901234567890

## For Sui
SUI_RPC_URL=https://fullnode.devnet.sui.io:443
SUI_PRIVATE_KEY=your_sui_private_key
SUI_MODULE_ID=0x1234567890123456789012345678901234567890::data_sharing_contract

# Veramo Configuration
VERAMO_SECRET_KEY=your_secret_key_here
DID_PROVIDER=ethereum  # or 'rest'

# Optional Configuration
PUBLIC_BLOCKCHAIN_API_URL=http://localhost:3000
INFURA_PROJECT_ID=your_infura_project_id
ETHEREUM_NETWORK=mainnet  # or 'goerli' or 'sepolia'
```

### 3. Build the Project

```bash
# Build TypeScript files
npm run build

# Verify installation
npm test
```

## Configuration Options

### Blockchain Configuration

#### Ethereum Setup
```typescript
const ethereumConfig = {
  rpcUrl: process.env.ETH_RPC_URL,
  privateKey: process.env.ETH_PRIVATE_KEY,
  contractAddress: process.env.ETH_CONTRACT_ADDRESS,
  network: process.env.ETHEREUM_NETWORK || 'mainnet'
};
```

#### Sui Setup
```typescript
const suiConfig = {
  rpcUrl: process.env.SUI_RPC_URL,
  privateKey: process.env.SUI_PRIVATE_KEY,
  moduleId: process.env.SUI_MODULE_ID
};
```

#### REST API Fallback
```typescript
const restConfig = {
  baseUrl: process.env.PUBLIC_BLOCKCHAIN_API_URL
};
```

### Storage Configuration

#### File Storage
```typescript
const fileStorageConfig = {
  dataDir: process.env.DATA_STORE_DIR || './data',
  encryption: true,
  compression: true
};
```

### P2P Network Configuration

#### GUN Configuration
```typescript
const gunConfig = {
  peers: [
    'http://localhost:8765/gun',
    // Additional peer URLs
  ],
  localStorage: false,
  radisk: true
};
```

## Deployment Options

### 1. Local Development
```bash
# Start in development mode
npm start

# Start in test mode
npm run testmode
```

### 2. Docker Deployment

#### Using Docker Compose
```yaml
# docker-compose.yml
version: '3.8'
services:
  travlrnode:
    build: .
    ports:
      - "3001:3001"
    volumes:
      - ./data:/app/data
    environment:
      - BLOCKCHAIN_TYPE=ethereum
      - API_PORT=3001
      # Add other environment variables
```

```bash
# Start with Docker Compose
docker-compose up -d
```

### 3. Production Deployment

#### System Service Setup
```bash
# Create system service
sudo nano /etc/systemd/system/travlrnode.service
```

```ini
[Unit]
Description=TravlrNode Service
After=network.target

[Service]
Type=simple
User=travlrnode
WorkingDirectory=/opt/travlrnode
ExecStart=/usr/bin/npm start
Restart=always
Environment=NODE_ENV=production

[Install]
WantedBy=multi-user.target
```

## Verification Steps

### 1. Health Check
```bash
curl http://localhost:3001/health
```

Expected response:
```json
{
  "status": "healthy",
  "blockchain": "connected",
  "p2p": "active",
  "storage": "operational"
}
```

### 2. Component Testing

#### Test Blockchain Connection
```bash
curl http://localhost:3001/api/v1/blockchain/status
```

#### Test P2P Network
```bash
curl http://localhost:3001/api/v1/p2p/peers
```

#### Test Storage
```bash
curl http://localhost:3001/api/v1/storage/status
```

## Troubleshooting

### Common Issues

#### 1. Blockchain Connection Errors
```bash
# Check blockchain connectivity
npm run check-blockchain

# Verify environment variables
npm run verify-env
```

#### 2. P2P Network Issues
```bash
# Check network connectivity
npm run check-network

# Reset peer connections
npm run reset-peers
```

#### 3. Storage Issues
```bash
# Verify storage permissions
npm run check-storage

# Clear storage cache
npm run clear-cache
```

### Debug Mode

Enable debug logging by setting:
```env
DEBUG=travlrnode:*
LOG_LEVEL=debug
```

### Support Resources

- [GitHub Issues](https://github.com/yourusername/travlrnode/issues)
- [Documentation](https://docs.travlrnode.com)
- [Community Forum](https://forum.travlrnode.com)
- [Stack Overflow](https://stackoverflow.com/questions/tagged/travlrnode)

## Security Considerations

### 1. Key Management
- Store private keys securely
- Use environment variables for sensitive data
- Implement key rotation policies
- Enable audit logging

### 2. Network Security
- Configure firewalls appropriately
- Use SSL/TLS for API endpoints
- Implement rate limiting
- Monitor for suspicious activity

### 3. Access Control
- Review permissions regularly
- Implement principle of least privilege
- Enable multi-factor authentication
- Monitor access logs