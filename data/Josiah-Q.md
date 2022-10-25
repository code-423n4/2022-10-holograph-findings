## COMMENT AND CODE LINE LENGTH
Try limit the length of comments and/or code lines to 80 - 100 character long for readability sake. Here is one instance found.

[Line 289](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L289)

## CAPITALIZATION OF CONSTANTS
As documented in the link below:

https://docs.soliditylang.org/en/v0.8.17/style-guide.html?highlight=capitalized

Constants should be named with all capital letters with underscores separating words. There are numerous instances found.

[Lines 120 - 152](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/Holograph.sol#L120-L152)
[Lines 126 - 142](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L126-L142)
[Lines 127 -131](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L127-L131)
[Lines 129 - 153](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L129-L153)
[Lines 126 - 146](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L126-L146)
[Lines 124 - 144](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L124-L144)
[Lines 136 - 140](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L136-L140)
[Lines 142 - 146](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L142-L146)
[Lines 110 - 114](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC721H.sol#L110-L114)
[Lines 110 - 114](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC20H.sol#L110-L114)

## UNUSED RECEIVE() FUNCTION WILL LOCK ETHER IN CONTRACT
If the intention is for the Ether to be used, the function should call another function, otherwise it should revert. There are five instances found.

[Line 212](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC20H.sol#L212)
[Line 212](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC721H.sol#L212)
[Line 223](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/Holographer.sol#L223)
[Line 962](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L962)
[Line 209](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L1209)
[Line 251](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L251)

## USE OF BLOCK.TIMESTAMP
Some contract code uses the block.timestamp as part of the calculations and time checks. Nevertheless, timestamps can be slightly altered by miners/validators to favor them in contracts that have logics that depend strongly on them.

Consider taking into account this issue and warning the users that such a scenario could happen. If the alteration of timestamps cannot affect the protocol in any way, consider documenting the reasoning and writing tests enforcing that these guarantees will be preserved even if the code changes in the future. Here are four instances found.

[Line 469](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L469)
[Line 345](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L345)
[Line 531](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L531)
[Line 499](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L499)

Note that the last instance that involves a random number generation would need to be used with much care although it might have been mitigated by `jobHash` and _jobNonce()`. Elsewhere, it could have easily been manipulated by the miners or validators.

## NOT USING OPENZEPPELIN CONTRACTS
 OpenZeppelin maintains a library of standard, audited, community-reviewed, and battle-tested smart contracts. Instead of always importing these contracts, the Holograph project reimplements them in some cases, while in other cases it just copies them. This increases the amount of code that the Holograph team will have to maintain and misses all the improvements and bug fixes that the OpenZeppelin team is constantly implementing with the help of the community. 

Consider importing the OpenZeppelin contracts instead of reimplementing or copying them. These contracts can be extended to add the extra functionalities required by Holograph. Here are some instances found.

[Line 105](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L105)
[Line 115](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L115)
[Line 107](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L107)
[Line 130](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L130)

## PAYABLE(<ADDRESS>).TRANSFER COULD CAUSE ETHER UNABLE TO SEND
When sending ETH, `HolographOperator.sol` uses Solidity’s transfer function. This has some notable shortcomings when the address is a smart contract, which can render ETH impossible to withdraw. Specifically, the withdrawal will inevitably fail when: 1) The withdrawer smart contract does not implement a payable fallback function. 2) The withdrawer smart contract implements a payable fallback function which uses more than 2300 gas units. 3) The withdrawer smart contract implements a payable fallback function which needs less than 2300 gas units but is called through a proxy that raises the call’s gas usage above 2300. Here is an instance found.

[Line 497](https://github.com/code-423n4/2022-10-holograph/blob/main/src/HolographOperator.sol#L497)

It is recommended that `sendValue` function available in OpenZeppelin Contract’s Address library can be used to transfer the withdrawn Ether without being limited to 2300 gas units. Alternatively, a low level `call()` may also be adopted. Risks of reentrancy stemming from the use of this function can be mitigated by tightly following the “Check-effects-interactions” pattern and using OpenZeppelin Contract’s ReentrancyGuard contract.

## MISSING EVENTS ON CRITICAL OPERATIONS
Several critical operations do not trigger events, which will make it difficult to review the correct behavior of the contracts once deployed. Users and blockchain monitoring systems will not be able to easily detect suspicious behaviors without events. There are numerous instances found.

[Lines 340 - 344](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L340-L344)
[Lines 380 - 384](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L380-L384)
[Lines 470 - 474](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L470-L474)

## Add a Timelock to Critical Parameter Change
It is a good practice to give time for users to react and adjust to critical changes with a mandatory time window between them. The first step merely broadcasts to users that a particular change is coming, and the second step commits that change after a suitable waiting period. This allows users that do not accept the change to withdraw within the grace period. A timelock provides more guarantees and reduces the level of trust required, thus decreasing risk for users. It also indicates that the project is legitimate (less risk of a malicious Owner making any malicious or ulterior intention). Specifically, privileged roles could use front running to make malicious changes just ahead of incoming transactions, or purely accidental negative effects could occur due to the unfortunate timing of changes. Here are some of the instances entailed:

[Lines 340 - 344](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L340-L344)
[Lines 360 - 364](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L360-L364)
[Lines 441 -445](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L441-L445)

## SETTING TO DEFAULT VALUES
As documented in the link:

https://docs.soliditylang.org/en/v0.8.17/types.html?highlight=delete#delete

It is recommended using `delete` whenever there is a need to set state variable to its default value, which has a good side effect of saving gas. Here are the instances found.

[Lines 667 - 668](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L667-L668)
[Line 399](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L399)
[Line 924](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L924)
[Lines 1132 - 1136](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L1132-L1136)

## DOS ON UNBOUNDED LOOP
Unbounded loop could lead to OOG (Out of Gas) denying the users' of needed services. Here are some instances found.

[Line 781](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L781)
[Line 564](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L564)
[Line 357](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L357)
[Line 716](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L716)

## UNCOMMENTED ASSEMBLY BLOCK
Assembly is a low-level language that is harder to parse by readers, consider including extensive documentation regarding the rationale behind its use, clearly explaining what every single assembly instruction does. This will make it easier for users to trust the code, for reviewers to verify it, and for developers to build on top of it or update it. Note that the use of assembly discards several important safety features of Solidity, which may render the code unsafer and more error-prone. Here are some instances found.

[Lines 937 - 997](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L973-L997)
[Lines 218 -227](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC721H.sol#L218-L227)

## ECRECOVER SUBJECT TO REPLAY ATTACK
Whenever possible, use OpenZeppelin’s ECDSA library which has been time tested in preventing replay attack of signature malleability associated with `ecrecover'. The issue could arise from the non-unique value of v and an s value that could end up in the upper half of the modulo set. Here are the instances found.

[Lines 333 - 334](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L333-L334)
