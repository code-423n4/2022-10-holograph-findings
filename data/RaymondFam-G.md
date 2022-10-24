## Use Fixed-size `bytes32` instead of `string`
Fitting your data in fixed-size 32 byte words is much cheaper than using arbitrary-length types (string in this case). Remember that bytes32 uses less gas because it fits in a single EVM word. Typically, any fixed size variable in solidity is cheaper than dynamically sized ones. Here are some of the instances entailed:

https://github.com/code-423n4/2022-10-holograph/blob/main/src/HolographBridge.sol#L225
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L171-L176
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L142-L143

Please visit the following link for more details on favoring bytes32 over string:

https://ethereum.stackexchange.com/questions/3795/why-do-solidity-examples-use-bytes32-type-instead-of-string

## Unused Constructor
Empty `constructor()` that is not used can be removed to save deployment gas. Here are some of the instances entailed:

https://github.com/code-423n4/2022-10-holograph/blob/main/src/HolographBridge.sol#L56
https://github.com/code-423n4/2022-10-holograph/blob/main/src/HolographOperator.sol#L134
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L136
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L151

## Private/Internal Function Embedded Modifier Reduces Contract Size
Consider having the logic of a modifier embedded through an internal or private (if no contracts inheriting) function to reduce contract size if need be. For instance, the following instance of modifier may be rewritten as follows:

https://github.com/code-423n4/2022-10-holograph/blob/main/src/HolographBridge.sol#L48-L51

```
    function _onlyOperator() private view {
        require(msg.sender == address(_operator()), "HOLOGRAPH: operator only call");
    }

    modifier onlyOperator() {
        _onlyOperator();
        _;
    }
```
## Function Order Affects Gas Consumption
The order of function will also have an impact on gas consumption. Because in smart contracts, there is a difference in the order of the functions. Each position will have an extra 22 gas. The order is dependent on method ID. So, if you rename the frequently accessed function to more early method ID, you can save gas cost. Please visit the following site for further information:

https://medium.com/joyso/solidity-how-does-function-name-affect-gas-consumption-in-smart-contract-47d270d8ac92

## Activate the Optimizer
Before deploying your contract, activate the optimizer when compiling using “solc --optimize --bin sourceFile.sol”. By default, the optimizer will optimize the contract assuming it is called 200 times across its lifetime. If you want the initial contract deployment to be cheaper and the later function executions to be more expensive, set it to “ --optimize-runs=1”. Conversely, if you expect many transactions and do not care for higher deployment cost and output size, set “--optimize-runs” to a high number.

```
module.exports = {
  solidity: {
    version: "0.8.11",
    settings: {
      optimizer: {
        enabled: true,
        runs: 1000,
      },
    },
  },
};
```
Please visit the following site for further information:

https://docs.soliditylang.org/en/v0.5.4/using-the-compiler.html#using-the-commandline-compiler

Here's one example of instance on opcode comparison that delineates the gas saving mechanism:

for !=0 before optimization
PUSH1 0x00
DUP2
EQ
ISZERO
PUSH1 [cont offset]
JUMPI

after optimization
DUP1
PUSH1 [revert offset]
JUMPI

Disclaimer: There have been several bugs with security implications related to optimizations. For this reason, Solidity compiler optimizations are disabled by default, and it is unclear how many contracts in the wild actually use them. Therefore, it is unclear how well they are being tested and exercised. High-severity security issues due to optimization bugs have occurred in the past . A high-severity bug in the emscripten -generated solc-js compiler used by Truffle and Remix persisted until late 2018. The fix for this bug was not reported in the Solidity CHANGELOG. Another high-severity optimization bug resulting in incorrect bit shift results was patched in Solidity 0.5.6. Please measure the gas savings from optimizations, and carefully weigh them against the possibility of an optimization-related bug. Also, monitor the development and adoption of Solidity compiler optimizations to assess their maturity.

## Non-strict inequalities are cheaper than strict ones
In the EVM, there is no opcode for non-strict inequalities (>=, <=) and two operations are performed (> + = or < + =). As an example, consider replacing >= with the strict counterpart > in the following line of code:

https://github.com/code-423n4/2022-10-holograph/blob/main/src/HolographOperator.sol#L1070

```
    if (pod > _operatorPods.length - 1) {
```
Similarly, as an example, consider replacing <= with the strict counterpart < in the following line of code:

https://github.com/code-423n4/2022-10-holograph/blob/main/src/HolographOperator.sol#L764

