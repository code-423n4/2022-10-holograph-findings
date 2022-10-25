# Holograph Ccontest QA Report

## Summary

The following QA issues were found during the code audit:

1. Unsafe ERC20 Operation(s) (8 instances)
2. `block.number` may exceed `type(uint32).max` (3 instances)

Total 11 instances of 2 issues.

## 1. Unsafe ERC20 Operation(s) (8 instances)

ERC20 operations can be unsafe due to different implementations and vulnerabilities in the standard. It is therefore recommended to always either use OpenZeppelin's SafeERC20 library or at least to wrap each operation in a require statement.

```solidity
contracts/HolographOperator.sol::400 => _utilityToken().transfer(job.operator, leftovers);

contracts/HolographOperator.sol::596 => payable(hToken).transfer(hlgFee);

contracts/HolographOperator.sol::839 => require(_utilityToken().transferFrom(msg.sender, address(this), amount), "HOLOGRAPH: token transfer failed");

contracts/HolographOperator.sol::889 => require(_utilityToken().transferFrom(msg.sender, address(this), amount), "HOLOGRAPH: token transfer failed");

contracts/HolographOperator.sol::932 => require(_utilityToken().transfer(recipient, amount), "HOLOGRAPH: token transfer failed");

contracts/enforcer/PA1D.sol::396 => addresses[i].transfer(sending);

contracts/enforcer/PA1D.sol::416 => require(erc20.transfer(addresses[i], sending), "PA1D: Couldn't transfer token");

contracts/enforcer/PA1D.sol::439 => require(erc20.transfer(addresses[i], sending), "PA1D: Couldn't transfer token");
```

## 2. `block.number` may exceed `type(uint32).max` (3 instances)

While this currently equates to around 1260 years, if there's a hard fork which makes block times much more frequent (e.g. to compete with Solana), then this limit may be reached much faster than expected, and transfers and delegations will remain stuck at their existing settings.

```solidity
contracts/HolographOperator.sol::499 => uint256 random = uint256(keccak256(abi.encodePacked(jobHash, _jobNonce(), block.number, block.timestamp)));

contracts/HolographOperator.sol::525 => (block.number << 176) |

contracts/HolographOperator.sol::1191 => return (random + uint256(blockhash(block.number - n))) % podSize;
```
