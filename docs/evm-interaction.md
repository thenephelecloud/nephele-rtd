# Interacting with the EVM Chain

## Overview

This documentation guides you through the Ethereum Virtual Machine (EVM) interactions within our forthcoming decentralized file sharing protocol, with a specific focus on the FileStorage smart contract's fallback function.

---

## Preparing for Smart Contract Interaction

### Smart Contract Information

- **Address**: TBD (To be provided post-network launch)
- **ABI**: Will be included in your contract deployment package.

### Functionality: Fallback for `storeFileMetadata`

The smart contract's fallback function will be employed to handle the storage of content identifiers within the blockchain, simplifying metadata management by automatically associating the content with the transaction initiator (msg.sender).

### Parameters (Planned)

- `contentId` (bytes32 or array): This is either a single unique identifier or an array of identifiers for the content, facilitating support for multipart formats and streaming protocols.

### Usage and Examples

Below is a hypothetical example using the Node.js Web3 library, tailored for after the final network deployment. This example demonstrates how to interact with the FileStorage smart contract's fallback function, solely utilizing `contentId`:

```javascript
const Web3 = require('web3');
// Placeholder values for contract ABI and address
const contractABI = []; 
const contractAddress = "TBD"; // Update post-launch

const web3 = new Web3('http://localhost:8545');
const fileStorageContract = new web3.eth.Contract(contractABI, contractAddress);

async function storeContent(contentIds) {
  const accounts = await web3.eth.getAccounts();
  // Prepare the data payload for the fallback function using contentIds
  const dataPayload = web3.utils.solidityPack(['bytes32[]'], [contentIds.map(id => web3.utils.asciiToHex(id))]);

  // Send the transaction to the smart contract
  const tx = {
    from: accounts[0],
    to: contractAddress,
    data: dataPayload,
    // Set gas and value as required for your contract's needs
  };

  const receipt = await web3.eth.sendTransaction(tx);
  console.log(receipt);
}

// Hypothetical usage example with multiple content IDs
storeContent(['content_id_1', 'content_id_2']);
```
---