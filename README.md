# STX-MemeticDAO Memecoin Token Contract

## Overview
The  **STX-MemeticDAO Memecoin Token Contract** is a fungible token contract with advanced features, including manual block height handling, transfer cooldowns, staking, and governance mechanisms. 
This contract is built using Clarity for the Stacks blockchain, providing transparency, security, and decentralized governance.

## Features
- **Fungible Token (MEME):** Implements a memecoin with a maximum supply cap.
- **Manual Block Height Management:** A unique feature that allows tracking and controlling block height manually.
- **Transfer Cooldown:** Prevents frequent transfers by enforcing a 10-block delay between successive transactions.
- **Staking Mechanism:** Users can stake MEME tokens for a specified duration, earning rewards.
- **Governance System:** Allows users to create and vote on governance proposals.

## Token Configuration
- **Name:** MemeToken
- **Symbol:** MEME
- **Max Supply:** 1,000,000,000 MEME
- **Total Supply:** Initially set to 0, minting is required.

## Error Codes
- `u100` - Only contract owner can perform this action.
- `u101` - Insufficient balance.
- `u102` - Transfer cooldown active.
- `u111` - No staking record found.
- `u112` - Staking period has not ended.
- `u113` - Proposal not found.
- `u114` - Voting deadline has passed.

---
## Functions

### **1. Block Height Management**
#### `update-block-height () → (ok uint)`
Manually increments the block height by 1.

#### `get-block-height () → uint`
Returns the current manually managed block height.

---
## **2. Token Transfers**
#### `transfer (amount uint, recipient principal) → (ok true | ERR-TRANSFER-COOLDOWN)`
Performs a token transfer with a **10-block cooldown** restriction.

##### **Conditions:**
- The sender must wait at least 10 blocks before making another transfer.
- The recipient receives the specified amount if conditions are met.

---
## **3. Staking Mechanism**
#### `stake-tokens (amount uint, lock-period uint) → (ok true | ERR-INSUFFICIENT-BALANCE)`
Allows users to stake MEME tokens for a specified lock duration, tracked in block height.

##### **Process:**
1. Tokens are deducted from the sender's balance and locked in the contract.
2. Unlock block is set as `current block + lock period`.

#### `unstake-tokens () → (ok true | ERR-STAKE-NOT-UNLOCKED)`
Allows users to unstake tokens if the lock period has passed.

##### **Process:**
1. Checks if the current block height is greater than or equal to the unlock block.
2. Transfers staked tokens back to the user.

---
## **4. Governance**
#### `create-governance-proposal (description string-utf8(200), voting-period uint) → (ok proposal-id)`
Creates a new governance proposal with a **voting deadline based on block height**.

##### **Process:**
1. Assigns a unique proposal ID.
2. Stores proposal details including proposer, description, and voting deadline.

#### `vote-on-proposal (proposal-id uint) → (ok true | ERR-VOTING-DEADLINE-PASSED)`
Allows users to vote on a proposal.

##### **Conditions:**
- Voting must occur before the proposal’s deadline.

---
## **Deployment & Usage**
1. **Deploy the contract** on the Stacks blockchain using Clarity.
2. **Initialize token supply** by minting MEME tokens (if applicable).
3. **Use functions** to transfer, stake, and participate in governance.

---
## **Security & Best Practices**
- Ensure only the contract owner can manage supply-related actions.
- Users must track block height updates regularly.
- Staked tokens are locked until the block height condition is met.
- Governance proposals have a strict voting deadline to maintain fairness.


