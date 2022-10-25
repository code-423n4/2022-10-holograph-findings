# [L-01] Bridge can mint Holographed ERC20 tokens
## Targets
- https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC20.sol#L415
## Impact
A bridge can mint an arbitrary amount of ERC20 tokens in a Holographed ERC20 contract.
## Proof of Concept
[HolographERC20.sol#L415](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC20.sol#L415):
```solidity
/**
  * @dev Allows the bridge to mint tokens (used for hTokens only).
  */
function holographBridgeMint(address to, uint256 amount) external onlyBridge returns (bytes4) {
  _mint(to, amount);
  return HolographERC20Interface.holographBridgeMint.selector;
}
```

While the function comment says the function will be used only in the hToken, leaving thus function in a contract that will be used by users of Holograph creates unnecessary risks.
## Tools Used
Manual review
## Recommended Mitigation Steps
Consider removing the `holographBridgeMint` function and leaving it only in the hToken contract.



# [L-02] Fallback operators can be duplicated
## Targets
- https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L526-L530
- https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L1185-L1193
## Impact
When picking random fallback operators, there's a chance of getting duplicates, which means the real number of fallback operators will be less than expected (less than 5). It's also possible that all five fallback operators will be the same operator. In some rare situations (e.g. when the picked operator hasn't executed the job) this may lead to a stalled job because none of the fallback operators (it can be only one operator) is willing to execute the job.
## Proof of Concept
When creating a new job, the list of fallback operators is filled and each operator is chosen pseudo-randomly
([HolographOperator.sol#L522-L533](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L522-L533)):
```solidity
_operatorJobs[jobHash] = uint256(
  ((pod + 1) << 248) |
    (uint256(_operatorTempStorageCounter) << 216) |
    (block.number << 176) |
    (_randomBlockHash(random, podSize, 1) << 160) |
    (_randomBlockHash(random, podSize, 2) << 144) |
    (_randomBlockHash(random, podSize, 3) << 128) |
    (_randomBlockHash(random, podSize, 4) << 112) |
    (_randomBlockHash(random, podSize, 5) << 96) |
    (block.timestamp << 16) |
    0
); // 80 next available bit position && so far 176 bits used with only 128 left
```
[HolographOperator.sol#L1185-L1193](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L1185-L1193):
```solidity
function _randomBlockHash(
  uint256 random,
  uint256 podSize,
  uint256 n
) private view returns (uint256) {
  unchecked {
    return (random + uint256(blockhash(block.number - n))) % podSize;
  }
}
```
Since there's no check for whether a fallback operator is already in the list, duplicates are possible.
## Tools Used
Manual review
## Recommended Mitigation Steps
Consider checking whether a fallback operator is already in the list before adding it.



# [L-03] Full minimal bond amount is slashed, instead of a percentage
## Targets
- https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L374-L378
## Impact
A failed operator gets slashed for a full minimal bond amount, instead of only a percentage, as explained in [Important flows](https://github.com/code-423n4/2022-10-holograph/blob/main/docs/IMPORTANT_FLOWS.md#leaving-pods).
## Proof of Concept
[HolographOperator.sol#L371-L385](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L371-L385):
```solidity
/**
  * @dev time to reward the current operator
  */
uint256 amount = _getBaseBondAmount(pod);
/**
  * @dev select operator that failed to do the job, is slashed the pod base fee
  */
_bondedAmounts[job.operator] -= amount;
/**
  * @dev the slashed amount is sent to current operator
  */
_bondedAmounts[msg.sender] += amount;
```
## Tools Used
Manual Review
## Recommended Mitigation Steps
Consider either updating the documentation to reflect the real behavior or updating the code to reflect the documentation.