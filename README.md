# Ex_No_4_DeFi-Lending-and-Borrowing-Protocol.md

# Experiment 4: DeFi Lending and Borrowing Protocol
# Aim:
To build a decentralized lending protocol where users can deposit assets to earn interest and borrow assets by providing collateral. This experiment introduces concepts like overcollateralization, liquidity pools, and interest accrual in DeFi.

# Algorithm:
Step 1: Setup Lending and Borrowing Mechanism
Users deposit ETH into the contract as liquidity.


Depositors receive interest based on their deposits.


Borrowers can borrow ETH but must provide collateral (e.g., 150% of the borrowed amount).


Interest on borrowed funds is calculated dynamically based on utilization rate.


Step 2: Implement Overcollateralization
If a borrower’s collateral value drops below a certain liquidation threshold, their collateral is liquidated to repay the debt.


Step 3: Allow Liquidation
If collateral < liquidation threshold, liquidators can repay the borrower's debt and claim their collateral at a discount.



Program:
```
//SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract DeFiLending {
    address public owner;
    uint256 public interestRate = 5; // 5% interest per cycle
    uint256 public liquidationThreshold = 150; // 150% collateralization
    mapping(address => uint256) public deposits;
    mapping(address => uint256) public borrowed;
    mapping(address => uint256) public collateral;

    event Deposited(address indexed user, uint256 amount);
    event Borrowed(address indexed user, uint256 amount, uint256 collateral);
    event Liquidated(address indexed user, uint256 debtRepaid, uint256 collateralSeized);

    constructor() {
        owner = msg.sender;
    }

    function deposit() public payable {
        require(msg.value > 0, "Deposit must be greater than zero");
        deposits[msg.sender] += msg.value;
        emit Deposited(msg.sender, msg.value);
    }

    function borrow(uint256 amount) public payable {
        require(msg.value >= (amount * liquidationThreshold) / 100, "Nota enough collateral");
        borrowed[msg.sender] += amount;
        collateral[msg.sender] += msg.value;
        payable(msg.sender).transfer(amount);
        emit Borrowed(msg.sender, amount, msg.value);
    }
    function reduceCollateral(address user, uint256 amount) public {
    require(msg.sender == owner, "Only owner can reduce");
    require(collateral[user] >= amount, "Not enough collateral to reduce");
    collateral[user] -= amount;
    }

    function liquidate(address borrower) public {
        require(collateral[borrower] < (borrowed[borrower] * liquidationThreshold) / 100, "Not eligible for liquidation");
        uint256 debt = borrowed[borrower];
        uint256 seizedCollateral = collateral[borrower];

        borrowed[borrower] = 0;
        collateral[borrower] = 0;
        payable(msg.sender).transfer(seizedCollateral);
        emit Liquidated(borrower, debt, seizedCollateral);
    }
}

```
# Expected Output:
Users can deposit ETH and earn interest.


Users can borrow ETH by providing collateral.


If collateral < 150% of borrowed amount, liquidators can seize the collateral.



# High-Level Overview:
Teaches key DeFi concepts: lending, borrowing, collateral, liquidation.


Introduces risk management: overcollateralization and liquidation.


Directly related to DeFi protocols like Aave and Compound.

# RESULT : 
![Screenshot 2025-04-29 144416](https://github.com/user-attachments/assets/ec5cdd60-3600-43a9-a650-dfff7dbb22ec)
![Screenshot 2025-04-29 144435](https://github.com/user-attachments/assets/7f4ec499-fded-4f73-9d2d-40a3b51eeadc)
![Screenshot 2025-04-29 144458](https://github.com/user-attachments/assets/eac0dff5-f008-4efd-b56b-7a96201b7e58)
![Screenshot 2025-04-29 144532](https://github.com/user-attachments/assets/f779b10a-883a-48e0-95f7-1af8f50fa90c)
![Screenshot 2025-04-29 144609](https://github.com/user-attachments/assets/c0616f51-b4cb-480f-856f-ec11ea1a9296)
![Screenshot 2025-04-29 144625](https://github.com/user-attachments/assets/c3415d88-b0e6-4986-8729-4f76dffd3256)
![Screenshot 2025-04-29 144639](https://github.com/user-attachments/assets/d7f152f2-d54e-4ffc-9dd3-e27bfc5923ae)
![Screenshot 2025-04-29 144729](https://github.com/user-attachments/assets/05377d6a-06e9-46a4-934f-24c3e0f57684)
![Screenshot 2025-04-29 144745](https://github.com/user-attachments/assets/a4f2c8d6-7e28-4b8c-8f70-c292fe23c972)
![Screenshot 2025-04-29 144804](https://github.com/user-attachments/assets/15666d1d-8d1b-492e-937d-02dadceaa4ac)
![Screenshot 2025-04-29 144822](https://github.com/user-attachments/assets/eb64da7f-67b0-4c98-a749-f2580832401c)
![Screenshot 2025-04-29 144843](https://github.com/user-attachments/assets/77fa8534-4f34-4e15-8c49-269f5d90447c)
![Screenshot 2025-04-29 144906](https://github.com/user-attachments/assets/8c1c0869-df2b-4373-8153-5bb002d2f4ed)
![Screenshot 2025-04-29 144941](https://github.com/user-attachments/assets/4fce9273-0992-4b01-9257-021ee9ccae1f)
![Screenshot 2025-04-29 144941](https://github.com/user-attachments/assets/a0164ac6-8e18-47b4-abec-fc1fbf88b168)
![Screenshot 2025-04-29 145009](https://github.com/user-attachments/assets/58708062-71ed-4f21-b233-a628f3072bff)












