## `constant` Variables Should be Capitalized
Constants should be named with all capital letters with underscores separating words if need be. Here are some of the instances entailed:

https://github.com/code-423n4/2022-10-holograph/blob/main/src/HolographBridge.sol#L27-L43
https://github.com/code-423n4/2022-10-holograph/blob/main/src/HolographOperator.sol#L30-L54
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L127-L131
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L126-L146

Please visit the following style guide for more details:

https://docs.soliditylang.org/en/v0.8.14/style-guide.html

## OpenZeppelin's Initializable
`init()` of `HolographBridge.sol`, `HolographOperator.sol`, `HolographFactory.sol`, and `LayerZeroModule.sol` uses `!_isInitialized()` to prevent re-initialization of the contract.

https://github.com/code-423n4/2022-10-holograph/blob/main/src/HolographBridge.sol#L64
https://github.com/code-423n4/2022-10-holograph/blob/main/src/HolographOperator.sol#L142
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L144
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L159

Consider using OpenZeppelin's Initializable which will have this scenario better taken care of. When a contract is initialized, its `isInitialized` state variable is set to true.

## Inadequately Commented Assembly Block
Assembly block is used in numerous contracts of Holograph. While this does not pose a security risk per se, it is at the same time a complicated and critical part of the system. Moreover, as this is a low-level language that is harder to parse by readers, consider including extensive documentation regarding the rationale behind its use, clearly explaining what every single assembly instruction does. This will make it easier for users to trust the code, for reviewers to verify it, and for developers to build on top of it or update it. Note that the use of assembly discards several important safety features of Solidity, which may render the code less safe and more error-prone.

Here are some of the instances entailed:

https://github.com/code-423n4/2022-10-holograph/blob/main/src/HolographBridge.sol#L325-L336
https://github.com/code-423n4/2022-10-holograph/blob/main/src/HolographOperator.sol#L571-L582
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/Holographer.sol#L230-L241

## Use of `ecrecover` is Susceptible to Signature Malleability
The built-in EVM pre-compiled `ecrecover` is susceptible to signature malleability due to non-unique v and s (which may not always be in the lower half of the modulo set) values, possibly leading to replay attacks. Devoid of the adoption of nonces, this could prove a vulnerability when not carefully used.

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L333-L334

Consider using OpenZeppelin’s ECDSA library which has been time tested in preventing this malleability where possible.

## Non-Assembly Method Alternative
Automated tools would typically flag a contract using inline-assembly as having high complexity, poor readability and error prone as far as security is concerned. As such, avoid using it where possible. For instance, the following block of codes could be replaced with:

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L310-L313

```
bytes32 codehash = contractAddress.codehash;
```
Similarly, there is a newer syntax way to invoke create2 without assembly, you just need to pass salt and the contract constructor arguments just as in the following instance as an example:

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L235-L246

```
address sourceContractAddress = address(new <ContractName>{salt: saltInt}(<constructor argument(s)>));
```

Please visit the following links for further details:

https://solidity-by-example.org/app/create2/
https://docs.soliditylang.org/en/latest/control-structures.html#salted-contract-creations-create2

## Lack of Events for Critical Operations
Critical operations not triggering events will make it difficult to review the correct behavior of the deployed contracts. Users and blockchain monitoring systems will not be able to detect suspicious behaviors at ease without events. Consider adding events where appropriate for all critical operations for better support of off-chain logging API. Here are some of the instances entailed:

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L320-L324
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L340-L344
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L360-L364
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L380-L384
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L441-L445
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L470-L474

## Add a Timelock to Critical Parameter Change
It is a good practice to give time for users to react and adjust to critical changes with a mandatory time window between them. The first step merely broadcasts to users that a particular change is coming, and the second step commits that change after a suitable waiting period. This allows users that do not accept the change to withdraw within the grace period. A timelock provides more guarantees and reduces the level of trust required, thus decreasing risk for users. It also indicates that the project is legitimate (less risk of a malicious Owner making any malicious or ulterior intention). Specifically, privileged roles could use front running to make malicious changes just ahead of incoming transactions, or purely accidental negative effects could occur due to the unfortunate timing of changes. Here are some of the instances entailed:

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L320-L324
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L340-L344
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L360-L364
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L380-L384
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L441-L445
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L470-L474

## Contract Owner Has Too Many Privileges
The owner of the contracts has too many privileges relative to standard users. The consequence is disastrous if the contract owner's private key has been compromised. And, in the event the key was lost or unrecoverable, no implementation upgrades and system parameter updates will ever be possible.

For a Holograph Project project this grand, it increases the likelihood that the owner will be targeted by an attacker, especially given the insufficient protection on sensitive owner private keys. The concentration of privileges creates a single point of failure; and, here are some of the incidents that could possibly transpire:

Change admin and mess up with all the setter functions, hijacking the entire protocol.

