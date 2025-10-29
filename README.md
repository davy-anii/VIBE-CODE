# VIBE-CODE
# 💫 VIBE CODE  

### A Simple On-Chain Balance & Transaction Tracker  

---




<img width="1470" height="956" alt="Screenshot 2025-10-29 at 2 23 50 PM" src="https://github.com/user-attachments/assets/d6698ba8-6acd-412f-8162-b444a7cb8503" />






## 🧠 Project Description  

**VIBE CODE** is a beginner-friendly Solidity project designed to keep your **account balance** and **transactions verifiable on-chain**.  
It represents the idea of transparency and trust in Web3 — letting users deposit, withdraw, and transfer Ether with every action recorded publicly on the blockchain.  

---

## ⚙️ What It Does  

- Allows users to **deposit**, **withdraw**, and **transfer** Ether securely.  
- Keeps a **public, verifiable transaction history** on the blockchain.  
- Stores user **balances** on-chain, accessible by anyone.  
- Demonstrates **core Solidity concepts** like mappings, structs, and events.  

---

## ✨ Features  

✅ **On-Chain Transparency** – All transactions are stored and visible publicly.  
✅ **Decentralized Account Balances** – No third party needed to verify funds.  
✅ **Event Logs** – Each transaction emits blockchain events for easy tracking.  
✅ **Beginner Friendly** – Clean, easy-to-understand Solidity code.  
✅ **Fully Verifiable Contract** – Anyone can interact and verify the logic.  

---

## 🧩 Smart Contract Code  

```solidity




// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

/*
 * VIBE CODE 💫
 * -----------------------------
 * A simple smart contract to keep your account balance 
 * and transactions verifiable on-chain.
 *
 * 🧠 Idea: "xyz" - Keep every user’s funds and movements 
 * transparent and trustless.
 */

contract VibeCode {

    // Structure to store each transaction
    struct Transaction {
        address from;
        address to;
        uint256 amount;
        uint256 timestamp;
    }

    // Mapping to store balance of each user
    mapping(address => uint256) public balances;

    // Array of all transactions on-chain
    Transaction[] public transactions;

    // Events (helpful for frontend or explorer tracking)
    event Deposit(address indexed user, uint256 amount, uint256 time);
    event Withdraw(address indexed user, uint256 amount, uint256 time);
    event Transfer(address indexed from, address indexed to, uint256 amount, uint256 time);

    // Deposit Ether to your account
    function deposit() public payable {
        require(msg.value > 0, "Deposit must be more than zero");

        balances[msg.sender] += msg.value;
        transactions.push(Transaction(address(0), msg.sender, msg.value, block.timestamp));

        emit Deposit(msg.sender, msg.value, block.timestamp);
    }

    // Withdraw Ether from your account
    function withdraw(uint256 _amount) public {
        require(balances[msg.sender] >= _amount, "Not enough balance");

        balances[msg.sender] -= _amount;
        payable(msg.sender).transfer(_amount);

        transactions.push(Transaction(msg.sender, address(0), _amount, block.timestamp));
        emit Withdraw(msg.sender, _amount, block.timestamp);
    }

    // Transfer Ether between users (within contract)
    function transfer(address _to, uint256 _amount) public {
        require(_to != address(0), "Invalid address");
        require(balances[msg.sender] >= _amount, "Not enough balance");

        balances[msg.sender] -= _amount;
        balances[_to] += _amount;

        transactions.push(Transaction(msg.sender, _to, _amount, block.timestamp));
        emit Transfer(msg.sender, _to, _amount, block.timestamp);
    }

    // View how many transactions recorded
    function getTransactionCount() public view returns (uint256) {
        return transactions.length;
    }

    // View details of a specific transaction
    function getTransaction(uint256 _index)
        public
        view
        returns (address from, address to, uint256 amount, uint256 timestamp)
    {
        require(_index < transactions.length, "Invalid index");
        Transaction memory txn = transactions[_index];
        return (txn.from, txn.to, txn.amount, txn.timestamp);
    }
}



