##  `UPGRADEABLE` CONTRACT IS MISSING A `__GAP[50]` STORAGE VARIABLE TO ALLOW FOR NEW STORAGE VARIABLES IN LATER VERSIONS

While some contracts may not currently be sub-classed, adding the variable now protects against forgetting to add it in the future.

* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L122
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L125
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L123
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L122
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/Holographer.sol#L115
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L120
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L132

## `require()`/`revert()` STATEMENTS SHOULD HAVE DESCRIPTIVE REASON STRINGS

* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L373
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L378
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L332
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L430
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L328
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L322
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L474
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L341
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L417
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/Holographer.sol#L236
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L979
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L656

## OPEN `TODOS`

Code architecture, incentives, and error handling/reporting questions/issues should be resolved before deployment.

* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L701

## `block.timestamp` and `block.number` are not be used a source or reference of time. Instead use oracle or verifiable random numbers

* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L469
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L345
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L499
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L525
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L531
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L1191

## MISSING `EVENT` AND OR `TIMELOCK` FOR CRITICAL PARAMETER CHANGE

Events help non-contract tools to track changes, and events prevent users from being surprised by changes.

* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L472
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L502
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L522
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L452
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L949
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L969
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L989
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L280
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L300
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L320
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L340


## Use of `ecrecover()` is deprecated. Consider adding checks for signature malleability.

Use OpenZeppelin’s ECDSA contract rather than calling `ecrecover()` directly.

* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L333-L334

## `Event` is missing indexed fields

Index event fields make the field more quickly accessible to off-chain tools that parse events. However, note that each index field costs extra gas during emission, so it’s not necessarily best to index the maximum allowed per event (three fields). Each event should use three indexed fields if there are three or more fields, and gas usage is not particularly of concern for the events in question. If there are fewer than three fields, all of the fields should be indexed.

* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L153

## Set `garbage` value in `mapping` for deleting that

If there is a mapping data structure present inside struct, then deleting the struct doesn't delete the mapping. Instead one should use lock to lock that data structure from further use.

* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L329
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L405
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L805

## Missing initializer`modifier` on constructor

OpenZeppelin recommends that the initializer modifier be applied to constructors in order to avoid potential griefs, social engineering, or exploits. Ensure that the modifier is applied to the implementation contract. If the default constructor is currently being used, it should be changed to be an explicit one with the modifier applied.

All the contracts present inside scope

* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L155
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L233
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L136
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L151
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/Holographer.sol#L140
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L166
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L231
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L211
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC721H.sol#L133
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC20H.sol#L133

## Using abi.decode(arg1,arg2) , is not safe that if `arg1` or `arg2` is `null`, or is `user-controllable`(returning null).

When using abi.decode(arg1,arg2) , one should keep in mind that if arg1 or arg2 is null, or is user-controllable(returning null), then the abi.decode() will revert. If such cases are not handled, then it may lead to revert the whole function where abi.decode was, and may lead to DOS-kind of situation, based on the logic of the function

* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/Holographer.sol#L165
