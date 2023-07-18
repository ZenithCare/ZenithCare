# UserRewards Contract

The `UserRewards` contract manages the user rewards system in the Zenith.Care platform. Users earn Z3n tokens by completing certain actions, and the tokens can be claimed and transferred to the user's address.

## Contract Overview

The `UserRewards` contract is implemented in Solidity and is deployed on the Polygon blockchain network. The contract keeps track of the amount of rewards each user has earned and allows users to claim their rewards.

## Contract Code

Here is a simplified version of the `UserRewards` contract code:

```solidity
pragma solidity ^0.8.0;

import "@openzeppelin/contracts/token/ERC20/IERC20.sol";
import "@openzeppelin/contracts/access/Ownable.sol";

contract UserRewards is Ownable {
    IERC20 public z3nToken;

    mapping (address => uint256) private _rewards;

    event RewardEarned(address indexed user, uint256 amount);
    event RewardClaimed(address indexed user, uint256 amount);

    constructor(IERC20 _z3nToken) {
        z3nToken = _z3nToken;
    }

    function earnReward(address user, uint256 amount) external onlyOwner {
        _rewards[user] += amount;
        emit RewardEarned(user, amount);
    }

    function claimReward() external {
        uint256 reward = _rewards[msg.sender];
        require(reward > 0, "No rewards to claim");
        _rewards[msg.sender] = 0;
        z3nToken.transfer(msg.sender, reward);
        emit RewardClaimed(msg.sender, reward);
    }

    function getReward(address user) external view returns (uint256) {
        return _rewards[user];
    }
}
```

Functions

**Here is a description of the functions in the UserRewards contract**:

**earnReward(address user, uint256 amount)**: Credits amount rewards to user. This function can only be called by the contract owner.

**claimReward()**: Transfers the caller's rewards to their address and resets their rewards balance to zero.

**getReward(address user)**: Returns the amount of rewards that user has earned but not yet claimed.
Events


**The UserRewards contract emits two events**:

**RewardEarned**: Emitted when a user earns rewards. The event includes the user's address and the amount of rewards earned.
**RewardClaimed**: Emitted when a user claims their rewards. The event includes the user's address and the amount of rewards claimed.


**Modifiers**

The UserRewards contract includes the onlyOwner modifier from the Ownable contract. This modifier restricts the earnReward function to be callable only by the contract owner.


**Interacting with the Contract**

**Here are some examples of how to interact with the UserRewards contract**:

**Earning rewards**: The contract owner can credit rewards to a user by calling the earnReward function. Here's an example of how to do this using the ethers.js library:

```
const rewardsContract = new ethers.Contract(rewardsAddress, rewardsABI, signer);
await rewardsContract.earnReward(userAddress, ethers.utils.parseEther("100"));
```

**Claiming rewards: A user can claim their rewards by calling the claimReward function**:

```
const rewardsContract = new ethers.Contract(rewardsAddress, rewardsABI, signer);
await rewardsContract.claimReward();
```

**Checking rewards balance: Any address can check the rewards balance of another address using the getReward function**:

```
const rewardsContract = new ethers.Contract(rewardsAddress, rewardsABI, provider);
const rewards = await rewardsContract.getReward(userAddress);
console.log(`Rewards: ${ethers.utils.formatEther(rewards)} Z3N`);
```

Remember to replace rewardsAddress with the actual address of the deployed UserRewards contract, and rewardsABI with the ABI of the contract. The signer is an instance of ethers.Signer that can sign transactions, and provider is an instance of ethers.Provider that can read data from the blockchain.

This documentation provides a comprehensive overview of the `UserRewards` contract. However, it's important to remember that interacting with smart contracts can result in loss of funds if not done correctly, so always be cautious and make sure you understand what you're doing.
