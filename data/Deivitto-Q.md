
# QA
# Low
## Prevent div by 0
### Impact
On a couple of places in the code precautions are not being taken not to divide by `0`, this would revert the code.

### Proof of Concept
Navigate to the following contracts,

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/HolographOperator.sol#L1177
      current += (current / _operatorThresholdDivisor) * (position / _operatorThresholdStep);

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/HolographOperator.sol#L346
        uint256 timeDifference = elapsedTime / job.blockTimes;


### Recommended Mitigation Steps
Recommend making sure division by `0` won’t occur by checking the variables beforehand and handling this edge case.




## Useless/unused fallback / receive function
### Summary
There are `fallback` and `receive` functions that does nothing. This can lead into unexpected behavior / loss funds if somebody sends ether and no withdraw / transfer flow created.

### Github Permalinks
https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC20.sol#L251-L257
  receive() external payable {}

### Mitigation
If the intention is for the Ether to be used, the function should call another function, otherwise it
should `revert` (e.g. require(msg.sender == address(weth)) )


## block.timestamp used as time proxy 
### Summary
Risk of using `block.timestamp` for time should be considered. 
### Details
`block.timestamp` is not an ideal proxy for time because of issues with synchronization, miner manipulation and changing block times. 

This kind of issue may affect the code allowing or reverting the code before the expected deadline, modifying the normal functioning or reverting sometimes.
### References
SWC ID: 116

### Github Permalinks 
https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/HolographOperator.sol#L899-L933
https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/HolographOperator.sol#L301-L439
https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/HolographOperator.sol#L849-L891
https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/HolographOperator.sol#L825-L840
https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/HolographOperator.sol#L1122-L1155
https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC20.sol#L460-L490

### Mitigation
- Consider the risk of using `block.timestamp` as time proxy and evaluate if block numbers can be used as an approximation for the application logic. Both have risks that need to be factored in. 
- Consider using an oracle for precision


## Front run initializer
### Summary
The initialize function that initializes important contract state can be called by anyone.
### Details
The attacker can initialize the contract before the legitimate deployer, hoping that the victim continues to use the same contract.

In the best case for the victim, they notice it and have to redeploy their contract costing gas.

### Github Permalinks
https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/HolographBridge.sol#L162
https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/HolographFactory.sol#L143
https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/HolographOperator.sol#L240
https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/abstract/ERC20H.sol#L140
https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/abstract/ERC721H.sol#L140
https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/Holographer.sol#L147
https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC20.sol#L218
https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC721.sol#L238
https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/PA1D.sol#L173
https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/module/LayerZeroModule.sol#L158

### Mitigation
Use the constructor to initialize non-proxied contracts.

For initializing proxy contracts deploy contracts using a factory contract that immediately calls initialize after deployment or make sure to call it immediately after deployment and verify the transaction succeeded.


## Incorrect user interfaces
### Github Permalinks
https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC20.sol#L138-L757
https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC20.sol#L138-L757
https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC20.sol#L138-L757
https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC20.sol#L138-L757
https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC20.sol#L138-L757
https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC20.sol#L138-L757
https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC20.sol#L138-L757
https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC20.sol#L138-L757


# Informational

## Missing indexed event parameters
### Summary
Events without indexed event parameters make it harder and
inefficient for off-chain tools to analyze them.
### Details
Indexed parameters (“topics”) are searchable event parameters.
They are stored separately from unindexed event parameters in an efficient manner to allow for faster access. This is useful for efficient off-chain-analysis, but it is also more costly gas-wise.
 
### Github Permalinks
https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/PA1D.sol#L153

### Mitigation
Consider which event parameters could be particularly useful to off-chain tools and should be indexed.




## Naming convention of constants
### Summary
Constant naming convention is all upper case. 
### Details
Some constants are not using proper style.
Constant should be in `UPPER_CASE_WITH_UNDERSCORES` as per Solidity Style Guide. 
### Github Permalinks
https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/Holographer.sol#L119-L135
https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/HolographBridge.sol#L126-L142
https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/HolographFactory.sol#L127-L131
https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/HolographOperator.sol#L129-L153
https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/abstract/ERC20H.sol#L110-L114
https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/abstract/ERC721H.sol#L110-L114
https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC20.sol#L142-L146
https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC721.sol#L136-L140
https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/PA1D.sol#L124-L144
https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/module/LayerZeroModule.sol#L126-L146
### Mitigation
Rename the constant to uppercase style: `CONSTANTS_WITH_UNDERSCORES`. 


## Missing Natspec 
### Summary 
Missing Natspec and regular comments affect readability and maintainability of a codebase. 
### Details 
Contracts has partial or full lack of comments
### Github Permalinks 
#### Totally Natspec @return value missing, partial @param missing
https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/module/LayerZeroModule.sol#L1-L485
https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/Holographer.sol#L1-L244
https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC20.sol#L1-L757
https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/abstract/ERC721H.sol#L1-L230
https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/HolographFactory.sol#L1-L351
https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/abstract/ERC20H.sol#L1-L229

 ### mitigation
 - Add @param descriptors
 - Add @return descriptors
 - Add Natspec comments. 
 - Add inline comments. 
 - Add comments for what the contract does




## Commented out code
### Summary
There is some code that is commented out. It could point to items that are not done or need redesigning, be a mistake, or just be testing overhead.

### Github Permalinks
https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC721.sol#L524-L570
https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/HolographOperator.sol#L438


### Mitigation
Review and remove or resolve/document the commented out lines if needed.


