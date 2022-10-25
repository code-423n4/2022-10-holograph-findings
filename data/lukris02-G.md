# Gas Optimizations Report for Holograph contest

## Overview
During the audit, 3 gas issues were found.  
Total savings more than 1100.

№ | Title | Instance Count
--- | --- | --- 
G-1| [Extra operation with an array](#g-1-extra-operation-with-an-array) | 1
G-2 | [Use unchecked blocks for incrementing i](#g-2-use-unchecked-blocks-for-incrementing-i) | 14
G-3 | [Use ```calldata``` instead of ```memory for``` read-only arguments](#g-3-use-calldata-instead-of-memory-for-read-only-arguments) | 10

## Gas Optimizations Findings (3)
### G-1. Extra operation with an array
##### Description
There is no need to [delete (make zero) element](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L1148) that will be [popped](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L1152).

##### Instances
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L1148

##### Recommendation
Remove this line of code.

#
### G-2. Use unchecked blocks for incrementing i
##### Description
In Solidity 0.8+, there’s a default overflow and underflow check on unsigned integers. In the loops, "i" will not overflow because the loop will run out of gas before that.

##### Instances
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L781
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L307
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L323
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L340
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L356
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L394
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L414
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L432
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L437
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L454
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L474
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L357
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L716
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L564

##### Recommendation
Change:
```
for (uint256 i; i < n; ++i) {
 // ...
}
```
to:
```
for (uint256 i; i < n;) { 
 // ...
 unchecked { ++i; }
}
```

##### Saved
This saves ~30-40 gas per iteration.  
So, ~35*14 = 490

#
### G-3. Use ```calldata``` instead of ```memory for``` read-only arguments
##### Description
Since Solidity v0.6.9, memory and calldata are allowed in all functions regardless of their visibility type (See "Calldata Variables" section [here](https://blog.soliditylang.org/2020/06/05/Solidity-069-release-announcement/)).  
When function arguments should not be modified, it is cheaper to use calldata.

##### Instances
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L316
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L349
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L365
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L372
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L426
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L471
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L517
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L683
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L456
- https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L620

##### Recommendation
Use calldata where possible.

##### Saved
This saves at least 60 gas per iteration.  
So, ~60*10 = 600