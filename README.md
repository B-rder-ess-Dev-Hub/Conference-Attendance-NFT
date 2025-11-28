# UBL Conference Attendance NFT Project

A decentralized NFT minting system for conference attendees to mint unique attendance proof tokens. Built with Foundry and Solidity using ERC721 standard.

## 🎯 Learning Objectives

- ERC721 NFT contract development with Foundry
- One-time minting per address enforcement
- Token metadata and URI management
- Access control patterns
- NFT transfer handling with minting restrictions

## 📋 Prerequisites

- Basic Solidity knowledge
- Foundry installed locally
- Understanding of ERC721 standards
- Familiarity with NFT concepts

## 🏗️ Project Structure
```bash
conference-nft/
├── src/
│   ├── ConferenceNFT.sol
│   └── interfaces/
├── script/
│   ├── Deploy.s.sol
│   └── SetupMetadata.s.sol
├── test/
│   ├── ConferenceNFT.t.sol
│   └── Integration.t.sol
├── metadata/
│   ├── base-uri.json
│   └── token-uris/
└── README.md
```

## 📝 Core Tasks

## Task 1: Conference NFT Contract

**File:** `src/ConferenceNFT.sol`

**Requirements:**
1. Create an ERC721 token with name "Conference Attendance" and symbol "CONF"
2. Implement one-time minting per address (even after transfer)
3. Add minting functionality with access control
4. Include token URI management for metadata
5. Track all attendees and minting history

**Key Data Structures:**
```solidity
mapping(address => bool) public hasMinted;
mapping(uint256 => string) private _tokenURIs;
uint256 private _tokenIdCounter;
address[] public allAttendees;
string public baseURI;
```

## Task 2: Minting Logic & Access Control

**File:** `src/ConferenceNFT.sol`

**Requirements:**
1. Implement `mintAttendanceNFT()` function that:
   - Checks if address hasn't minted before
   - Mints a unique token to the attendee
   - Records the minting event
   - Sets appropriate token URI
2. Add owner/admin functions to:
   - Update base URI
   - Withdraw funds (if paid minting)
   - Manage contract settings

**Core Functions:**
```solidity
function mintAttendanceNFT() external returns (uint256)
function hasAddressMinted(address _address) external view returns (bool)
function setBaseURI(string memory _newBaseURI) external onlyOwner
function getTotalAttendees() external view returns (uint256)
```

## Task 3: Metadata Management

**File:** `src/ConferenceNFT.sol`

**Requirements:**
1. Implement proper ERC721 metadata extension
2. Create token URI structure that includes:
   - Conference name and details
   - Attendee information (optional)
   - Minting timestamp
   - Unique traits/properties
3. Support both individual token URIs and base URI pattern

## Task 4: Deployment Script

**File:** `script/Deploy.s.sol`

**Requirements:**
- Deploy ConferenceNFT contract
- Set up initial metadata base URI
- Configure owner/admin addresses
- Set up any initial parameters

## Task 5: Testing

**Files:** `test/ConferenceNFT.t.sol` and `test/Integration.t.sol`

**Test Coverage Requirements:**
- One-time minting enforcement
- NFT transfer functionality
- Minting after transfer prevention
- Metadata URI correctness
- Access control checks
- Edge cases and security tests

# 🚀 Step-by-Step Implementation Guide

## Phase 1: Project Setup
```bash
# Initialize Foundry project
forge init conference-nft
cd conference-nft

# Install OpenZeppelin contracts for ERC721
forge install OpenZeppelin/openzeppelin-contracts --no-commit

# Initialize git and add remote
git init
git remote add origin git@github.com:B-rder-ess-Dev-Hub/Conference-Attendance-NFT
```

## Phase 2: NFT Contract Implementation

1. Create `src/ConferenceNFT.sol` importing OpenZeppelin ERC721
2. Implement one-time minting logic using mapping
3. Add metadata management functions
4. Write comprehensive tests

## Phase 3: Minting Logic

1. Implement `mintAttendanceNFT()` with access checks
2. Add token ID counter and tracking
3. Create view functions for attendee management
4. Test minting restrictions thoroughly

## Phase 4: Metadata & Deployment

1. Set up token URI structure
2. Create deployment scripts
3. Prepare metadata JSON files
4. Test end-to-end minting flow

## Phase 5: Advanced Features

1. Add event logging for minting
2. Implement batch operations (if needed)
3. Add upgradeability considerations
4. Final security audit

## 🧪 Testing Commands
```bash
# Run all tests
forge test

# Run specific test suite
forge test --match-test testMinting

# Test with gas reporting
forge test --gas-report

# Run tests with verbose output
forge test -vv

# Test specific function
forge test --match-test testOneTimeMinting
```

## 📤 Deployment Commands
```bash
# Deploy to local network
forge script script/Deploy.s.sol --fork-url http://localhost:8545 --broadcast

# Deploy to testnet
forge script script/Deploy.s.sol --rpc-url $TESTNET_RPC --private-key $PRIVATE_KEY --broadcast

# Verify contract on Etherscan
forge verify-contract <CONTRACT_ADDRESS> src/ConferenceNFT.sol:ConferenceNFT --etherscan-api-key $ETHERSCAN_KEY
```

## 🔒 Security Considerations

- Prevent reentrancy attacks in minting
- Validate all user inputs and URIs
- Implement proper access controls
- Ensure one-time minting cannot be bypassed
- Test transfer scenarios thoroughly
- Use Checks-Effects-Interactions pattern

## 💡 Key Algorithms

### One-Time Minting Enforcement
```solidity
function mintAttendanceNFT() external {
    require(!hasMinted[msg.sender], "Already minted");
    require(balanceOf(msg.sender) == 0, "Already holds NFT");
    
    hasMinted[msg.sender] = true;
    uint256 tokenId = _tokenIdCounter.current();
    _safeMint(msg.sender, tokenId);
    _tokenIdCounter.increment();
}
```

### Metadata Management
```solidity
function tokenURI(uint256 tokenId) public view override returns (string memory) {
    require(_exists(tokenId), "Token does not exist");
    
    if (bytes(_tokenURIs[tokenId]).length > 0) {
        return _tokenURIs[tokenId];
    }
    
    return string(abi.encodePacked(baseURI, Strings.toString(tokenId)));
}
```

## 🎨 Metadata Structure Example
```json
{
  "name": "UBL Conference Attendance Proof #1",
  "description": "Proof of attendance at Blockchain Conference 2024",
  "image": "https://example.com/conference-nft/1.png",
  "attributes": [
    {
      "trait_type": "Conference", 
      "value": "Unizik Blockchain Conference 4.0"
    },
    {
      "trait_type": "Date",
      "value": "2024-03-15"
    },
    {
      "trait_type": "Type",
      "value": "Attendance Proof"
    }
  ]
}
```

## 📚 Resources

- [Foundry Documentation](https://getfoundry.sh/)
- [OpenZeppelin ERC721](https://docs.openzeppelin.com/contracts/4.x/erc721)
- [ERC721 Standard](https://eips.ethereum.org/EIPS/eip-721)
- [NFT Metadata Standards](https://docs.opensea.io/docs/metadata-standards)

## 🎯 Success Criteria

- ✅ Each address can mint only once
- ✅ Minting blocked even after NFT transfer
- ✅ Proper metadata management
- ✅ Comprehensive test coverage
- ✅ Secure access controls
- ✅ Gas-efficient implementation

Happy Building!
