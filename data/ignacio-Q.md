# USE CALL() INSTEAD OF TRANSFER() ON AN ADDRESS PAYABLE 
The claimer smart contract does implement a payable fallback which uses more than 2300 gas unit.
The claimer smart contract implements a payable fallback function that needs less than 2300 gas units but is called through proxy, raising the call’s gas usage above 2300.
Additionally, using higher than 2300 gas might be mandatory for some multisig wallets.
Whenever the user either fails to implement the payable fallback function or cumulative gas cost of the function sequence invoked on a native token transfer exceeds 2300 gas consumption limit the native tokens sent end up undelivered and the corresponding user funds return functionality will fail each time.

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L596

Recommended Mitigation
I recommend using call() instead of transfer().


reference  :
https://code4rena.com/reports/2021-09-sushimiso/#m-01-use-of-transfer-instead-of-call--to-send-eth
https://code4rena.com/reports/2022-04-backd/#m-01-call-should-be-used-instead-of-transfer-on-an-address-payable
https://code4rena.com/reports/2022-05-bunker/#m-03-call-should-be-used-instead-of-transfer-on-an-address-payable

# LACK OF REENTRANCY GUARDS ON EXTERNAL FUNCTIONS
The following external functions within the mover contracts contain function calls (e.g. safeTransferFrom, safeTransfer) that pass control to external contracts. Additionally, if ERC777 tokens are being used within an order, it contains various hooks that will pass the execution control to the external party.
No re-entrancy attacks that could lead to loss of assets were observed during the assessment.

The following shows examples of function call being made to an external contract
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L889

It is recommended to follow the good security practices and apply necessary reentrancy prevention by utilizing the nonReentrant modifier from Openzeppelin Library to block possible re-entrancy.
