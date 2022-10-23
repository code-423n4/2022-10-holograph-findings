## USE CALLDATA INSTEAD OF MEMORY

When a function with a `memory` array is called externally, the `abi.decode()` step has to use a for-loop to copy each index of the `calldata` to the `memory` index. Each iteration of this for-loop costs at least 60 gas (i.e. `60 * <mem_array>.length`). Using `calldata` directly, obliviates the need for such a loop in the contract code and runtime execution.

When arguments are read-only on external functions, the data location should be `calldata`
Instances:
-Holographer.sol lines 147, 218
-HolographERC20.sol lines 499, 524, 641
-HolographERC721 llines 238, 456, 620

## <ARRAY>.LENGTH SHOULD NOT BE LOOKED UP IN EVERY LOOP OF A FOR-LOOP

Reading array length at each iteration of the loop consumes more gas than necessary.
In the best case scenario (length read on a memory variable), caching the array length in the stack saves around 3 gas per iteration. In the worst case scenario (external calls at each iteration), the amount of gas wasted can be massive.

Consider storing the array’s length in a variable before the for-loop, and use this new variable instead

Instances:
-HolographOperator.sol line 871
-HolographERC20.sol line 564
-PA1D.sol lines 432, 437, 454, 474


## <X> += <Y> COSTS MORE GAS THAN <X> = <X> + <Y> FOR STATE VARIABLES

Instances:
-HolographOperator.sol lines 378, 382, 834
-HolographERC20.sol lines 685, 686, 702, 633





## `++I` INSTEAD OF `I++` (OR USE ASSEMBLY WHEN APPLICABLE)

Use `++i` instead of ` i++`. This is especially useful in for loops but this optimization can be used anywhere in your code. 

Instances:
-HolographOperator.sol lines 781, 871
-HolographERC20.sol lines 564, 713
-HolographERC721.sol lines 357, 716, 779
-PA1D.sol lines 307, 323, 340, 356, 394, 414, 432, 437, 454, 474

## USE MULTIPLE `REQUIRE()` STATMENTS INSTEAD OF `REQUIRE(EXPRESSION && EXPRESSION && ...)`

Instances:
-Holographer.sol line 166
-HolographERC721.sol line 464

## IT COSTS MORE GAS TO INITIALIZE VARIABLES WITH THEIR DEFAULT VALUE THAN LETTING THE DEFAULT VALUE BE APPLIED

If a variable is not set/initialized, it is assumed to have the default value (0 for uint, false for bool, address(0) for address…). Explicitly initializing it with its default value is an anti-pattern and wastes gas.

As an example: for (uint256 i = 0; i < numIterations; ++i) { should be replaced with for (uint256 i; i < numIterations; ++i) {

Instances:
-HolographBridge.sol lines 380, 310, 311, 781
-HolographERC721.sol line 357, 716
-PA1D.sol lines 307, 323, 340, 356, 394, 414, 432, 437, 454, 474

## `>=` COSTS LESS GAS THAN `>`

The compiler uses opcodes `GT` and `ISZERO` for solidity code that uses `>`, but only requires `LT` for `>=`, which saves 3 gas

Instances:
-HolographBridge line 218 in this case is cheaper to use `hTokenValue >= 1`
-HolographOperator.sol lines 309, 350, 363, 393, 519, 1126
-HolographERC712.sol lines 815


## `++I/I++` SHOULD BE `UNCHECKED{++I}`/`UNCHECKED{I++}` WHEN IT IS NOT POSSIBLE FOR THEM TO OVERFLOW, AS IS THE CASE WHEN USED IN FOR- AND WHILE-LOOPS

The unchecked keyword is new in solidity version 0.8.0, so this only applies to that version or higher, which these instances are. This saves 30-40 gas per loop
`
   for (uint256 i = 0; i < orders.length; /** NOTE: Removed i++ **/ ) {
           // Do the thing
           // Unchecked pre-increment is cheapest
           unchecked { ++i; }   
}  `

Instances:
-HolographOperator.sol lines 781, 871
-HolographERC20.sol lines 564
-HolographERC721.sol lines 357, 716, 779
-PA1D.sol lines 307, 323, 340, 356, 394, 414, 432, 437, 454, 474

## USE CUSTOM ERRORS RATHER THAN REVERT()/REQUIRE() STRINGS TO SAVE GAS
Custom errors are available from solidity version 0.8.4. Custom errors save ~50 gas each time they’re hitby avoiding having to allocate and store the revert string. Not defining the strings also save deployment gas



