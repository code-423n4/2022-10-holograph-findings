### [G-01] Loop can be more optimizable

There are 15 instances of this issue:

> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L781
> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L871
> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L307
> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L323
> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L340
> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L356
> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L394
> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L414
> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L432
> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L437
> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L454
> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L474
> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L357
> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L716
> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L564


#### Recommended Mitigation
. Should not initialize uint with default value i.e uint i=0 **TO** uint i;
. Should use ++i instead i++
. Should uncheck i++



### [G-02] USE CUSTOM ERRORS RATHER THAN REVERT()/REQUIRE() STRINGS TO SAVE GAS

Custom errors are available from solidity version 0.8.4. Custom errors save ~50 gas each time they’re hit by avoiding having to allocate and store the revert string. Not defining the strings also save deployment gas 

There are 1 instances of this issue:

> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L214

#### Recommended Mitigation
require() with long error message length more than 32bytes can replace with custom error message to save gas.



### [G-03] FUNCTIONS GUARANTEED TO REVERT WHEN CALLED BY NORMAL USERS CAN BE MARKED PAYABLE

If a function modifier such as onlyOwner is used, the function will revert if a normal user tries to pay the function. Marking the function as payable will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided. 

There are 26 instances of this issue:

> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L285
> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L969
> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L989
> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L1009
> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L1029
> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L1049
> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L199
> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L452
> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L472
> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L502
> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L522
> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L280
> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L300
> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L320
> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L340
> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L360
> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L380
> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L441
> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L470
> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L471
> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L399
> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L417
> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L500
> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L508
> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L520
> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L577




### [G-04] Assigning default value to uint 

There are 3 instances of this issue:

> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L380
> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L310
> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L311



### [G-05] Using Bools for storage incurs overhead

Booleans are more expensive than uint256 or any type that takes up a full word because each write operation emits an extra SLOAD to first read the slot's contents, replace the bits taken up by the boolean, and then write back. This is the compiler's defense against contract upgrades and pointer aliasing, and it cannot be disabled. 

There are 2 instances of this issue:

> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L198
> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L196

#### Recommended Mitigation

Use uint256(1) and uint256(2) for true/false to avoid a Gwarmaccess (100 gas) for the extra SLOAD, and to avoid Gsset (20000 gas) when changing from ‘false’ to ‘true’, after having been ‘true’ in the past



### [G-06] <x> += <y> costs more gas than <x> = <x> + <y> for state variables  

There are 11 instances of this issue:

> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L378
> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L382
> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L834
> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L1177
> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L378
> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L382
> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L834
> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L1175
> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L328
> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L685
> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L686



### [G-07] Divide by 2 should be bit-shift. If possible try to use bit-shift in replace of multiplication and division

There are multiple instances of this issue::


### [G-08] Unchecking those arithmetic operation which can't overflow or underflow can save gas

Solidity version 0.8+ comes with implicit overflow and underflow checks on unsigned integers. When an overflow or an underflow isn’t possible (as an example, when a comparison is made before the arithmetic operation), some gas can be saved by using an unchecked block: https://docs.soliditylang.org/en/v0.8.10/control-structures.html#checked-or-unchecked-arithmetic 

There are 3 instances of this issue ::

> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L433
> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L520
> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L779



### [G-09] Some public function could be external for saving gas

There is 14 instance of this issue :: 

> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L638
> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L643
> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L656
> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L273
> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L297
> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L301
> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L306
> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L310
> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L314
> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L318
> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L322
> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L326
> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L347
> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L420
> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L492



### [G-10] Split require() statement that use && operator can help in save gas

There is 1 instances of this issue:

> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/Holographer.sol#L166
