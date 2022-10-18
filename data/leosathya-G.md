# [G-01] Loop can be more optimizable

There are 1 instances of this issue:

> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L781
> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L871
> ** File :  **  =>
> ** File :  **  =>
> ** File :  **  =>
> ** File :  **  =>
> ** File :  **  =>

#### Recommended Mitigation
. Should not initialize uint with default value i.e uint i=0 **TO** uint i;
. Should use ++i instead i++
. Should uncheck i++





### [G-02] USE CUSTOM ERRORS RATHER THAN REVERT()/REQUIRE() STRINGS TO SAVE GAS

Custom errors are available from solidity version 0.8.4. Custom errors save ~50 gas each time they’re hitby avoiding having to allocate and store the revert string. Not defining the strings also save deployment gas 

There are 1 instances of this issue:

> ** File :  **  =>
> ** File :  **  =>
> ** File :  **  =>
> ** File :  **  =>
> ** File :  **  =>
> ** File :  **  =>
> ** File :  **  =>




### [G-03] FUNCTIONS GUARANTEED TO REVERT WHEN CALLED BY NORMAL USERS CAN BE MARKED PAYABLE

If a function modifier such as onlyOwner is used, the function will revert if a normal user tries to pay the function. Marking the function as payable will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided. 

There are 1 instances of this issue:

> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L285
> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L969
> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L989
> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L1009
> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L1029
> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L1049
> ** File :  **  =>

#### Recommended Mitigation




### [G-04] Assigning default value to uint 

There are 1 instances of this issue:

> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L380
> ** File :  **  =>
> ** File :  **  =>
> ** File :  **  =>
> ** File :  **  =>
> ** File :  **  =>
> ** File :  **  =>

#### Recommended Mitigation




### [G-05] USING BOOLS FOR STORAGE INCURS OVERHEAD

Booleans are more expensive than uint256 or any type that takes up a full word because each write operation emits an extra SLOAD to first read the slot's contents, replace the bits taken up by the boolean, and then write back. This is the compiler's defense against contract upgrades and pointer aliasing, and it cannot be disabled. 

There are 1 instances of this issue:

> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L198
> ** File :  **  =>
> ** File :  **  =>
> ** File :  **  =>
> ** File :  **  =>
> ** File :  **  =>
> ** File :  **  =>

#### Recommended Mitigation

Use uint256(1) and uint256(2) for true/false to avoid a Gwarmaccess (100 gas) for the extra SLOAD, and to avoid Gsset (20000 gas) when changing from ‘false’ to ‘true’, after having been ‘true’ in the past



### [G-06] <x> += <y> costs more gas than <x> = <x> + <y> for state variables  

There are 1 instances of this issue:

> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L378
> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L382
> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L834
> ** File :  **  => https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L1177
> ** File :  **  =>
> ** File :  **  =>
> ** File :  **  =>

#### Recommended Mitigation



