# GAS REPORT

## [GAS] Cache the following arrays size before the loop


### Proof of concept:
- [HolographRegistry.sol#L174](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographRegistry.sol#L174)
- [HolographERC20.sol#L563](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L563)

## [GAS] Caching msg.sender is unnecessary


### Proof of concept:
- [HolographRegistry.sol#L169](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographRegistry.sol#L169)
- [HolographERC20.sol#L125](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/HolographERC20.sol#L125)

## [GAS] The following require statements could be split into multiple require statements to save gas


### Proof of concept:
- [HolographTreasuryProxy.sol#L26](https://github.com/code-423n4/2022-10-holograph/tree/main/src/proxy/HolographTreasuryProxy.sol#L26)
- [HolographERC721.sol#L463](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L463)

## [GAS] Consider using custom errors
You can utilize customized errors in require statements to save gas.

### Proof of concept:
- [HolographGenesis.sol#L129](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographGenesis.sol#L129)
- [HolographFactory.sol#L120](https://github.com/code-423n4/2022-10-holograph/tree/main/src/HolographFactory.sol#L120)

