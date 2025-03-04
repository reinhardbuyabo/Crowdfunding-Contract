# Crowdfunding Smart Contract

## Overview
This project implements a basic **Crowdfunding** smart contract using Solidity, deployed and compiled in **Remix IDE**. The contract allows users to create campaigns, donate to campaigns, and retrieve campaign details. The goal is to integrate this contract into various front-end platforms, including **iOS (UIKit & SwiftUI), Websites, and Android (Jetpack Compose)**.

## Features Implemented
- **Create Campaigns:** Users can create crowdfunding campaigns by providing details such as title, description, target amount, deadline, and an image URL.
- **Donate to Campaigns:** Users can contribute funds to campaigns using Ethereum.
- **Retrieve Campaign Data:** Users can get details of all campaigns and retrieve donor lists for a particular campaign.

## Smart Contract Structure
- **Data Structure**
  - `Campaign` struct storing campaign details.
  - `campaigns` mapping storing campaign instances.
  - `numberOfCampaigns` counter tracking total campaigns.
- **Functions**
  - `createCampaign()`: Allows users to create a new campaign.
  - `donateToCampaign()`: Enables users to donate ETH to campaigns.
  - `getDonators()`: Retrieves donors and their contributions for a campaign.
  - `getCampaigns()`: Fetches all campaign details.

## Integration Guide

### 1. **iOS (UIKit & SwiftUI) Integration**
To interact with the smart contract in an **iOS app**, use `web3.swift` or `Web3Modal` to connect with **Ethereum** via Metamask or WalletConnect.

#### Dependencies
- [web3.swift](https://github.com/argentlabs/web3.swift)
- [WalletConnect](https://github.com/WalletConnect/WalletConnectSwift)

#### Steps
1. **Install Dependencies**:
   ```sh
   pod init
   pod 'web3swift'
   pod install
   ```
2. **Connect to Ethereum**:
   ```swift
   import web3swift
   import BigInt
   
   let web3 = Web3.InfuraRopstenWeb3()
   let contractAddress = EthereumAddress("<DEPLOYED_CONTRACT_ADDRESS>")!
   let walletAddress = EthereumAddress("<WALLET_ADDRESS>")!
   ```
3. **Calling Smart Contract Functions**:
   ```swift
   let contract = web3.contract(<ABI>, at: contractAddress)!
   let parameters: [Any] = [walletAddress, "Campaign Title", "Description", 100000, Date().timeIntervalSince1970 + 86400, "image_url"]
   let transaction = contract.write("createCampaign", parameters: parameters, extraData: Data(), from: walletAddress, password: "<WALLET_PASSWORD>")
   ```

### 2. **Web (JavaScript & React Integration)**
Use **ethers.js** to connect to the Ethereum network.

#### Dependencies
```sh
npm install ethers
```

#### Web3 Connection
```javascript
import { ethers } from "ethers";
const provider = new ethers.providers.Web3Provider(window.ethereum);
const signer = provider.getSigner();
const contractAddress = "<DEPLOYED_CONTRACT_ADDRESS>";
const contractABI = [...] // Smart contract ABI
const contract = new ethers.Contract(contractAddress, contractABI, signer);
```

#### Calling Smart Contract
```javascript
const createCampaign = async () => {
  await contract.createCampaign(
    userAddress,
    "Campaign Title",
    "Campaign Description",
    100000,
    Math.floor(Date.now() / 1000) + 86400,
    "image_url"
  );
};
```

### 3. **Android (Jetpack Compose & Kotlin Integration)**
Use **web3j** to interact with the smart contract.

#### Dependencies
Add to `build.gradle`:
```gradle
dependencies {
    implementation 'org.web3j:core:4.8.7-android'
}
```

#### Connecting to Ethereum
```kotlin
val web3 = Web3j.build(HttpService("https://mainnet.infura.io/v3/<YOUR_INFURA_PROJECT_ID>"))
val contract = CrowdFunding.load("<DEPLOYED_CONTRACT_ADDRESS>", web3, credentials, BigInteger.ZERO, BigInteger.ZERO)
```

#### Calling Smart Contract
```kotlin
val transactionReceipt = contract.createCampaign(
    "0xYourWalletAddress",
    "Campaign Title",
    "Description",
    BigInteger.valueOf(100000),
    BigInteger.valueOf(System.currentTimeMillis() / 1000 + 86400),
    "image_url"
).send()
```

## Environment Variables
Create a `.env` file in your project root and include the following:
```
INFURA_API_KEY=<your_infura_api_key>
ALCHEMY_API_KEY=<your_alchemy_api_key>
DEPLOYED_CONTRACT_ADDRESS=<your_deployed_contract_address>
WALLET_PRIVATE_KEY=<your_wallet_private_key>
ABI=<contract_abi_json>
```

Rename `.env-example` to `.env` and fill in the required values.

## Contribution Guidelines
1. Fork the repository and create a new branch.
2. Implement new features or fix bugs.
3. Create a pull request for review.

## Future Features
- Campaign expiration and refund mechanism.
- Integration with ERC-20 tokens.
- More advanced authentication methods.

## License
This project is licensed under the **MIT License**.

