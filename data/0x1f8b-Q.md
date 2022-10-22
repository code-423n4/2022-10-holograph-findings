- [Low](#low)
    - [**1. Allows malleable SECP256K1 signatures**](#1-allows-malleable-secp256k1-signatures)
    - [**2. Mixing and Outdated compiler**](#2-mixing-and-outdated-compiler)
    - [**3. Method marked as payable unnecessarily**](#3-method-marked-as-payable-unnecessarily)
    - [**4. Be resistant to hash collisions**](#4-be-resistant-to-hash-collisions)
    - [**5. Unsafe math**](#5-unsafe-math)
    - [**6. Lack of no reentrant modifier**](#6-lack-of-no-reentrant-modifier)
    - [**7. Don't publish internal methods**](#7-dont-publish-internal-methods)
    - [**8. jobNonce was not increased**](#8-jobnonce-was-not-increased)
    - [**9. Lack of checks supportsInterface**](#9-lack-of-checks-supportsinterface)
- [Non critical](#non-critical)
    - [**10. Change abstract method to virtual**](#10-change-abstract-method-to-virtual)
    - [**11. Avoid hardcoded values**](#11-avoid-hardcoded-values)

# Low

## **1. Allows malleable `SECP256K1` signatures**

Here, the `ecrecover()` method doesn't check the `s` range.

Homestead ([EIP-2](https://eips.ethereum.org/EIPS/eip-2)) added this limitation, however the precompile remained unaltered. The majority of libraries, including OpenZeppelin, do this check.

Also there was no check of the `address(0)`, so using a signer with an empty address can by pass the signature and sign arguments in `deployHolographableContract`.

**Reference:**

- https://github.com/OpenZeppelin/openzeppelin-contracts/blob/7201e6707f6631d9499a569f492870ebdd4133cf/contracts/utils/cryptography/ECDSA.sol#L138-L149

**Affected source code:**

- [HolographFactory.sol:333](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographFactory.sol#L333)

## **2. Mixing and Outdated compiler**

The pragma version used are:

```
pragma solidity 0.8.13;
```

*Note that mixing pragma is not recommended. Because different compiler versions have different meanings and behaviors, it also significantly raises maintenance costs. As a result, depending on the compiler version selected for any given file, deployed contracts may have security issues.*

The minimum required version must be [0.8.17](https://github.com/ethereum/solidity/releases/tag/v0.8.17); otherwise, contracts will be affected by the following **important bug fixes**:

[0.8.14](https://blog.soliditylang.org/2022/05/18/solidity-0.8.14-release-announcement/):

- ABI Encoder: When ABI-encoding values from calldata that contain nested arrays, correctly validate the nested array length against `calldatasize()` in all cases.
- Override Checker: Allow changing data location for parameters only when overriding external functions.

[0.8.15](https://blog.soliditylang.org/2022/06/15/solidity-0.8.15-release-announcement/)

- Code Generation: Avoid writing dirty bytes to storage when copying `bytes` arrays.
- Yul Optimizer: Keep all memory side-effects of inline assembly blocks.

[0.8.16](https://blog.soliditylang.org/2022/08/08/solidity-0.8.16-release-announcement/)

- Code Generation: Fix data corruption that affected ABI-encoding of calldata values represented by tuples: structs at any nesting level; argument lists of external functions, events and errors; return value lists of external functions. The 32 leading bytes of the first dynamically-encoded value in the tuple would get zeroed when the last component contained a statically-encoded array.

[0.8.17](https://blog.soliditylang.org/2022/09/08/solidity-0.8.17-release-announcement/)

- Yul Optimizer: Prevent the incorrect removal of storage writes before calls to Yul functions that conditionally terminate the external EVM call.

Apart from these, there are several minor bug fixes and improvements.

## **3. Method marked as `payable` unnecessarily**

The following methods have been marked as `payable`, this may cause both administrators and users calling them to accidentally deposit ether with no recovery possible.

This may be done to save gas, but may result in more loss than benefit in case of one human error.

**Affected source code:**

- [HolographERC721.sol:368](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L368)
- [HolographERC721.sol:440](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L440)
- [HolographERC721.sol:457](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L457)
- [HolographERC721.sol:588](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L588)
- [HolographERC721.sol:603](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L603)
- [HolographERC721.sol:621](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L621)

## **4. Be resistant to hash collisions**

`abi.encodePacked` with dynamic types does not pad the elements, so it is possible to create a `keccack` collision for attackers if there are multiple string or byte arguments.

**Affected source code:**

- [HolographFactory.sol:206-215](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographFactory.sol#L206-L215)
- [HolographFactory.sol:226](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographFactory.sol#L226)
- [HolographFactory.sol:333](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographFactory.sol#L333)
- [HolographOperator.sol:499](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L499)
- [PA1D.sol:257](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L257)
- [PA1D.sol:269](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L269)
- [PA1D.sol:280](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L280)
- [PA1D.sol:292](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L292)
- [PA1D.sol:308](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L308)
- [PA1D.sol:324](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L324)
- [PA1D.sol:341](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L341)
- [PA1D.sol:357](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L357)
- [PA1D.sol:366](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L366)
- [PA1D.sol:373](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L373)

## **5. Unsafe math**

A user supplied argument can be passed as zero to multiplication operation.

**Reference:**

- https://slowmist.medium.com/analysis-of-the-treasuredao-zero-fee-exploit-73791f4b9c14

**Affected source code:**

- [PA1D.sol:551](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L551)
- [PA1D.sol:553](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L553)
- [PA1D.sol:641](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L641)
- [PA1D.sol:643](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L643)
- [LayerZeroModule.sol:274](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/module/LayerZeroModule.sol#L274)
- [LayerZeroModule.sol:293](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/module/LayerZeroModule.sol#L293)

## **6. Lack of no reentrant modifier**

The `PA1D.getEthPayout`, `PA1D.getTokenPayout` and `PA1D.getTokensPayout` methods do not have the `noReentrant` modifier and make calls to an external contract that can take advantage of and call these methods again, but it seems to fail due to the lack of tokens.

However, if any of the other addresses used their receive event to provide liquidity to the contract, the attacking account could benefit from it.

```js
  function getEthPayout() public {
    _validatePayoutRequestor();
    _payoutEth();
  }

  /**
   * @notice Get payout for a specific token address. Token must have a positive balance!
   * @dev Contract owner, admin, identity wallet, and payout recipients can call this function.
   * @param tokenAddress An address of the token for which to issue payouts for.
   */
  function getTokenPayout(address tokenAddress) public {
    _validatePayoutRequestor();
    _payoutToken(tokenAddress);
  }

  /**
   * @notice Get payouts for tokens listed by address. Tokens must have a positive balance!
   * @dev Each token balance must be equal or greater than 10000. Otherwise calculating BP is difficult.
   * @param tokenAddresses An address array of tokens to issue payouts for.
   */
  function getTokensPayout(address[] memory tokenAddresses) public {
    _validatePayoutRequestor();
    _payoutTokens(tokenAddresses);
  }
```

**Affected source code:**

- [PA1D.sol:497-520](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L497-L520)

## **7. Don't publish internal methods**

The comment says: `Do not call this function, it will always revert`, but that's not true, it won't revert if the external call throw an error, it's better to hide this method and change the visibility to internal becauthe the method contains a call to an external contract:

```diff
  function revertedBridgeOutRequest(
    address sender,
    uint32 toChain,
    address holographableContract,
    bytes calldata bridgeOutPayload
- ) external returns (string memory revertReason) {
+ ) internal returns (string memory revertReason) {
```

**Affected source code:**

- [HolographBridge.sol:296-301](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographBridge.sol#L296-L301)

## **8. `jobNonce` was not increased**

The method `HolographBridge.getBridgeOutRequestPayload` has the following comment:

> dev the latest job nonce is incremented by one

But that's not true because it only uses the current `jobNonce + 1`, it wasn't incremented, next time, it will be exactly the same number. Nothe that it's also not increased during the `HolographOperator.crossChainMessage` and it could cause the contract to not work as expected, and cause unexpected errors.

**Affected source code:**

- [HolographBridge.sol:393](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographBridge.sol#L393)
- [HolographOperator.sol:499](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L499)

## **9. Lack of checks `supportsInterface`**

The `EIP-165` standard helps detect that a smart contract implements the expected logic, prevents human error when configuring smart contract bindings, so it is recommended to check that the received argument is a contract and supports the expected interface.

**Reference:**

- https://eips.ethereum.org/EIPS/eip-165

**Affected source code:**

- [HolographBridge.sol:164](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographBridge.sol#L164)
- [HolographFactory.sol:145](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographFactory.sol#L145)
- [Holographer.sol:150](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/Holographer.sol#L150)
- [HolographOperator.sol:242](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L242)

---

# Non critical

## **10. Change `abstract` method to `virtual`**

It is better to use virtual to improve the extensibility of the contract.

**Affected source code:**

- [ERC20H.sol:174](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/abstract/ERC20H.sol#L174)
- [ERC721H.sol:175](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/abstract/ERC721H.sol#L175)

## **11. Avoid hardcoded values**

It is not good practice to hardcode values, but if you are dealing with addresses much less, these can change between implementations, networks or projects, so it is convenient to remove these values from the source code.

**Affected source code:**

Use the selector instead of the raw value:

- [HolographBridge.sol:126](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographBridge.sol#L126)
- [HolographBridge.sol:130](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographBridge.sol#L130)
- [HolographBridge.sol:134](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographBridge.sol#L134)
- [HolographBridge.sol:138](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographBridge.sol#L138)
- [HolographBridge.sol:142](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographBridge.sol#L142)
- [HolographFactory.sol:127](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographFactory.sol#L127)
- [HolographFactory.sol:131](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographFactory.sol#L131)
- [HolographOperator.sol:129](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L129)
- [HolographOperator.sol:133](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L133)
- [HolographOperator.sol:137](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L137)
- [HolographOperator.sol:141](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L141)
- [HolographOperator.sol:145](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L145)
- [HolographOperator.sol:149](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L149)
- [HolographOperator.sol:153](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/HolographOperator.sol#L153)
- [ERC20H.sol:110](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/abstract/ERC20H.sol#L110)
- [ERC20H.sol:114](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/abstract/ERC20H.sol#L114)
- [ERC721H.sol:110](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/abstract/ERC721H.sol#L110)
- [ERC721H.sol:114](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/abstract/ERC721H.sol#L114)
- [HolographERC20.sol:142](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC20.sol#L142)
- [HolographERC20.sol:146](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC20.sol#L146)
- [HolographERC721.sol:136](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L136)
- [HolographERC721.sol:140](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/HolographERC721.sol#L140)
- [Holographer.sol:119](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/Holographer.sol#L119)
- [Holographer.sol:123](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/Holographer.sol#L123)
- [Holographer.sol:127](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/Holographer.sol#L127)
- [Holographer.sol:131](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/Holographer.sol#L131)
- [Holographer.sol:135](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/Holographer.sol#L135)
- [PA1D.sol:124](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L124)
- [PA1D.sol:128](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L128)
- [PA1D.sol:132](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L132)
- [PA1D.sol:136](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L136)
- [PA1D.sol:140](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/enforcer/PA1D.sol#L140)
- [LayerZeroModule.sol:126](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/module/LayerZeroModule.sol#L126)
- [LayerZeroModule.sol:130](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/module/LayerZeroModule.sol#L130)
- [LayerZeroModule.sol:134](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/module/LayerZeroModule.sol#L134)
- [LayerZeroModule.sol:138](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/module/LayerZeroModule.sol#L138)
- [LayerZeroModule.sol:142](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/module/LayerZeroModule.sol#L142)
- [LayerZeroModule.sol:146](https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/module/LayerZeroModule.sol#L146)
