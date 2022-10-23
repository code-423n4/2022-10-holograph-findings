#Issue 1: Solidity Version for all files in scope

Upgrade solidity version to at least Solidity 0.8.4 in all files in scope . Using newer versions and the optimiser gives gas optimisations and safety checks for free.




#Issue 2:  Default value of variable is 0 so there is no need to initialize it with 0 as it would cost more gas to execute.

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/src/HolographOperator.sol#L211-L212

#Issue 3: Unindexed values in event function

https://github.com/code-423n4/2022-10-holograph/blob/f8c2eae866280a1acfdc8a8352401ed031be1373/src/enforcer/PA1D.sol#L54

