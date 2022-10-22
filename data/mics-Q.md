Table Of Content
================

* [QA REPORT](#qa-report)
        * [Array access is out of bounds](#array-access-is-out-of-bounds)
        * [Unused success return value](#unused-success-return-value)
        * [Missing two steps verification process](#missing-two-steps-verification-process)
        * [Missing 0 address check at transfer](#missing-0-address-check-at-transfer)
        * [Loss of precision: multiplications should be before divisions](#loss-of-precision-multiplications-should-be-before-divisions)
        * [Use safeTransfer() instead transfer()](#use-safetransfer-instead-transfer)
        * [SPDX license not provided in source file](#spdx-license-not-provided-in-source-file)
        * [Consider adding constant variables instead of hardcoded strings](#consider-adding-constant-variables-instead-of-hardcoded-strings)
        * [Add event to the following functions](#add-event-to-the-following-functions)
        * [Several functions are declaring named returns but then are using return statements. I suggest choosing only one for readability reasons.](#several-functions-are-declaring-named-returns-but-then-are-using-return-statements-i-suggest-choosing-only-one-for-readability-reasons)

# QA REPORT

## Array access is out of bounds
There is no check for the access to be in the array bounds.

### Code Instances:
- [HolographERC721.sol#L725](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/HolographERC721.sol#L725)
- [HolographERC721.sol#L824](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L824)
- [HolographRegistry.sol#L147](https://github.com/code-423n4/2022-10-holograph/tree/main/src/HolographRegistry.sol#L147)
- [HolographERC721.sol#L710](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L710)

## Unused success return value
The following calls ignores the return value of the called function that might indicate the the call failed.

### Code Instances:
- [HolographERC721.sol#L200](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/HolographERC721.sol#L200)
- [CxipERC721.sol#L88](https://github.com/code-423n4/2022-10-holograph/tree/main/src/token/CxipERC721.sol#L88)
- [HolographERC721.sol#L299](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L299)
- [HolographERC721.sol#L300](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L300)

## Missing two steps verification process
The process of transferring ownership is dangerous since typing the wrong address can lead to severe implications. It is better to have to steps verification process with set and claim functions to decrease the chances of human error. Consider changing to two steps verification process of transferring privileges. Human mistakes can happen.

### Code Instances:
- [HolographERC721.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/HolographERC721.sol)
- [HolographERC721.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol)

## Missing 0 address check at transfer
Some contracts does not support 0 transfer, then the transaction will revert with no explanation. We recommend to add a require statement that the amount is not 0.

### Code Instances:
- [HolographERC20.sol#L438](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/HolographERC20.sol#L438)
- [HolographERC20.sol#L587](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/HolographERC20.sol#L587)
- [HolographERC20.sol#L477](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/HolographERC20.sol#L477)
- [HolographERC20.sol#L576](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L576)

## Loss of precision: multiplications should be before divisions
Consider changing the order of the following instances math operators such that multiplications comes before divisions to improve calculation precision with no cost.

### Code Instances:
- [hToken.sol#L101](https://github.com/code-423n4/2022-10-holograph/tree/main/src/token/hToken.sol#L101)
- [hToken.sol#L200](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/token/hToken.sol#L200)
- [hToken.sol#L259](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/token/hToken.sol#L259)

## Use safeTransfer() instead transfer()
Use openzeppelin safeTransfer() method instead of transfer() in the following locations.

### Code Instances:
- [Faucet.sol#L161](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/faucet/Faucet.sol#L161)
- [Faucet.sol#L156](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/faucet/Faucet.sol#L156)
- [PA1D.sol#L296](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/PA1D.sol#L296)
- [Faucet.sol#L68](https://github.com/code-423n4/2022-10-holograph/tree/main/src/faucet/Faucet.sol#L68)

## SPDX license not provided in source file
Before publishing, consider adding a comment containing 'SPDX-License-Identifier: MIT' at the beginning of each source file.

### Code Instances:
- [CxipERC721.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/src/token/CxipERC721.sol)
- [HolographTreasury.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/src/HolographTreasury.sol)
- [StrictERC1155H.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/src/abstract/StrictERC1155H.sol)
- [StrictERC721H.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/src/abstract/StrictERC721H.sol)

## Consider adding constant variables instead of hardcoded strings
A good practice is to use constant variables instead of hardcoded strings in the code.

### Code Instances:
- [HolographERC721.sol#L815](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L815)
- [HolographERC721.sol#L717](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/HolographERC721.sol#L717)
- [Holograph.sol#L164](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/Holograph.sol#L164)
- [HolographERC20.sol#L683](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC20.sol#L683)

## Add event to the following functions


### Code Instances:
- [StrictERC1155H.sol#L226](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/abstract/StrictERC1155H.sol#L226)
- [StrictERC721H.sol#L107](https://github.com/code-423n4/2022-10-holograph/tree/main/src/abstract/StrictERC721H.sol#L107)
- [StrictERC20H.sol#L225](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/abstract/StrictERC20H.sol#L225)
- [StrictERC20H.sol#L52](https://github.com/code-423n4/2022-10-holograph/tree/main/src/abstract/StrictERC20H.sol#L52)

## Several functions are declaring named returns but then are using return statements. I suggest choosing only one for readability reasons.
Using both named returns and a return statement isn't necessary. Removing one of those can improve code clarity.

### Code Instances:
- [StrictERC1155H.sol#L226](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/abstract/StrictERC1155H.sol#L226)
- [StrictERC20H.sol#L52](https://github.com/code-423n4/2022-10-holograph/tree/main/src/abstract/StrictERC20H.sol#L52)
- [HolographFactory.sol#L181](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/HolographFactory.sol#L181)
- [StrictERC721H.sol#L258](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/abstract/StrictERC721H.sol#L258)
