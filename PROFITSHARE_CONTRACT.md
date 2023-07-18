# ProfitShare Contract

The `ProfitShare` contract handles the distribution of profits generated from data monetization in the Zenith.Care platform. Users who contribute their data to the platform share in the profits.

## Contract Overview

The `ProfitShare` contract is implemented in Solidity and is deployed on the Polygon blockchain network. The contract allows the Zenith.Care platform to distribute profits to users based on their data contributions.

## Contract Code

Here is a simplified version of the `ProfitShare` contract code:

```solidity
pragma solidity ^0.8.0;

import "@openzeppelin/contracts/token/ERC20/IERC20.sol";
import "@openzeppelin/contracts/access/Ownable.sol";

contract ProfitShare is Ownable {
    IERC20 public z3nToken;

    mapping (address => uint256) public pendingProfits;
    mapping (address => uint256) public claimedProfits;

    event ProfitDistributed(address indexed user, uint256 amount);
    event ProfitWithdrawn(address indexed user, uint256 amount);

    constructor(IERC20 _z3nToken) {
        z3nToken = _z3nToken;
    }
}
```

## Functions

The ProfitShare contract has several functions, including:

**distributeProfit(address user, uint256 amount)**: Distributes amount profits to user. The profits are stored in the pendingProfits mapping and can be withdrawn by the user at any time.

**withdrawProfits()**: Withdraws the caller's pending profits. The profits are transferred from the contract to the caller's address, and the caller's pending profits are reset to zero.
Events

**The ProfitShare contract emits several events**:

**ProfitDistributed**: Emitted when profits are distributed to a user. The event includes the user's address and the amount of profits.

**ProfitWithdrawn**: Emitted when a user withdraws their profits. The event includes the user's address and the amount of profits.
Modifiers

The ProfitShare contract includes the onlyOwner modifier from the Ownable contract. This modifier restricts certain functions to be callable only by the contract owner.

## Interacting with the Contract

Here are some examples of how to interact with the ProfitShare contract:

Distributing profits: The Zenith.Care platform can distribute profits to a user by calling the distributeProfit function. Here's an example of how to do this using the ethers.js library:

```
const profitShareContract = new ethers.Contract(profitShareAddress, profitShareABI, signer);
await profitShareContract.distributeProfit(userAddress, ethers.utils.parseEther("100"));
```

Withdrawing profits: A user can withdraw their profits by calling the withdrawProfits function:

```
const profitShareContract = new ethers.Contract(profitShareAddress, profitShareABI, signer);
await profitShareContract.withdrawProfits();
```

Remember to replace profitShareAddress with the actual address of the deployed ProfitShare contract, and profitShareABI with the ABI of the contract. The signer is an instance of ethers.Signer that can sign transactions.

This documentation provides a comprehensive overview of the ProfitShare contract. However, it's important to remember that interacting with smart contracts can result in loss of funds if not done correctly, so always be cautious and make sure you understand what you're doing.
