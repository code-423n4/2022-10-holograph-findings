# QA REPORT

## [QA 00] Consider adding to the following contracts an MIT license


### Proof of concept:
- [HolographRegistry.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/src/HolographRegistry.sol)
- [HolographERC721.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/HolographERC721.sol)
- [Holograph.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/src/Holograph.sol)
- [SampleERC20.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/src/token/SampleERC20.sol)
- [HolographUtilityToken.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/src/token/HolographUtilityToken.sol)

## [QA 01] Add Timelock for the following functions:


### Proof of concept:
- [HolographOperatorProxy.sol#L137](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/proxy/HolographOperatorProxy.sol#L137)
- [Faucet.sol#L73](https://github.com/code-423n4/2022-10-holograph/tree/main/src/faucet/Faucet.sol#L73)
- [Faucet.sol#L145](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/faucet/Faucet.sol#L145)
- [HolographERC721.sol#L384](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/HolographERC721.sol#L384)
- [HolographTreasury.sol#L146](https://github.com/code-423n4/2022-10-holograph/tree/main/src/HolographTreasury.sol#L146)
