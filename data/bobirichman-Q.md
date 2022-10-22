# QA REPORT

## [LOW] Consider adding two steps verification process
Protocol ownership transfer should be dealt with great care. Adding two steps verification is necessary for that matter.

### Proof of concept:
- [HolographERC721.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/HolographERC721.sol)
- [HolographERC721.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol)

## [LOW] Use mult before div
To improve the following calculations precision consider changing the order of the operations such that multiplications come before divisions.

### Proof of concept:
- [hToken.sol#L200](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/token/hToken.sol#L200)
- [hToken.sol#L259](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/token/hToken.sol#L259)

## [LOW] Missing nonReentrancy modifier
The following functions allows attackers to try reentrancy since they are calling to external contracts / transferring eth. Consider adding a nonReentrancy modifier.

### Proof of concept:
- [Faucet.sol#L62](https://github.com/code-423n4/2022-10-holograph/tree/main/src/faucet/Faucet.sol#L62)
- [HolographERC721.sol#L577](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/HolographERC721.sol#L577)

## [LOW] Missing pause functionality


### Proof of concept:
- [StrictERC20H.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/abstract/StrictERC20H.sol)
- [Faucet.sol](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/faucet/Faucet.sol)

## [LOW] Approve 0 first
At some tokens you can approve an amount (at USDT for instance) only after approving to 0. Consider using increase/decrease approve notation instead.

### Proof of concept:
- [HolographERC20.sol#L387](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/HolographERC20.sol#L387)
- [HolographERC20.sol#L232](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/HolographERC20.sol#L232)

## [LOW] The project is compiled with different solidity versions


## [NON CRITICAL] The following events are not indexed


### Proof of concept:
- [PA1D.sol#L153](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/enforcer/PA1D.sol#L153)
- [HolographGenesis.sol#L13](https://github.com/code-423n4/2022-10-holograph/tree/main/src/HolographGenesis.sol#L13)

## [NON CRITICAL] Missing function spec comments


### Proof of concept:
- [CxipERC721.sol#L111](https://github.com/code-423n4/2022-10-holograph/tree/main/src/token/CxipERC721.sol#L111)
- [Holograph.sol#L65](https://github.com/code-423n4/2022-10-holograph/tree/main/src/Holograph.sol#L65)

## [NON CRITICAL] Consider emitting an event at the following functions


### Proof of concept:
- [HolographERC20.sol#L119](https://github.com/code-423n4/2022-10-holograph/tree/main/src/enforcer/HolographERC20.sol#L119)
- [StrictERC20H.sol#L181](https://github.com/code-423n4/2022-10-holograph/tree/main/contracts/abstract/StrictERC20H.sol#L181)
