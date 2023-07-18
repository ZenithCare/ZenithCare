# Z3N Token Contract

The `Z3NToken` contract manages the Z3N tokens in the Zenith.Care platform. Z3N tokens are the native cryptocurrency of the platform and are used for various purposes including rewarding users, voting on proposals in the DAO, and profit sharing.

## Contract Overview

The `Z3NToken` contract is an ERC20-compliant token contract with additional functionality for minting new tokens. The contract is implemented in Solidity and is deployed on the Polygon blockchain network.

## Contract Code

Here is the full Solidity code of the `Z3NToken` contract:

```solidity
pragma solidity ^0.8.0;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
import "@openzeppelin/contracts/access/Ownable.sol";

contract Z3NToken is ERC20, Ownable {
    constructor() ERC20("Z3NToken", "Z3N") {}

    function mint(address to, uint256 amount) public onlyOwner {
        _mint(to, amount);
    }
}
```

**Functions**

The Z3NToken contract inherits all standard ERC20 functions, and includes an additional mint function. Here is a description of the mint function:

**mint(address to, uint256 amount)**: Mints amount new tokens and assigns them to to, increasing the total supply. This function can only be called by the contract owner.
Events

The Z3NToken contract emits all standard ERC20 events. These include Transfer and Approval.

**Modifiers**

The Z3NToken contract includes the onlyOwner modifier from the Ownable contract. This modifier restricts certain functions to be callable only by the contract owner.

**Interacting with the Contract**

**Here are some examples of how to interact with the Z3NToken contract**:

**Minting tokens**: The contract owner can mint new tokens by calling the mint function. Here's an example of how to do this using the ethers.js library:

```const tokenContract = new ethers.Contract(tokenAddress, tokenABI, signer);
await tokenContract.mint(userAddress, ethers.utils.parseEther("1000"));
```

**Transferring tokens: Any address can transfer their tokens to another address using the transfer function**:

```const tokenContract = new ethers.Contract(tokenAddress, tokenABI, signer);
await tokenContract.transfer(recipientAddress, ethers.utils.parseEther("100"));
```

**Querying balance**: Any address can query the balance of another address using the balanceOf function:

```const tokenContract = new ethers.Contract(tokenAddress, tokenABI, provider);
const balance = await tokenContract.balanceOf(userAddress);
console.log(`Balance: ${ethers.utils.formatEther(balance)} Z3N`);
```

The signer is an instance of ethers.Signer that can sign transactions, and provider is an instance of ethers.Provider that can read data from the blockchain.

This documentation provides a comprehensive overview of the `Z3NToken` contract. However, it's important to remember that interacting with smart contracts can result in loss of funds if not done correctly, so always be cautious and make sure you understand what you're doing.
