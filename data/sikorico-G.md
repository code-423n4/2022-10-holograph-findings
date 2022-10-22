# GAS REPORT

## [GAS 00] Use custom error instead string error for the following require statements


### Proof of concept:
- [Holographer.sol#L66](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/Holographer.sol#L66)
- [HolographERC721.sol#L319](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/HolographERC721.sol#L319)
- [PA1D.sol#L459](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L459)
- [Faucet.sol#L67](https://github.com/code-423n4/2022-10-holograph/tree/main/src/faucet/Faucet.sol#L67)
- [HolographOperatorProxy.sol#L117](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/proxy/HolographOperatorProxy.sol#L117)

## [GAS 01] Use ```> 0``` instead ```!= 0``` to check if an unsigned int is not 0


### Proof of concept:
- [HolographERC721.sol#L715](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/HolographERC721.sol#L715)
- [HolographERC721.sol#L814](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L814)

## [GAS 02] Cache the array size for the following loops over array


### Proof of concept:
- [PA1D.sol#L354](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/PA1D.sol#L354)
- [PA1D.sol#L337](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/PA1D.sol#L337)
- [PA1D.sol#L473](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L473)
- [PA1D.sol#L436](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L436)
- [HolographRegistry.sol#L321](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographRegistry.sol#L321)
