# DAO Contract

The `DAO` contract manages the Decentralized Autonomous Organization (DAO) in the Zenith.Care platform. The DAO enables users to propose and vote on various proposals related to the platform.

## Contract Overview

The `DAO` contract is implemented in Solidity and is deployed on the Polygon blockchain network. The contract allows users to stake Z3N tokens to gain voting power, create proposals, and vote on them.

## Contract Code

Here is a simplified version of the `DAO` contract code:

```solidity
pragma solidity ^0.8.0;

import "@openzeppelin/contracts/token/ERC20/IERC20.sol";
import "@openzeppelin/contracts/access/Ownable.sol";

contract DAO is Ownable {
    IERC20 public Z3NToken;

    struct Proposal {
        address proposer;
        string description;
        uint256 forVotes;
        uint256 againstVotes;
        uint256 endTime;
        bool executed;
    }

    Proposal[] public proposals;

    mapping (address => uint256) public stakes;
    mapping (uint256 => mapping (address => bool)) public votes;

    event ProposalCreated(uint256 indexed proposalId, address indexed proposer, string description);
    event Voted(uint256 indexed proposalId, address indexed voter, bool support, uint256 votes);
    event ProposalExecuted(uint256 indexed proposalId);
    event Staked(address indexed user, uint256 amount);
    event Unstaked(address indexed user, uint256 amount);

    constructor(IERC20 _Z3NToken) {
        Z3NToken = _Z3NToken;
    }

    // Contract functions go here
}
```

## Functions

**The DAO contract has several functions, including**:

**createProposal(string description)**: Creates a new proposal with the given description. The proposer is the address that calls this function.

**vote(uint256 proposalId, bool support)**: Votes on a proposal. The support parameter determines whether the vote is for or against the proposal.

**executeProposal(uint256 proposalId)**: Executes a proposal if it has more votes for than against and the voting period has ended.

**stake(uint256 amount)**: Stakes amount Z3N tokens to gain voting power in the DAO. The tokens are transferred from the caller's address to the contract.

**unstake(uint256 amount)**: Unstakes amount Z3N tokens, reducing the caller's voting power in the DAO. The tokens are transferred from the contract to the caller's address.
Events


**The DAO contract emits several events**:

**ProposalCreated**: Emitted when a new proposal is created. The event includes the proposal ID, the proposer's address, and the proposal description.

**Voted**: Emitted when a user votes on a proposal. The event includes the proposal ID, the voter's address, whether the vote was for or against the proposal, and the number of votes.

**ProposalExecuted**: Emitted when a proposal is executed. The event includes the proposal ID.

**Staked**: Emitted when a user stakes tokens. The event includes the user's address and the amount of tokens staked.

**Unstaked**: Emitted when a user unstakes tokens. The event includes the user's address and the amount of tokens unstaked.


## Modifiers

The DAO contract includes the onlyOwner modifier from the Ownable contract. This modifier restricts certain functions to be callable only by the contract owner.

**Interacting with the Contract**

**Here are some examples of how to interact with the DAO contract**:

**Creating a proposal**: A user can create a proposal by calling the createProposal function. Here's an example of how to do this using the ethers.js library:

```
const daoContract = new ethers.Contract(daoAddress, daoABI, signer);
await daoContract.createProposal("Proposal description");
```

**Voting on a proposal: A user can vote on a proposal by calling the vote function**:

```const daoContract = new ethers.Contract(daoAddress, daoABI, signer);
await daoContract.vote(proposalId, true);  // true for supporting the proposal, false for against
```

**Staking tokens: A user can stake tokens by calling the stake function**:

```const daoContract = new ethers.Contract(daoAddress, daoABI, signer);
await daoContract.stake(ethers.utils.parseEther("100"));
```

**Unstaking tokens: A user can unstake tokens by calling the unstake function**:

```const daoContract = new ethers.Contract(daoAddress, daoABI, signer);
await daoContract.unstake(ethers.utils.parseEther("100"));
```

The signer is an instance of ethers.Signer that can sign transactions.

This documentation provides a comprehensive overview of the DAO contract. However, it's important to remember that interacting with smart contracts can result in loss of funds if not done correctly, so always be cautious and make sure you understand what you're doing.
