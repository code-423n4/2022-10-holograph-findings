
## 1. Multiple address/ID mappings can be combined into a single mapping of an address/ID to a struct, where appropriate
Saves a storage slot for the mapping. Depending on the circumstances and sizes of types, can avoid a Gsset (20000 gas) per mapping combined. Reads and subsequent writes can also be cheaper when a function requires both values and they both fit in the same storage slot. Finally, if both fields are accessed in the same function, can save ~42 gas per access due to not having to recalculate the key’s keccak256 hash (Gkeccak256 - 30 gas) and that calculation’s associated stack operations. 

`_ownedTokensIndex` and `_ownedTokensCount` and `_ownedTokens`
- [HolographERC721.sol#L170](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L170)


## 2. Add `unchecked {}` for subtractions where the operands cannot underflow because of a previous `require()` or `if` statement

require(a <= b); x = b - a => require(a <= b); unchecked { x = b - a }
if(a <= b); x = b - a => if(a <= b); unchecked { x = b - a }
this will stop the check for overflow and underflow so it will save gas

- [HolographOperator.sol#L359](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L359)
- [HolographOperator.sol#L729](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L729)
- [HolographOperator.sol#L740](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L740)

- [PA1D.sol#L391](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L391)


## 3. `<x> += <y>` costs more gas than `<x> = <x> + <y>` for state variables
Using the addition operator instead of plus-equals saves gas

- [HolographOperator.sol#L378](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L378)
- [HolographOperator.sol#L382](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L382)
- [HolographOperator.sol#L834](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L834)

- [HolographERC20.sol#L633](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L633)
- [HolographERC20.sol#L685](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L685)
- [HolographERC20.sol#L686](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L686)
- [HolographERC20.sol#L702](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L702)

## 4. not using the named return variables when a function returns, wastes deployment gas

- [HolographBridge.sol#L301](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L301)

- [HolographOperator.sol#L717](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L717)
- [HolographOperator.sol#L804](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L804)
- [HolographOperator.sol#L814](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L814)

- [HolographFactory.sol#L181](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L181)

- [LayerZeroModule.sol#L256](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L256)
- [LayerZeroModule.sol#L299](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L299)

- [HolographERC20.sol#L396](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L396)
- [HolographERC20.sol#L449](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L449)

- [HolographERC721.sol#L417](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L417)

- [PA1D.sol#L665](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L665)


## 5. `++i/i++` should be `unchecked{++i}/unchecked{i++}` when it is not possible for them to overflow, as is the case when used in for-loop and while-loops

In Solidity 0.8+, there’s a default overflow check on unsigned integers. It’s possible to uncheck this in for-loops and save some gas at each iteration, but at the cost of some code readability, as this uncheck cannot be made inline.

- [HolographOperator.sol#L781](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L781)
- [HolographOperator.sol#L871](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L871)


## 6. splitting require() statements that use `&&` saves gas

this will have a large deployment gas cost but with enough runtime calls the split require version will be 3 gas cheaper

- [HolographOperator.sol#L857](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L857)

- [Holographer.sol#L166](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/Holographer.sol#L166)

- [HolographERC721.sol#L263](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L263)
- [HolographERC721.sol#L464](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L464)


## 7. using `calldata` instead of `memory` for read-only arguments in external functions saves gas

When a function with a memory array is called externally, the abi.decode() step has to use a for-loop to copy each index of the calldata to the memory index. Each iteration of this for-loop costs at least 60 gas (i.e. 60 * <mem_array>.length). Using calldata directly, obliviates the need for such a loop in the contract code and runtime execution. 

- [HolographBridge.sol#L162](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L162)

- [HolographOperator.sol#L240](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L240)

- [HolographFactory.sol#L143](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L143)
- [HolographFactory.sol#L192](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L192)

- [LayerZeroModule.sol#L158](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L158)

- [ERC721H.sol#L140](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC721H.sol#L140)

- [ERC20H.sol#L140](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC20H.sol#L140)

- [Holographer.sol#L147](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/Holographer.sol#L147)

- [HolographERC20.sol#L218](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L218)

- [HolographERC721.sol#L238](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L238)

- [PA1D.sol#L173](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L173)
- [PA1D.sol#L185](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L185)


## 8. abi.encode() is less efficient than abi.encodepacked()

- [HolographFactory.sol#L252](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L252)

- [HolographERC20.sol#L409](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L409)

- [HolographERC721.sol#L260](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L260)
- [HolographERC721.sol#L426](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L426)


## 9. require()/revert() strings longer than 32 bytes cost extra gas
Each extra memory word of bytes past the original 32 incurs an MSTORE which costs 3 gas

- [PA1D.sol#L411](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L411)
- [PA1D.sol#L435](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L435)


## 10. bytes constants are more efficient than string constants
If data can fit into 32 bytes, then you should use bytes32 datatype rather than bytes or strings as it is cheaper in solidity.

- [HolographERC20.sol#L171](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L171)
- [HolographERC20.sol#L176](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L176)

- [HolographERC721.sol#L155](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L155)
- [HolographERC721.sol#L160](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L160)

- [PA1D.sol#L142](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L142)
- [PA1D.sol#L143](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L143)
- [PA1D.sol#L144](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L144)
