## USE CUSTOM ERROR INSTEAD OF REVERTING WITH STRINGS
In solidity ^0.8.4 you can use custom error which are more efficient than reverting with string
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L163
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L148
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L205

## DO NOT EXPLICITLY INITIALIZE UINT WITH 0
uint by default is initialize with 0 so explicitly declaring it to 0 will cost gas
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L380
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L414
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L432
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L437

## `++jobNonce` COST LESS GAS THAN `jobNonce+1`
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L393

## USE UINT256 INSTEAD OF BOOLS
Uint256 is more efficient than bools so using uint256(0) for true and uint256(1) will save gas

## `>0` COST MORE GAS THAN `!=0` FOR UINT IN REQUIRE
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L309
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L350

## `<X> +=/-= <Y>` COST MORE GAS THAN `<X> = <X> +/- <Y>`
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L378
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L382
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L834

## USING UNCHEKED BLOCK FOR OPERATION THAT CANNOT UNDERFLOW/OVERFLOW SAVES GAS
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L433

## `--pod` COST LESS GAS THAN `pod--`
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L760

## USING UNCHECK BLOCK IN EVERY LOOP FOR `i++/i--` SAVES GAS
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L437
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L432
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L414
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L394
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L454

## `<array>.length` should not be looked up in every loop of a `for-loop`
CACHING LENGTH WILL SAVE GAS
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L437
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L432
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L454