```
      require(current < amount + 1, "HOLOGRAPH: bond amount too small");
```

## Unchecked SafeMath Saves Gas
"Checked" math, which is default in ^0.8.0 is not free. The compiler will add some overflow checks, somehow similar to those implemented by `SafeMath`. While it is reasonable to expect these checks to be less expensive than the current `SafeMath`, one should keep in mind that these checks will increase the cost of "basic math operation" that were not previously covered. This particularly concerns variable increments in for loops. When no arithmetic overflow/underflow is going to happen, `unchecked { ++i ;}` to use the previous wrapping behavior further saves gas just as in the for loop below as an example:

https://github.com/code-423n4/2022-10-holograph/blob/main/src/HolographOperator.sol#L682-L684

```
    for (uint256 i = 0; i < length;) {
      operators[i] = _operatorPods[pod][index + i];
          unchecked {
              ++i;
          }
    }
```
## Split Require Statements Using &&
Instead of using the `&&` operator in a single require statement to check multiple conditions, using multiple require statements with 1 condition per require statement will save 3 GAS per `&&`. Here are some of the instances entailed:

https://github.com/code-423n4/2022-10-holograph/blob/main/src/HolographOperator.sol#L758
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L464-L470
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/Holographer.sol#L166

## `||` Costs Less Gas Than Its Equivalent `&&`
Rule of thumb: `(x && y)` is `(!(!x || !y))`

Even with the 10k Optimizer enabled: `||`, OR conditions cost less than their equivalent `&&`, AND conditions.

As an example, the following code line may be rewritten as:

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L527

```
      if (!(msg.sender == _holograph().getBridge() || msg.sender == _holograph().getOperator())) {
```
## `uint256` Can be Cheaper Than `uint8` and Other Unsigned Integer Type of Smaller Bit Size
When dealing with function arguments or memory values, there is no inherent benefit because the compiler does not pack these values. Your contract’s gas usage may be higher because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size. The EVM needs to properly enforce the limits of this smaller type.

It is only more efficient when you can pack variables of uint8 into the same storage slot with other neighboring variables smaller than 32 bytes. Here are some of the instances entailed:

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L323
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L192

## += and -= Costs More Gas
`+=` generally costs 22 more gas than writing out the assigned equation explicitly. The amount of gas wasted can be quite sizable when repeatedly operated in a loop. As an example, the following line of code could be rewritten as:

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L328

```
      v = v + 27;
```
## Payable Access Control Functions Costs Less Gas
Consider marking functions with access control as `payable`. This will save 20 gas on each call by their respective permissible callers for not needing to have the compiler check for `msg.value`. Here are some of the instances entailed:

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L320-L324
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L340-L344
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L360-L364
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L380-L384
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L441-L445
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L470-L474

Report contents changed: ## calldata and memory
When running a function we could pass the function parameters as calldata or memory for variables such as strings, bytes, structs, arrays etc. If we are not modifying the passed parameter, we should pass it as calldata because calldata is more gas efficient than memory.

Calldata is a non-modifiable, non-persistent area where function arguments are stored, and behaves mostly like memory. However, it alleviates the compiler from the `abi.decode()` step that copies each index of the calldata to the memory index, bypassing the cost of 60 gas on each iteration. Here are some of the instances entailed:

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L193-L194
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L298

## Use Storage Instead of Memory for Structs/Arrays
A storage pointer is cheaper since copying a state struct in memory would incur as many SLOADs and MSTOREs as there are slots. In another words, this causes all fields of the struct/array to be read from storage, incurring a Gcoldsload (2100 gas) for each field of the struct/array, and then further incurring an additional MLOAD rather than a cheap stack read. As such, declaring the variable with the storage keyword and caching any fields that need to be re-read in stack variables will be much cheaper, involving only Gcoldsload for all associated field reads. Read the whole struct/array into a memory variable only when it is being returned by the function, passed into a function that requires memory, or if the array/struct is being read from another memory array/struct. Here are some of the instances entailed:

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L325

## Ternary Over `if ... else`
Using ternary operator instead of the if else statement saves gas. For instance the following code block may be rewritten as:

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L560-L564

```
    _getReceiver(tokenId) == address(0)
        ? {
            bps[0] = _getDefaultBp();
        } 
        : {
            bps[0] = _getBp(tokenId);
        }
```
## External costs less gas than public visibility
Functions not internally called may have its visibility changed to external to save gas. Here are some of the instances entailed:

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L471
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L497
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L507
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L517
