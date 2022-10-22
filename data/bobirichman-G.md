# GAS REPORT

## [GAS] Use > instead != to compare uint with 0


### Proof of concept:
- [HolographERC721.sol#L715](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/HolographERC721.sol#L715)
- [HolographERC721.sol#L814](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L814)

## [GAS] Mark as payable If has onlyOwner modifier
In order to save gas you can put a payable modifier for functions that are called by protocol owners.

### Proof of concept:
- [Holograph.sol#L189](https://github.com/code-423n4/2022-10-holograph/tree/main/src/Holograph.sol#L189)
- [SampleERC721.sol#L195](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/token/SampleERC721.sol#L195)

## [GAS] Use assembly opcodes iszero in the following locations


### Proof of concept:
- [HolographERC20.sol#L552](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/HolographERC20.sol#L552)
- [PA1D.sol#L434](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/PA1D.sol#L434)

## [GAS] Use abiEncodePacked()


### Proof of concept:
- [HolographERC20.sol#L469](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L469)
- [HolographFactory.sol#L249](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographFactory.sol#L249)

--