---

# RebaseToken Smart Contract

This repository contains the **RebaseToken** smart contract, an ERC20-compliant token with automated supply adjustments (rebasing). Built using **Solidity** and tested with **Foundry**, this contract dynamically changes the token supply based on predefined logic.

## 📌 Features

- **ERC20 Compliance** – Standard token functionality.
- **Rebasing Mechanism** – Adjusts total supply periodically.
- **Admin-Controlled Rebase** – Only the owner can trigger rebases.
- **Burn & Mint Mechanism** – Ensures supply elasticity.

---

## 🚀 Installation & Setup (Foundry)

To set up your Foundry environment and compile the contract, follow these steps:

### 1️⃣ Install Foundry

If you haven't installed Foundry yet, run:

```sh
curl -L https://foundry.paradigm.xyz | bash
foundryup
```

### 2️⃣ Clone the Repository

```sh
git clone https://github.com/yourusername/RebaseToken.git
cd RebaseToken
```

### 3️⃣ Install Dependencies

```sh
forge install
```

### 4️⃣ Compile the Contract

```sh
forge build
```

### 5️⃣ Run Tests

```sh
forge test
```

To see detailed logs:

```sh
forge test -vvvv
```

---

## 📜 Smart Contract Overview

### **RebaseToken.sol**

The **RebaseToken** contract is an ERC20 token that implements a rebasing mechanism. Key functionalities:

- **Constructor:** Initializes the token with a name, symbol, and initial supply.
- **Rebase Function:** Adjusts the total supply based on a multiplier.
- **Owner-Only Functions:** Secure access to rebasing.
- **Event Emissions:** Logs rebases for transparency.

---

## 🛠️ How Rebasing Works

The **rebase()** function modifies the token supply dynamically:

```solidity
function rebase(uint256 percentage) external onlyOwner {
    require(percentage > 0, "Percentage must be greater than zero");

    uint256 newSupply = (totalSupply() * (100 + percentage)) / 100;
    
    if (newSupply > totalSupply()) {
        uint256 mintAmount = newSupply - totalSupply();
        _mint(owner(), mintAmount);
    } else {
        uint256 burnAmount = totalSupply() - newSupply;
        _burn(owner(), burnAmount);
    }

    emit Rebase(block.timestamp, newSupply);
}
```

📌 **Key Mechanisms:**
- If `percentage` is **positive**, new tokens are minted.
- If `percentage` is **negative**, tokens are burned.
- `Rebase` events are emitted for tracking.

---

## ✅ Running a Local Node

To test on a local blockchain:

```sh
anvil
```

Then deploy using:

```sh
forge script script/Deploy.s.sol:Deploy --fork-url http://localhost:8545 --broadcast
```

---

## 🔗 Deployment (Optional)

If deploying to **Ethereum**, **Polygon**, or **Arbitrum**, configure Foundry with RPC:

```sh
export ETH_RPC_URL="https://mainnet.infura.io/v3/YOUR_INFURA_KEY"
```

Deploy with:

```sh
forge script script/Deploy.s.sol:Deploy --rpc-url $ETH_RPC_URL --broadcast --verify
```

---

## 📜 License

This project is licensed under the **MIT License**.

---
