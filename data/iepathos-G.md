## Gas Optimizations

### G-01 Use a local variable to cache .length calls in for loops

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L564

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L871

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L432
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L437
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L454
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L474


### G-02 Use ++i prefix instead of i++ postfix where possible to save gas

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L307
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L323
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L340
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L356
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L394
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L414
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L437
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L454
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L474

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L357
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L716

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L564

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L781
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L871


### G-03 In the EVM, there is no opcode for >= or <=. When using greater than or equal, two operations are performed: > and = and the same for <=. Use strict comparison operators to save gas.

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L469

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L863
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L871

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L354
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L386
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L728
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L739
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L756
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L1169

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L349
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L365
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L400
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L427
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L450
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L529
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L599
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L629
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L698


### G-04 Identifying a function as payable saves gas because it lowers the number of opcodes being executed. Functions that have the onlyOwner modifier cannot be called by normal users and will not mistakenly receive ETH. These functions can be payable to save gas.

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L471
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L533


### G-05 Similar to adding payable modifier to onlyOwner functions to save gas, we can do the same for onlyAdmin functions to avoid the extra opcodes being executed and save some gas.  These functions can be payable to save gas.

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L278-L285
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L949
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L969
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L989
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L1009
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L1029
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L1049

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L452
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L472
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L502
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L522

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L280
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L300


### G-06 Using `> 0` is less efficient than using `!= 0`. Use `!= 0` when comparing uint variables to zero, which cannot hold values below zero.

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L309
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L350
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L363
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L398
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L1126

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L218

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L815