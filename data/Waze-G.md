## [G-01] Sort Solidity operations using short-circuit mode
Short-circuiting is a solidity contract development model that uses OR/AND logic to sequence different cost operations. It puts low gas cost operations in the front and high gas cost operations in the back, so that if the front is low If the cost operation is feasible, you can skip (short-circuit) the subsequent high-cost Ethereum virtual machine operation.

    //f(x) is a low gas cost operation 
    //g(y) is a high gas cost operation 

    //Sort operations with different gas costs as follows 
    f(x) || g(y) 
    f(x) && g(y)

## code snipped
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/src/HolographBridge.sol#L105
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/src/HolographBridge.sol#L156
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/src/HolographBridge.sol#L253

## recommendation
change from

      require(_registry().isHolographedContract(holographableContract) || address(_factory()) == holographableContract,
      "HOLOGRAPH: not holographed"
to

      require(address(_factory()) == holographableContract || _registry().isHolographedContract(holographableContract) ,
      "HOLOGRAPH: not holographed"

## [G-02] Use private rsther than for constants.
Using private rather than public for constants, saves gas 10 gas per instances.

## code snipped
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/src/HolographOperator.sol#L30
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/src/enforcer/Holographer.sol#L20-L36
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/src/enforcer/PA1D.sol#L25-L45
## recommendation
use private for constant variable states.

## [G-03] Multiple address mappings can be combined into a single mapping of an address to a struct
Depending on the state and size of the species, this can avoid Gsset (20,000 gases) per mapping when combined. Subsequent reads and writes can also be cheaper when a function requires both values and they both fit in the same storage slot. Of course this saves storage slots for mapping.
If both fields are accessed in the same function, this can save ~42 gas per access because there is no need to recalculate the keccak256 hash key and the stack operations associated with the computation.

## code snipped
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/src/HolographOperator.sol#L119-L129

## recommendation
 combined multiple mapping into a single mapping of an address to a struct

## [G-04] x -= y (x += y) costs more gas than x = x – y (x = x + y) for state variables
x -= y costs more gas than x = x – y for state variables.
## code snipped
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/src/HolographOperator.sol#L279
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/src/HolographOperator.sol#L283
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/src/HolographOperator.sol#L735
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/src/HolographOperator.sol#L1076
## recommendation
split x -= y (x += y) to x = x – y (x = x + y).

## [G-05] Use ++index  instead of index++ to increment a loop counter
Due to reduced stack operations, using ++index saves 5 gas per iteration.
## code snipped
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/src/HolographOperator.sol#L421
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/src/HolographOperator.sol#L661
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/src/enforcer/HolographERC20.sol#L614
## recommendation
Use ++index to increment a loop counter.

## [G-06] Use require instead && (split require)
## code snipped
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/src/HolographOperator.sol#L758
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/src/enforcer/Holographer.sol#L67
## recommendation
When using logical conjunction (&&), make sure to order your functions correctly for optimal gas usage. we reccomend to use require instead of logical conjunction (&&)  for save gas cost.

## [G-07] Use calldata instead memory
## code snipped
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/src/HolographFactory.sol#L93
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/src/module/LayerZeroModule.sol#L59
## recommendation
In the external functions where the function argument is read-only, the function() has an inputed parameter that using memory, if this function didnt change the parameter, its cheaper to use calldata then memory.

## [G-08] Use abi.encodePackage instead of abi.encode is more efficient 
abi.encode encodes its parameters using the ABI specification. ABI is designed to make calls to contracts. Parameters filled up to 32 bytes. If you make calls to contracts you should probably use abi.encode if you are dealing with more than one dynamic data type as it prevents collisions.
abi.encodePacked: abi.encodePacked encodes its parameters using the minimal space required by the type. Encoding that uint8 will use 1 byte. This is used when you want to save space, and not call contracts. Takes any type of data and any amount of input. 
so from the explanation above it will be more efficient to use abi.encodePacked.
## code snipped
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/src/HolographFactory.sol#L153
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/src/enforcer/HolographERC20.sol#L372
## recommendation
use abi.encodePacked instead abi.encode

## [G-09] Use storage instead memory
When fetching data from a storage location, assigning the data to a memory variable causes all fields of the struct/array to be read from storage, which incurs a Gcoldsload (2100 gas) for each field of the struct/array. If the fields are read from the new memory variable, they incur an additional MLOAD rather than a cheap stack read. Instead of declearing the variable with the memory keyword, declaring the variable with the storage keyword and caching any fields that need to be re-read in stack variables, will be much cheaper, only incuring the Gcoldsload for the fields actually read. The only time it makes sense to read the whole struct/array into a memory variable, is if the full struct/array is being returned by the function, is being passed to a function that requires memory, or if the array/struct is being read from another memory array/struct
## code snipped
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/src/enforcer/PA1D.sol#L284
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/src/enforcer/PA1D.sol#L285
https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/src/enforcer/PA1D.sol#L307
## recommendation
use storage instead memory