Consider:
1) splitting privileges (e.g. via the multisig option) to ensure that no one address has excessive ownership of the system,
2) clearly documenting the functions and implementations the owner can change,
3) documenting the risks associated with privileged users and single points of failure, and
4) ensuring that users are aware of all the risks associated with the system.

## Function Calls in Loop Could Lead to Denial of Service
Function calls made in unbounded loop are error-prone with potential resource exhaustion as it can trap the contract due to the gas limitations or failed transactions. Here are some of the instances entailed:

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L307
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L323
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L356

Consider bounding the loop where possible to avoid unnecessary gas wastage and denial of service.

## Commented Code
Throughout the codebase `HolographERC721`, there are lines of code that have been commented out with //. This can lead to confusion and is detrimental to overall code readability. Consider removing commented out lines of code that are no longer needed.

Here are some of the instances entailed:

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L527-L537
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L542-L552
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L557-L570

## Expression for Calculated Values
Long bytes of literal values assigned to constants may incur typo/human error. Considering assigning these constants with their corresponding expression. For instance, the following code line could be refactored to:

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L126

```
  bytes32 constant _factorySlot = bytes32(uint256(keccak256('eip1967.Holograph.factory')) - 1)
```
Note: The calculated byte value may be included as a comment instead.

## Require Statement at the Top of Code Block
It is a good practice to allow an anticipated failed transaction to transpire as early as possible to avoid any unnecessary wastage of gas. Among all operator inputted parameters, `doNotRevert' is a boolean which will directly determine whether or not the function call is going to revert. Consider placing the following require statement at the beginning of the code block: 
 
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L233

## Use `delete` to Clear Variables
`delete a` assigns the initial value for the type to `a`. i.e. for integers it is equivalent to `a = 0`, but it can also be used on arrays, where it assigns a dynamic array of length zero or a static array of the same length with all elements reset. For structs, it assigns a struct with all members reset. Similarly, it can also be used to set an address to zero address. It has no effect on whole mappings though (as the keys of mappings may be arbitrary and are generally unknown). However, individual keys and what they map to can be deleted: If `a` is a mapping, then `delete a[x]` will delete the value stored at x.

The delete key better conveys the intention and is also more idiomatic. Consider replacing assignments of zero with delete statements. Here are some of the instances entailed:

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L399
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L924
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L1132-L1136

## `block.timestamp` Unreliable
The use of `block.timestamp` as part of the time checks can be slightly altered by miners/validators to favor them in contracts that have logic strongly dependent on them.

Consider taking into account this issue and warning the users that such a scenario could happen. If the alteration of timestamps cannot affect the protocol in any way, consider documenting the reasoning and writing tests enforcing that these guarantees will be preserved even if the code changes in the future.

Here are some of the instances entailed:

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L345
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L531

The following instance entailing the generation of a random number is of particular concern although it has been documented that `_jobNonce()` will have the risk taken care of:

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L499

## Inadequate NatSpec
Solidity contracts can use a special form of comments, i.e., the Ethereum Natural Language Specification Format (NatSpec) to provide rich documentation for functions, return variables and more. Please visit the following link for further details:

https://docs.soliditylang.org/en/v0.8.16/natspec-format.html

Here are some of the functions lacking @param comments:

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L445
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L484
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L1122
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L1160
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L1185

## Open TODOs
Open TODOs can point to architecture or programming issues that still need to be resolved. Consider resolving them before deploying.

Here is one of the instances entailed:

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L274-L277

## Not Completely Using OpenZeppelin Contracts
OpenZeppelin maintains a library of standard, audited, community-reviewed, and battle-tested smart contracts. Instead of always importing these contracts, Holograph project re-implements them in some cases. This increases the amount of code that the Holograph team will have to maintain and miss all the improvements and bug fixes that the OpenZeppelin team is constantly implementing with the help of the community.

Consider importing the OpenZeppelin contracts instead of re-implementing or copying them. These contracts can be extended to add the extra functionalities required by Holograph. Here are some of the instances entailed:

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L104
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L115

## Minimization of Truncation
As an example, each respective code line below may be refactored to minimize the frequency of truncation:

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L274

```
    return (gasPrice * gasLimit * 11 * dstPriceRatio / 10**11, nativeFee);
```
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L293

```
    return gasPrice * gasLimit * 11 * dstPriceRatio / 10**11;
```
Note: The original code expression may be commented to augment the refactored expression. 

## Lines Too Long
Lines in source code are typically limited to 80 characters, but it’s reasonable to stretch beyond this limit when need be as monitor screens theses days are comparatively larger. Considering the files will most likely reside in GitHub that will have a scroll bar automatically kick in when the length is over 164 characters, all code lines and comments should be split when/before hitting this length. Keep line width to max 120 characters for better readability where possible. Here is one of the instances entailed:

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L289

## No Withdraw Function for ETH
Contracts receiving native token payments via `receive() external payable {}` have no method to withdraw the Ether. Devoid of implementing a way to withdraw funds or send them to another account, there is no built in way to release the money. The Ether is stuck on the contract balance forever. Here are some some of the instances entailed:

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L251
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L962