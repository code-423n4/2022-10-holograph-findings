Table Of Content
================

* [GAS REPORT](#gas-report)
        * [Using abiEncodePacked() is more efficient that abiEncode()](#using-abiencodepacked-is-more-efficient-that-abiencode)
        * [Use custom errors](#use-custom-errors)

# GAS REPORT

## Using abiEncodePacked() is more efficient that abiEncode()


### Code Instances:
- [HolographERC721.sol#L258](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L258)
- [StrictERC1155H.sol#L34](https://github.com/code-423n4/2022-10-holograph/tree/main/src/abstract/StrictERC1155H.sol#L34)
- [EIP712.sol#L87](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/abstract/EIP712.sol#L87)
- [StrictERC1155H.sol#L133](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/abstract/StrictERC1155H.sol#L133)

## Use custom errors
In the following require statements you can use custom errors to save gas and improve code quality.

### Code Instances:
- [Faucet.sol#L38](https://github.com/code-423n4/2022-10-holograph/tree/main/src/faucet/Faucet.sol#L38)
- [PA1D.sol#L74](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/PA1D.sol#L74)
- [HolographERC721.sol#L238](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L238)
- [HolographERC20.sol#L626](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L626)
