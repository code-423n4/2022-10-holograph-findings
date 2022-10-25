# 1) Use calldata instead of memory in read only functions
If a reference type function parameter is read-only, it is cheaper in gas to use calldata instead of memory.
3 Instances:
HolographBridge.sol
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L324

HolographERC20.sol
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L310
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L318

Recommended mitigation steps:
Consider changing memory with calldata in read-only functions


# 2) Prefix increments
Prefix increments are cheaper than postfix increments - 6 gas. 
11 Instances:
HolographERC20.sol
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L564

HolographERC721.sol
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L357
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L716

HolographOperator.sol
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L781
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L871

PA1D.sol
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L307
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L321
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L340
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L356
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L454
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L474

Recommended mitigation steps:
Consider changing i++ to ++i.


# 3) Bytes32 constants are cheaper than string constants
If the string can fit into 32 bytes, then bytes32 is cheaper than string.
5 Instances:
HolographBridge.sol
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L324

HolographERC20.sol
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L310
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L318

HolographERC721.sol
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L282
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L313

Recommended mitigation steps:
Consider changing strings to bytes32