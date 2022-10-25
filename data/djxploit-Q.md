## 1) Avoid using `ecrecover`

`ecrecover` is vulnerable to signature malleability. So it is better to use ECDSA library to eliminate the risk of signature malleability.

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L333-L334

## 2) Missing storage gap for upgradeable contracts

Storage gap is necessary for upgradeable contracts, to prevent storage collision due to future developments. So according to openzeppelin it is recommended to add a storage gap of `uint _gap[50]`.
Below upgradeable contracts are missing such storage gaps :
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L125
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L123
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/Holographer.sol#L115
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L122

## 3) Missing events and timelock mechanisms for critical functions

Some critical functions which updates an important part of the protocol, should emit the updated record, to let the offchain users know about the change. 
For example, in https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L502,
`setOperator` is a function to set the operator, which is a critical aspect of the protocol. Hence the function should emit to let the other users know about the change. 
Also , timelocks are also needed , in case, where malicious operator is set, to give the users time , to act upon it.

Other such critical functions are :
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L278-L285
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L472
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L522

## 4) Dependence on `block.timestamp`

`block.timestamp` are riskier to use, as they can be manipulated by the miners. So any logic that depends on it, is vulnerable to manipulation.
Similarly other block attributes like `block.number` are also susceptible to miner manipulation. Hence we should avoid them.

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L525
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L1191
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L345
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L469

## 5) `require` should have descriptive reasons on failure to enhance the readability of the source code

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L606
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L502
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L484
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L391
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L373
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L460

## 6) Events are missing `indexed` fields

`indexed` field allows offchain monitoring tools to find the emitted event faster.  
In https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L153, all argument of `SecondarySaleFees` event are missing `indexed` field

## 7) TODO comments

TODO comments should be removed before going into production. So complete the TODO in https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L701, before deploying the project



