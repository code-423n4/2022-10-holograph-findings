## OPEN TODOS

* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L701

## `require()` STATEMENTS SHOULD HAVE DESCRIPTIVE REASON STRINGS

* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L373
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L378
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L332
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L430
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L328
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L759
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L624

## Use of Block.timestamp

Block timestamps have historically been used for a variety of applications, such as entropy for random numbers (see the Entropy Illusion for further details), locking funds for periods of time, and various state-changing conditional statements that are time-dependent. Miners have the ability to adjust timestamps slightly, which can prove to be dangerous if block timestamps are used incorrectly in smart contracts.

Navigate to the following contracts.

* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L469
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L345
* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L531

##  Missing indexed fields in `event`

* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L153

## Use of `ecrecover()` is deprecated.

* https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L333-L334

## Missing initializer`modifier` on constructor

    File: HolographBridge.sol, HolographOperator.sol, HolographFactory.sol, module/LayerZeroModule.sol,        
    enforcer/Holographer.sol, enforcer/PA1D.sol, enforcer/HolographERC721.sol, enforcer/HolographERC20.sol, 
    abstract/ERC721H.sol, abstract/ERC20H.sol.

All the above files misses `modifier` on constructor.
	