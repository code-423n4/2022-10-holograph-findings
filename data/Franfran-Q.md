# Comments

Should update to `@dev bytes32(uint256(keccak256('eip1967.Holograph.baseGas')) - 1)`
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L140

Should update to `@dev bytes32(uint256(keccak256('eip1967.Holograph.gasPerByte')) - 1)`
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L146

# Logic

Missing address(0) check for the signature recover. This is reported as Low finding because they are no heavy side effects:
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L220
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L333-L334
As an address(0) return from `ecrecover` may mean that the recover failed.

# Misc

Function that was let here for testing purposes. Not much issue because is has proper access control and the admin is initialized:
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L274-L294

This function can use the `view` keyword as it is not making any state change and is made to estimate gas usage that is mostly called off-chain in this context: 
https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L549
