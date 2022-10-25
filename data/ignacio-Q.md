USE CALL() INSTEAD OF TRANSFER() ON AN ADDRESS PAYABLE CAN SAVE GAS
The claimer smart contract does implement a payable fallback which uses more than 2300 gas unit.
The claimer smart contract implements a payable fallback function that needs less than 2300 gas units but is called through proxy, raising the call’s gas usage above 2300.
Additionally, using higher than 2300 gas might be mandatory for some multisig wallets.
Whenever the user either fails to implement the payable fallback function or cumulative gas cost of the function sequence invoked on a native token transfer exceeds 2300 gas consumption limit the native tokens sent end up undelivered and the corresponding user funds return functionality will fail each time.

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L596

Recommended Mitigation
I recommend using call() instead of transfer().


reference  :
https://code4rena.com/reports/2022-04-backd/#m-01-call-should-be-used-instead-of-transfer-on-an-address-payable