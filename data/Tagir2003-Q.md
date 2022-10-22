# QA REPORT

## [LOW] For the following functions, add timelock


### Proof of concept:
- [Holograph.sol#L128](https://github.com/code-423n4/2022-10-holograph/tree/main/src/Holograph.sol#L128)
- [HolographRegistry.sol#L237](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographRegistry.sol#L237)

## [NON CRITICAL] Use of magic numbers in the following locations


### Proof of concept:
- [PA1D.sol#L552](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L552)
- [PA1D.sol#L394](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L394)

## [NON CRITICAL] The following functions may not be payable


### Proof of concept:
- [HolographOperatorProxy.sol#L46](https://github.com/code-423n4/2022-10-holograph/tree/main/src/proxy/HolographOperatorProxy.sol#L46)
- [HolographRegistryProxy.sol#L145](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/proxy/HolographRegistryProxy.sol#L145)