## Missing initializer modifier on constructor
### Impact
OpenZeppelin [recommends](https://forum.openzeppelin.com/t/uupsupgradeable-vulnerability-post-mortem/15680/5) that the initializer modifier be applied to constructors in order to avoid potential griefs, [social engineering](https://forum.openzeppelin.com/t/uupsupgradeable-vulnerability-post-mortem/15680/4), or exploits. 

### Github Permalink
https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/abstract/ERC20H.sol#L133
https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/HolographBridge.sol#L155
https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/HolographFactory.sol#L136
https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/HolographOperator.sol#L233
https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/abstract/ERC721H.sol#L133
https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/Holographer.sol#L140
https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC20.sol#L211
https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC721.sol#L231
https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/PA1D.sol#L166
https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/module/LayerZeroModule.sol#L151
### Mitigation
Ensure that the modifier is applied to the implementation contract. If the default constructor is currently being used, it should be changed to be an explicit one with the modifier applied.





## Open TODOs   
### Summary
Code architecture, incentives, and error handling/reporting questions/issues should be resolved before deployment
### Details
The code includes a `TODO` that affects readability and focus on the readers/auditors of the contracts
### Github Permalinks


https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/HolographOperator.sol#L701
        // TODO: move the bit-shifting around to have it be sequential


### Mitigation
Remove already done `TODO`



## Missing error messages in require statements
### Summary
Require/revert statements should include error messages in order to help at monitoring the system.
### Github Permalinks
https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC721.sol#L373
      require(SourceERC721().beforeApprove(tokenOwner, to, tokenId));

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC721.sol#L378
      require(SourceERC721().afterApprove(tokenOwner, to, tokenId));


https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC721.sol#L391
      require(SourceERC721().beforeBurn(wallet, tokenId));

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC721.sol#L395
      require(SourceERC721().afterBurn(wallet, tokenId));

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC721.sol#L460
      require(SourceERC721().beforeSafeTransfer(from, to, tokenId, data));


https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC721.sol#L473
      require(SourceERC721().afterSafeTransfer(from, to, tokenId, data));

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC721.sol#L486
      require(SourceERC721().beforeApprovalAll(to, approved));

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC721.sol#L491
      require(SourceERC721().afterApprovalAll(to, approved));

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC721.sol#L624
      require(SourceERC721().beforeTransfer(from, to, tokenId, data));

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC721.sol#L628
      require(SourceERC721().afterTransfer(from, to, tokenId, data));
https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC721.sol#L759
      require(SourceERC721().beforeOnERC721Received(_operator, _from, address(this), _tokenId, _data));

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC721.sol#L767
      require(SourceERC721().afterOnERC721Received(_operator, _from, address(this), _tokenId, _data));

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC20.sol#L328
      require(SourceERC20().beforeApprove(msg.sender, spender, amount));

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC20.sol#L332
      require(SourceERC20().afterApprove(msg.sender, spender, amount));

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC20.sol#L339
      require(SourceERC20().beforeBurn(msg.sender, amount));

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC20.sol#L343
      require(SourceERC20().afterBurn(msg.sender, amount));


https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC20.sol#L541
      require(SourceERC20().afterSafeTransfer(account, recipient, amount, data));

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC20.sol#L582
      require(SourceERC20().beforeTransfer(msg.sender, recipient, amount));

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC20.sol#L586
      require(SourceERC20().afterTransfer(msg.sender, recipient, amount));

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC20.sol#L606
      require(SourceERC20().beforeTransfer(account, recipient, amount));

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC20.sol#L610
      require(SourceERC20().afterTransfer(account, recipient, amount));


https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/HolographBridge.sol#L366
      revert(revertReason);

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/HolographBridge.sol#L578
    revert();

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/HolographBridge.sol#L585
    revert();

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/HolographOperator.sol#L1215
    revert();

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/HolographFactory.sol#L341
    revert();

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/HolographFactory.sol#L348
    revert();

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/module/LayerZeroModule.sol#L417
    revert();

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/module/LayerZeroModule.sol#L424
    revert();

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC20.sol#L430
      require(SourceERC20().beforeApprove(msg.sender, spender, newAllowance));

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC20.sol#L434
      require(SourceERC20().afterApprove(msg.sender, spender, newAllowance));
https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC20.sol#L354
      require(SourceERC20().beforeBurn(account, amount));

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC20.sol#L358
      require(SourceERC20().afterBurn(account, amount));
https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC20.sol#L371
      require(SourceERC20().beforeApprove(msg.sender, spender, newAllowance));

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC20.sol#L375
      require(SourceERC20().afterApprove(msg.sender, spender, newAllowance));

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC20.sol#L447
      require(SourceERC20().beforeOnERC20Received(account, sender, address(this), amount, data));


https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC20.sol#L455
      require(SourceERC20().afterOnERC20Received(account, sender, address(this), amount, data));
https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC20.sol#L484
      require(SourceERC20().beforeApprove(account, spender, amount));

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC20.sol#L488
      require(SourceERC20().afterApprove(account, spender, amount));

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC20.sol#L502
      require(SourceERC20().beforeSafeTransfer(msg.sender, recipient, amount, data));
https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC20.sol#L507
      require(SourceERC20().afterSafeTransfer(msg.sender, recipient, amount, data));

https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC20.sol#L536
      require(SourceERC20().beforeSafeTransfer(account, recipient, amount, data));
https://github.com/code-423n4/2022-10-holograph/blob/24bc4d8dfeb6e4328d2c6291d20553b1d3eff00b/contracts/enforcer/HolographERC20.sol#L529
### Mitigation
Add error messages 


