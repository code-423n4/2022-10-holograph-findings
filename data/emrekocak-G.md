## BYTES CONSTANT ARE CHEAPER THAN STRING CONSTANTS

If the string can fit into 32 bytes, then `bytes32` is cheaper than `string`. `string` is a dynamically sized-type, which has current limitations in Solidity compared to a statically sized variable. This means extra gas is spent upon deployment and the constant is read every time.

Instances include:
[`enforcer/PA1D.sol:43`](https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/PA1D.sol#L43)
[`enforcer/PA1D.sol:44`](https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/PA1D.sol#L44)
[`enforcer/PA1D.sol:45`](https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/PA1D.sol#L45)

## Use assembly to check for address(0)  
Saves 6 gas per instance if using assembly to check for address(0)  
e.g.  
  
```  
assembly {  
 if iszero(_addr) {  
  mstore(0x00, "zero address")  
  revert(0x00, 0x20)  
 }  
}  
```  
  
Instances include:
[`HolographOperator.sol:234`](https://github.com/code-423n4/2022-10-holograph/blob/main/src/HolographOperator.sol#L234)
[`enforcer/HolographERC20.sol:521`](https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC20.sol#L521)  
[`enforcer/HolographERC20.sol:522`](https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC20.sol#L522)  
[`enforcer/HolographERC20.sol:528`](https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC20.sol#L528)  
[`enforcer/HolographERC20.sol:585`](https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC20.sol#L585)  
[`enforcer/HolographERC20.sol:596`](https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC20.sol#L596)  
[`enforcer/HolographERC20.sol:597`](https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC20.sol#L597)  
[`enforcer/HolographERC721.sol:320`](https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC721.sol#L320)
[`enforcer/HolographERC721.sol:540`](https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC721.sol#L540)
[`enforcer/HolographERC721.sol:590`](https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC721.sol#L590)
[`enforcer/HolographERC721.sol:717`](https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC721.sol#L717)
[`enforcer/HolographERC721.sol:771`](https://github.com/code-423n4/2022-10-holograph/blob/main/src/enforcer/HolographERC721.sol#L771)

## Write direct outcome, instead of performing mathematical operations  
Instances include:
[`HolographOperator.sol:157`](https://github.com/code-423n4/2022-10-holograph/blob/main/src/HolographOperator.sol#L157)
[`module/LayerZeroModule.sol:175`](https://github.com/code-423n4/2022-10-holograph/blob/main/src/module/LayerZeroModule.sol#L175)
[`module/LayerZeroModule.sol:194`](https://github.com/code-423n4/2022-10-holograph/blob/main/src/module/LayerZeroModule.sol#L194)

## Splitting require() statements that use && saves gas

Saves 16 gas per instance. If you're using the Optimizer at 200, instead of using the && operator in a single require statement to check multiple conditions, multiple require statements with 1 condition per require statement should be used to save gas:
[`HolographOperator.sol:758`](https://github.com/code-423n4/2022-10-holograph/blob/main/src/HolographOperator.sol#L758)