1. Unused code
The following code is not used in the HolographTreasury contract:https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographTreasury.sol#L251-L273

2. Functions should be declared external
The admin and setAdmin functions in Admin contract should be declared external.

3. Unused return
The reversedBridgeOutRequest function in the HolographBridge contract does not process the return value when calling the bridgeOut function of the holographableContract contract.
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L305
The getBridgeOutRequestPayload function in the HolographBridge contract does not process the return value when calling its own reversedBridgeOutRequest function.
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L360

4. Missing zero address check
The zero address check for holograph and sourceContract is missing in the init function of the Holographer contract.

5. Missing event trigger
It is recommended to trigger the corresponding event when the resetOperator function in the HolographOperator contract modifies the parameters.

6. The return value of the token transfer function is not checked
The return value of the token transfer function is not checked in the executeJob function in the HolographOperator contract.
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L400



