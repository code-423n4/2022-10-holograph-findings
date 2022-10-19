## Code

Admin.admin() - https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/abstract/Admin.sol#L117-L119

Admin.setAdmin(address) - https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/contracts/abstract/Admin.sol#L127-L131

## Description

public functions that are never called by the contract should be declared external, and its immutable parameters should be located in calldata to save gas.

## Recommendation

Use the external attribute for functions never called from the contract, and change the location of immutable parameters to calldata to save gas.