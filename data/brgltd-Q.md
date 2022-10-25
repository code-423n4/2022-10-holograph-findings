## [01] Consistent use of ECDSA.recover

## Impact

`HolographERC20.permit()` is using ECDSA.recover. However, `HolographFactory._verifySigner()` is using ecrecover. Consider using only `ECDSA.recover` on all instances for signature verification. The function ecrecover is [vulnarable](https://swcregistry.io/docs/SWC-117) to replay attacks and signature malleability.

## Proof of Concept

Usage of ecrecover in `HolographFactory`.

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L333-L334

## Recommended Mitigation Steps

Replace ecrecover with `ECDSA.recover`. Note that the return of both `ECDSA.recover` and ecrecover need checks for returned address different than zero. In `HolographFactory.verifySigner` if the hash signature returns address zero, and the `signer` suppplied at `HolographFactory.deployHolographableContract` is also zero, the validation will pass causing slient failures.

The following change is recommended:

```
diff --git a/HolographFactory.sol.orig b/HolographFactory.sol
--- a/HolographFactory.sol.orig
+++ b/HolographFactory.sol
-   return (ecrecover(keccak256(abi.encodePacked("\x19Ethereum Signed Message:\n32", hash)), v, r, s) == signer ||
-   ecrecover(hash, v, r, s) == signer);
+   return signer != address(0) && (
+     ECDSA.recover(keccak256(abi.encodePacked("\x19Ethereum Signed Message:\n32", hash)), v, r, s) == signer ||
+     ECDSA.recover(hash, v, r, s) == signer
+   );
```

## [02] Lack of checks in `_mint` to validate if the recipient is able to receive ERC721

## Impact

`HolographERC721_mint()` and `HolographERC721.bridgeIn()` will not check if the `to` address is capable of receiving the ERC721. This could lead to loss of funds/assets.

## Proof of Concept

`HolographERC721.bridgeIn()`and `HolographERC721_mint()` won't check the receiver address.

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L814-L822

## Recommended Mitigation Steps

Implement checks on the recipient address to validate if it's capable of receiving NFTs. This [link](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/token/ERC721/ERC721.sol#L429-L451) contains OpenZeppelin implementation.

# [03] `block.number` and `block.timestamp` are not strong sources of entropy

Randomness is not a trivial task on the blockchain. Using `block.number` and `block.timestamp` is not recommended.

[Reference 1](https://solidity-by-example.org/hacks/randomness/)

[Reference 2](https://ethereum.stackexchange.com/questions/108033/what-do-i-need-to-be-careful-about-when-using-block-timestamp)

## Proof of Concept

Usage of `block.number` and `block.timestamp` for entropy in `HolographOperator.crossChainMessage()`.

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L484-L539

## Recommended Mitigation Steps

Evaluate the usage of a system such as chainlink to provide stronger randomness guarantees and prevent randomness bugs for the Holograph operator cross chain messaging.

# [04] Usage of solidity 0.8.13 which has know assembly issues

## Impact

The project makes heavy usage of assembly and v0.8.13 of solidity has a issue related to the yul optimizer mistakenly removing memory writes.

## Proof of Concept

Multiple assembly blocks with memory writes. 

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L445-L455

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L550-L555

[Detail description](https://blog.soliditylang.org/2022/06/15/inline-assembly-memory-side-effects-bug/) of the bug.

## Recommended Mitigation Steps

Update the project to use solidity >0.8.15, which solves the assembly issue.

# [05] Lack of zero address checks for setter and initializers

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L949

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L240-L245

If a variable gets configured with address zero, failure to immediately reset the value can result in unexpected behavior for the project.

# [06] Critical changes should use two-step procedure

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L949

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L969

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L1009

Lack of two-step procedure for critical operations leaves them error-prone. Consider adding two step procedure on the critical functions.

Consider adding a two-steps pattern on critical changes to avoid mistakenly transferring ownership of roles or critial functionalities to the wrong address.

# [07] Missing event for parameter changes

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L949

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L969

Adding events will faciliate offchain monitoring.

# [08] Identifier to write comments

Some commends are using /**/, while other are using ///. E.g.

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L492-L498

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L459-L468

Consider using the same approach to write comments throughout the codebase. 

# [09] Prefer the usage of scientific notation instead of exponentiation

10**18 = 10e18

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L256

# [10] Fix typo

rever = revert

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L553

# [11] Cache msg.value

msg.value is computed three times in `HologaphOperator.send()`. Consider caching this value in the beginning of the function.

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L595

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L640

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L645

# [12] Order of functions

The solidity [documentation](https://docs.soliditylang.org/en/v0.8.17/style-guide.html#order-of-functions) recommends the following order for functions:

constructor
receive function (if exists)
fallback function (if exists)
external
public
internal
private

The following instances shows internal above external.

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC721H.sol#L159

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC721H.sol#L174

# [13] Missing NATSPEC

Consider adding NATSPEC on all functions to improve documentation.

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L580

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L399

# [14] Remove commended code

Consider removing or resolving commended code sections.

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L527-L570

# [15] Open TODO

Todo's should be resolved before mainnet deployment.

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L701

# [16] Public functions not called by the contract should be declared external

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L347
